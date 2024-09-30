202409291337
Status: #idea
Tags: [[UMG]]
# 双向绑定的实现
## ViewModel -> View
继承自 UMVVMViewModelBase 的类（已经继承 INotifyFieldValueChanged），实现 ViewModel 到 View 的绑定
## 反射信息
在UPROPERTY 宏中加入 FieldNotify 标识符，就会在 generated 文件中生成这个变量的 FieldID，之后调用 UE_MVVM_SET_PROPERTY_VALUE 宏的时候就会自动将这个 FieldID 作为变量传入广播函数中

拼接所有反射宏的结果
![[Pasted image 20240929135024.png]]

由于 FieldID 只在 C++ 中可见，所以只能在 C++ 中手动 AddFieldValueChangedDeleggate 来添加额外的绑定函数

## View -> ViewModel
View 也需要通过继承 INotifyFieldValueChanged 方法来实现 View 到 ViewModel 的绑定，为此 UVisual 定义了一系列 FieldNotify 宏，所以如果需要支持低版本，有必要修改引擎源码

# UI 的扩展机制
通过继承 UserWidgetExtension ，可以实现对 UserWiget 本身行为的扩充，同时也能使得代码更加模块化，在内存结构上 ，UUserWidgetExtension 的指针数组聚合在 UUserWidget 中，想要调用其中方法时需要遍历指针数组依次触发

> [!NOTE] Extension
> 只要继承自 UBlueprint 的资产都拥有自己的扩展

## MVVM 扩展机制的生效流程
### 添加 ViewModel 触发蓝图的扩展
当在 UMG 编辑界面试图添加 ViewModel 时，Slate 界面会触发 MVVMEditorSubsystem 的 RequestView 和 AddViewModel，调用 RequestView 方法时会转发到 UMVVMWidgetBlueprintExtension_View 的 RequestExtension，从而在蓝图资产中加入 UMVVMWidgetBlueprintExtension_View 类型的 Extension
### 编译扩展后的蓝图，触发 Class 的扩展
Extension 加入成功以后当此 UMG 蓝图编译结束（编辑器启动或者说加载 Level 的同时也会把加载到的蓝图类编译一遍）时都会触发 UMVVMWidgetBlueprintExtension_View 的HandleFinishCompilingClass，进而实例化一个 MVVMViewClass 对象加入到 WidgetBlueprintGeneratedClass 的 Extensions 中
### 初始化经过 Class 扩展的控件，触发 Object 扩展
然后 UserWidget 初始化的时候就会调用其蓝图反射信息遍历所有 MVVMViewClass 调用其初始化函数，从而实现对 UserWidget 自身 Extensions 的添加
## 四种关联方式
和传统的 UMG 属性绑定最大的区别就是可以自由选择数据源，甚至可以直接获取全局的 ViewModel 对象来实现对外部数据的绑定
1. Manual
2. CreateInstance
3. Global View Model Collection，也需要手动创建 ViewModel 然后再加入到 Collection 中，方便多个 Widget 共享一个 ViewModel
4. Property Path

# DataBinding 面板
## 四种绑定方式
1. One Time To Widget (1)
2. One Way To Widget <-
3. Two Way <->
4. One Way To View Model ->
## 转换函数
当转换函数无法支持一些嵌套、自定义的数据类型时，可以直接绑定一个自定义函数用作回调，类似 quabqi 之前 LUA MVVM 框架的 Callback

---
# References