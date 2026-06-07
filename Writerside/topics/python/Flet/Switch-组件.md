<show-structure depth="3"/>

# Switch 组件

开关组件，可用于切换主题等功能

## 属性

### 主要属性

```python
switch = ft.Switch(
	label="切换主题",  # 标题
	value=False,  # 组件默认值
	active_color="#2f94e6",  # 激活时的颜色
)
```

在 switch 中支持给标题添加样式，这使得可以像是 Text 组件一样进行编辑

### 其他属性

```python

switch = ft.Switch(
    ...
	label_text_style=ft.TextStyle( # 标题样式
        font_family="Maple Mono NF CN",  # 字体
        size=16, # 字体大小
        color="#2f94e6", # 字体颜色
        bgcolor="#ff00ff", # 背景色
        weight="BOLD",  # type: ignore # 字体粗细程度
        height=40, # 标题高度
        italic=True,  # 斜体
        decoration=ft.TextDecoration.UNDERLINE,  # 文本装饰，例如下划线之类的
        decoration_color="#FF0000",  # 装饰的颜色
        decoration_thickness=2,  # 装饰的厚度/像素高度
        decoration_style=ft.TextDecorationStyle.SOLID,  # 装饰的样式
        font_family_fallback=["Maple Mono NF CN", "Arial"],  # 字体的回退
        shadow=ft.BoxShadow(# 阴影
            spread_radius=0,  # 阴影的扩散半径
            blur_radius=10,  # 阴影的模糊半径
            color="#FF0000",  # 阴影的颜色
            offset=ft.Offset(0, 0),  # 阴影的偏移量
            blur_style="NOMAL",  # type: ignore  # 阴影的模糊样式，ft.BlurStyle.NORMAL
        ),
        letter_spacing=0.5,  # 字间距，例如 0.5 表示每个字符之间的间距为 0.5 个字符
        word_spacing=0.5,  # 单词间距，例如 0.5 表示每个单词之间的间距为 0.5 个字符
        ...
	)
)
```

## 回调函数

switch 的功能主要是通过回调函数来实现

- `on_change`: 当开关的状态改变时调用
- `on_focus`: 当控件获得焦点时调用
- `on_blur`: 当控件失去焦点时调用

```python
def onchange(e: ft.Event):
	if e.control.value:
		page.theme_mode = "LIGHT"  # type: ignore
	else:
		page.theme_mode = "DARK"  # type: ignore
	# page.update()  # 如果切换的效果不仅在当前组件，就要使用窗口级别的刷新，否则会很慢
	pass
```

> 此类影响到整个窗口甚至整个程序的回调函数刷新不要使用组件级的 `e.control.update()`
>
> 要使用页面级的 `page.update()` 