<show-structure depth="3"/>

# Column

Column 是一个列容器，和窗口（page）很相似，都是自上而下的存储组件

## Padding 和 Spacing

在[](第一个界面.md)中我们见到了 <tooltip term="padding">padding</tooltip>
，现在我们来看另一个属性：<tooltip term="spacing">spacing</tooltip>

![](padding和spacing.png)

```python
# 给四个方位同时设置 padding
page.padding = 0 # 容器与组件之间的间距
# 为四个方位分别设置 padding
page.padding = ft.Padding(0, 0, 0, 0) # 同上面，分别代表左、上、右、下

# spacing 没有分别设置只有这一个方式
page.spacing = 100 # 组件与组件之间的间距

```

## 属性 {id="column-attribute"}

```python
column = ft.Column(
	controls=[ # controls 支持放入多个组件，按照顺序自上而下渲染
		ft.Container(width=100, height=100, bgcolor="#FF00FF"),
		ft.Container(width=100, height=100, bgcolor="#FFFF00"),
	],
	spacing=0,  # Column 内部组件之间的间距
	alignment="CENTER",  # type: ignore # Column 的 alignment 值是 ft.MainAxisAlignment 类型，因此可以使用此方式
	width=500,  # 设置水平方向对齐的时候必须设置宽度
	horizontal_alignment="CENTER",  # type: ignore # 水平对齐
	expand=True,  # 容器的宽高继承父容器的宽高，在不知高度的前提下，使用此方式效果会更好，其实可以看作是界面的自适应效果
)
```

## 布局嵌套

在 Container 中有 padding 属性；在 Column 中有 spacing 属性

因此可以让它们相互嵌套实现和 page 相同的效果 (能同时使用 padding 和 spacing)

- 在 Column 中实现 padding 的效果可以在其中嵌套一个 Container
- 在 Container 中实现 spacing 的效果可以在其中嵌套一个 Column

> 虽然可以一直套娃，但还是推荐在开始编写代码之前规划好布局设计，以避免额外系统开销

{style="warning"}