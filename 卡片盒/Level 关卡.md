202409301523
Status: #moc
Tags:
# Level 关卡

# Tips & Tricks

## Level Designer
编辑地形时，可以考虑拖入一个 **Camera** 方便看玩家视角是否与设计目的符合
用不同的颜色标记来提示这里的视觉目的，记得加上特殊材质提示美术这一段只是开发者用素材
可以适当添加一些细节方便美术理解，比如断木，可以适当加一些圆锥形分叉
## Viewport 右上角
可以 Actor 切换本地坐标系和世界坐标系的 gizmo，对于旋转物体很实用
开启 **Surface Snapping** 可以方便的把一些模型贴在其他模型上
键盘上按下 **End** 键 ，可以使悬浮的模型贴近最近的地面
按住 **V** （Vertices）拖动 Actor，可以将选中的物体吸附到其他 Mesh 的顶点上
## Viewport 左上角
 **G** (Game View) 隐藏编辑器下的一些图标
 **P** 显示导航网格
 **H** 隐藏物体
 **Alt + R** 隐藏光源半径
 **Ctrl + H** 还原所有隐藏物体
 **F9** 快速截图，左上角菜单选择 **High Resolution Screenshot** 可以截更高分辨率的截图
 **Ctrl** + **鼠标中键** 拖动到上下左右可以自动切换成线框模式的不同方向试图，拖到左下方可以还原
## 地编
 选择物体右键 **Visiblity - Show Only Selected** 可以快速预览单个物体的效果，最好和光源一起选中
 选择物体右键 **Transform - Lock Actor Movement** 避免 Actor 被误移动
 
**Shift + E** 可以选取在 Level 中所有相同类型的资产

**Ctrl + E** 可以快速打开 Level 中物体对应的资产编辑界面

如果你在蓝图中有一个 **Vector** 类型变量，你可以选择勾选上 Instance Editable 和 Show 3D Widget 来可视化编辑这个变量

按住 **Alt** 键拖动模型，可以自动复制模型，也可以考虑设置 Paste Here 快捷键
**Alt** + **鼠标左键** 旋转（与 Maya 相同）
**Alt** + **鼠标右键** Camera 推拉（与 Maya 相同）
**鼠标右键 + WASD** 自由视角
**鼠标右键 + 滚轮** 调整移动速度
**鼠标右键 + C/Z** 快速推拉预览
**F** 键，设置 Actor 为焦点
按住 **shift** 移动可以让摄像机跟随选中的 Actor

点击 **T** 可以无视透明、半透明物体选中不透明物体，再次点击一次 **T** 还原

选中场景中的物体，**Ctrl + Shift + P** 可以将其当作照相机一样自由移动

打开 Landscape 模式，可以通过 import from file 导入高度图（png 格式）快速创造地形

Color Correction Regions 和 后处理体积的差别：CCR 不需要 Camera 放置在其中就能生效

通过 Env Light Mixer 可以统一处理场景中的所有环境光属性，包括平行光和天光

**Ctrl + L** 并且按住 **Ctrl** 拖动鼠标就可以预览场景的实时光照效果

（LightChannel，貌似 Nanite 不支持这个东西了）光源可以勾选特定的通道，物体也可以勾选特定的通道，这样可以控制该光源只作用于某个物体（恐怖游戏中高亮显示的效果）

利用 Modeling 模式的 Boolean 操作可以方便地在模型上打洞

Spine Mesh Actor 非常实用，你可以选择一个 Mesh，随便扭曲延长它
## 控制台
控制台指令 + ？ 可以查看控制台的 Help
控制台输入 Help 会自动根据引擎所有的控制台指令生成一个 html 文件用来查询 help
控制台输入 FreezeRendering 可以冻结当前的视锥剔除和遮挡裁剪，方便进行渲染方面的 debug
控制台输入 dumpTicks 可以看到当前所有的 TickEvent，你也可以直接在设置中禁止蓝图 Tick（一般没人这么干吧。。）
控制台输入 r.Editor.NewLevelGird2 可以切换控制 LevelGrid 渲染的材质球，你也可以直接在 EngineContent 中编辑 NewLevelGird2 这个材质球实现你自己想要的效果

---
# References