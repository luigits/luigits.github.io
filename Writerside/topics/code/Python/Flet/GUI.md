# GUI

<secondary-label ref="global-construct"/>


本页面记录和图形化用户界面相关的内容

flet 有视图的概念，系统默认提供一个视图，当存在多个视图时，会显示在排列中的最后一个视图:`[view1,view4,view3,view2]`,此时显示的是
`view2`,而非 view4
> 思考：切换窗口能否通过 View 的方式进行切换？
> 
> 关系：在 flet 中 view 是 window 的一部分，可以认为 view 是所属窗口的顶级目录
> 
> 导航栏只是定义在当前视图的 appbar 中，切换视图后需要重新定义