202412181118
Status: #idea
Tags: [[UE 容器]]
# TArray
未设置第二个模板参数的 TArray 是 ContainerFwd 中设置了默认分配器的
``` C++
template<typename T, typename Allocator = FDefaultAllocator> class TArray;
```

FDefaultAllocator 是对 TSizedDefaultAllocator 的实例化
``` C++
using FDefaultAllocator = TSizedDefaultAllocator<32>;
```

TSizedDefaultAllocator 继承了 TSizedHeapAllocator < IndexSize >
但是在 TSizedDefaultAllocator 这个子类中只不过新增加了对 Typedef 的定义，实际内容还在
TSizedHeapAllocator 中（这个 Typedef 是用来做 TAllocatorTraits 的模板操作的，无底洞，暂时跳过）

TSizedHeapAllocator<32, FMemory> 就是 TArray 默认使用的内存分配器，可以看到他初始化一个 TArray 时分配的 Capacity 是 0
``` C++
SizeType GetInitialCapacity() const  
{  
    return 0;  
}
```

# 扩容方式
和 std::vector 差不多，容器满了以后额外分配一个更大的内存，将整个数组的数据拷贝到新内存上，之后释放旧的内存（InlineAllocator 不会释放 Inline 部分，因为本来就不会分配在连续的内存上）

# 构造函数
TArray 提供了各种拷贝和移动构造函数，包括指针+数量、TArrayView、初始化列表、其他的 TArray
其中指针+数量和 TArrayView 的构造函数本质上相同，都是从指针开始连续拷贝连续的内存到数组中

TArrayView 和 std::span 类似，是对指针和元素操作的轻量级封装，用来取代 C style 基于指针的连续内存访问，并且不会影响任何内存管理

# 增删改查
Add 相关方法对应 std::vector 的 push_back 和 emplace_back, 底层调用的都是 Emplace，Emplace 内部用完美转发和参数包实现了对任意元素类型构造函数的调用，GetRef 后缀的相关版本返回的是引用而不是索引，AddDefault 会调用默认无参数的构造函数，AddZero 会调用 memset 将内存值全部设为 0
大批量添加新元素时，可以使用 Append 和 += 运算符
Insert 相关方法对应 std::vector 的 insert 和 empalce，可以在指定位置插入元素

Remove 相关方法对应 std::vector 的 erase，但是没有迭代器范围删除的版本，作为替代，UE 在迭代器内部实现了一个 RemoveCurrent 方法用来在迭代中删除元素，更加易用
针对性能要求比较高的场景，可以考虑使用 RemoveAtSawp 来替代 Remove，这个方法会用数组末尾的元素替换掉即将删除元素的内存位置，避免大范围的元素移动

# 迭代器
UE 提供的迭代器能够做到移动位置、清空、跳到末尾、移除当前元素等操作，相较于 STL 提供的迭代器，UE 迭代器的 RemoveCurrent 可以直接在遍历中执行，不用担心 Index 越界问题，此外，迭代器结束是通过 operator bool 来判断的，而不是 stl 的 end() 函数

为了实现 range-for 语法，TArray 被迫实现了 stl 标准的 begin() 和 end() 函数，命名只能小写，注释中标注了不要直接使用

# 基于 TArray 的大/小根堆
相比 std::vector，TArray 将堆相关的函数全放在了容器内部，并没有放在通用算法库中，因为不支持其他容器


---
# References