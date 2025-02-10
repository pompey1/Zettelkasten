202409301506
Status: #idea
Tags: [[UI]]
# Slate
# TAttribute
Slate 中广泛使用的成员变量封装，能够支持动态绑定的功能
``` C++
// 示例：在 Slate 小部件中使用 TAttribute  
class SMyWidget : public SCompoundWidget  
{  
public:  
    SLATE_BEGIN_ARGS(SMyWidget) {}  
        SLATE_ATTRIBUTE(FText, LabelText)  
    SLATE_END_ARGS()  
  
    void Construct(const FArguments& InArgs)  
    {  
        LabelText = InArgs._LabelText;  
  
        ChildSlot  
        [  
            SNew(STextBlock)  
            .Text(LabelText)  
        ];  
    }  
  
private:
	// LabelText 可以在实际创建组件的时候再赋值
    TAttribute<FText> LabelText;  
};
```

# Persona Editor
EngineLoop::Tick -> 

---
# References