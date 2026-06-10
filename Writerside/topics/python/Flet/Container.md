<show-structure depth="3"/>

# Container

## 属性

Container 的 content 只能存放一个组件的实例

但可以通过嵌套的方式实现 container 中存放多个组件，例如 content 存放一个 [](Column.md) 实例

```python
container1 = ft.Container(
	content=None, # container 绑定的组件实例
	width=300, # container 的宽度，默认宽度是父容器的宽度
	height=300,# container 的高度，默认高度是 0
	# 在开发时可以通过设置 Border 和 bgcolor 来确认容器的大小和位置
	border=ft.Border.all(width=2, color="#FF00FF"), # container 的边框
	bgcolor="#0000FF" # 容器背景色，默认无色
	padding=10,  # 容器内部的间距设置，见于容器与内部组件之间，无法通过百分比设置
	
	# 对齐方式
	alignment=ft.Alignment.CENTER,  # 必须写全，否则不认。要求容器空间远大于内部组件空间，否则无效
	expand=True,  # container 继承父容器的    全部宽高
)
```

## 对齐方式

相较于 [](Text-组件.md) 提供的 3 种水平对齐方式，Container 提供了 9 种方位的对齐方式

|   | 左           | 中             | 右            |
|---|-------------|---------------|--------------|
| 上 | TOP_LEFT    | TOP_CENTER    | TOP_RIGHT    |
| 中 | CENTER_LEFT | CENTER        | CENTER_RIGHT |
| 下 | BOTTOM_LEFT | BOTTOM_CENTER | BOTTOM_RIGHT |

{style="both"}

要求 Container 的空间大小要大于内部组件的大小，否则无效

```python
def main(page: ft.Page):
    page.add(
        ft.Container(
            content=ft.Text(
                value="""ABCDEFGHIKL
ABCDEFGHIKL
ABCDEFGHIKL
ABCDEFGHIKL
ABCDEFGHIKL""",
                width=100,
                height=100,
            ),
            width=200,
            height=200,
            border=ft.Border.all(width=2, color="#FF00FF"),
            alignment=ft.Alignment.CENTER,
        )
    )
```

> 经过测试，当内部组件的空间被完全填充时，才能看出是否符合 `alignment` 设置的对齐方式
>
> <img src="alignment-1.png" thumbnail="true" alt="" height="100"/>
>
> 当 Container 的宽高是内部组件的 2 倍时，效果如上图
>
{style="note"}

## 注意

### 因更新导致的方法失效

flet 正在频繁更新，因此会有各种失效的方式

例如 `ft.border.all()` 就因此失效了

最简单的方式就是点开底层定义，因为在导入时是直接导入了整个 flet，因此可以省略掉文件，直接通过 `ft.Border` 来调用其中的函数、方法

> 点开 border.py 文件查看后发现文件内定义了 all 方法
>
> all 归属 Border 类，而 Border 类归属 BorderSide 类
>
> 在现在的情况下 all 的关系是 border 文件下 BorderSide 类定义的子类 Border
> (可能是错误的)

{style="note"}

> 在 flet 0.85 版本中
>
> Border 属性 ft.Border 和 ft.border.Border 都可以使用 all 方法，它们实际上指向了同一个方法

### Container 宽高

不设置宽高时，它的宽高等于内部组件的宽高
