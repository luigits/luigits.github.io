<show-structure depth="3"/>

# Text 组件

Text 组件在 GUI 编程中是非常常见的基础组件

## 属性

```python
text = ft.Text(
	value="Hello Flet!",  # 文本显示的内容
	size=20,  # 文本的大小
	weight="BOLD",  # type: ignore  # 底层同样是同过单词定义的粗细
	italic=True,  # 斜体
	color="#00ff00",  # 字体颜色
	# bgcolor="#f0f0f0",  # 字体背景色，字体在被选中时没有蓝底的交互，但能被选中复制，一般情况下不要和可被选中同时设置
	selectable=True,  # 文本是否可被选中，默认为 false
	max_lines=4,  # 可显示的最大行数，超出部分需要滑动滚动条
	text_align="CENTER",  # type: ignore
	width=150,  # width 足够大时文本对齐方式才会明显，否则没有意义
	height=70,
)
```

除上述的属性外，还有其他属性相对不常用，可以在遇到时查阅 [官网文档](https://flet.dev/docs/controls/text/)

> 注册组件
> 
> 几乎所有的图形化库都会告诉你：
> 
> 当你创建了一个组件时，想要将该组件显示在窗口中就需要将组件与窗口绑定 (也叫注册该组件到窗口)
> ```python
> page.add(text) # 将定义好的文本组件注册到 page 中
> ```

### 注意

<tldr>

- 使用 `selectable` 时尽量不要搭配 `bgcolor`。 

  这会使无法通过肉眼确认文本被选中的情况，虽然能复制，但没有任何额外的显示帮助确定选中了哪些

- 使用 `on_tap` 时必须使用 `selectable`。

  无法选中文本则无法点击文本组件触发函数
</tldr>
 
## 回调函数

`on_tap` 是 Text 组件中的一个函数参数，当文本被点击时触发函数

```python
def ontap(e):
    print(e.control.value) # 在控制台打印获取的文本内容
    e.control.value = "我被点击了" # 点击组件后修改文本内容

    # page.update() # 若回调函数能够调用 page，则无需显式调用更新
    e.control.update()  # 更新调用该函数的组件
    pass
```
<deflist>
<def title="ontap(e)">

此处的 e 是 event 的意思，它的数据类型是 `ft.Event`

使用 e.control 可以获取当前组件的所有属性，在文本组件中可以通过此方式设置文本被点击后的各种属性
</def>
</deflist>

在该函数中出现了 *update()*，但能看出来它们归属两个不同作用域

### 作用域

#### page.update()

- 作用域：归属 window，属于窗口级别的刷新
- 适用：回调函数定义于窗口函数内部

```python
def main(page: ft.Page):
	...
    text = ft.Text(
		...
        on_tap=ontap,
    )

    # 注册组件到窗口
    page.add(text)
        
    def ontap(e:ft.Event):
	    ...
		page.update() # 一般情况下无需显式调用
```

#### e.control.update()

- 作用域：归属回调函数，属于组件级别的刷新
- 适用：回调函数定义于窗口函数外部

```python
def ontap(e:ft.Event):
	...
	e.control.update() # 需要显式调用

def main(page: ft.Page):
	...
    text = ft.Text(
        ...
        on_tap=ontap,
    )

    # 注册组件到窗口
    page.add(text)
```

> 切换主题色
> 
> 必须使用窗口级别的刷新，组件级别的刷新需要很久
> 
> ~~不会真的有人使用组件级别刷新而不是窗口级别刷新吧？~~
> 
> ~~哦，原来那个人是我，那没事了~~

{style="warning"}