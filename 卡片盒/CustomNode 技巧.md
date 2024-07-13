202406291936
Status: #idea
Tags: [[Material 材质]]
# CustomNode 技巧
在材质编辑器中输入
``` c++
retunr 1
```
最后生成的代码是
``` c++
MaterialFloat3 CustomExpression0(FMaterialPixelParameters Parameters)
{
	return 1;
}
```

所以在Custom Node 中输入
``` c++
return 1;
 }

float MyGlobalVariable;

int MyGlobalFunction(int x)
 {
     return x;
```
生成的 HLSL 将是
```cpp
MaterialFloat3 CustomExpression0(FMaterialPixelParameters Parameters)
 {
     return 1;
 }

float MyGlobalVariable;

int MyGlobalFunction(int x)
 {
     return x;
 }
```

这样我们就定义了一个很方便的全局函数和一个很方便的全局变量

---
# References
[UE4 HLSL 和 Shader 开发指南和技巧 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/66288908)