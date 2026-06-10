# OutlinedButton 组件

OutlinedButton 是 Button 的子组件，OutlinedButton 是带外边框的组件。
与 OutlinedButton 类似的还有 FilledButton。

![](outlinedButton&filledButton.png)

## 属性

| 属性            | 说明                |
|---------------|:------------------|
| autofocus     | 自动获取焦点            |
| Clip_behavior | 内容将根据此选项被剪辑（或不剪辑） |
| content       | 表示自定义按钮内容的控件      |
| icon          | 在此按钮中显示的图标        |
| icon_color    | 图标颜色              |
| url           | 单击此按钮时要打开的 URL    |

```python
outlinedbutton=ft.OutlinedButton(
    content="OutlinedButton",  # 按钮的文字或其他控件
    icon=ft.Icons.LOGIN,  # 为按钮增加图标
    icon_color="#ff00ff",  # 为图标设置颜色
    url="https://github.com",  # 链接外部网络
    autofocus=True,  # 获取焦点
    # 还有一个 style 属性
)
```
## style 属性

style 属性是用来调整按钮样式的属性，但它是一个很神奇的属性，因为样式可能会因为嵌套位置导致无法正常渲染

这回让我们翻来覆去的打断自己的思绪，思考这个仅仅是外观样式的小细节。

```Python
style=ft.ButtonStyle(
    side=ft.BorderSide(
        width=1,
        color="#568FB7",
    ),
    shape=ft.RoundedRectangleBorder(radius=5),
)
```
为什么说它很神奇呢？因为 `shape` 属性的类型`RoundedRectangleBorder` 内同样有 side

> 建议在使用 style 进行调整样式时先将 shape 设置好，在进行其他样式的设置

<compare first-title="正确样式" second-title="错误样式" type="left-right">
<code-block lang="python">
style = ft.ButtonStyle(
    side=ft.BorderSide(
        width=1,
        color="#568FB7",
    ),
    shape=ft.RoundedRectangleBorder(radius=5),
)
</code-block>
<code-block lang="python" >
style = ft.ButtonStyle(
    shape=ft.RoundedRectangleBorder(
        side=ft.BorderSide(
            width=1,
            color="#568FB7",
        ),
        radius=5,
    ),
)
</code-block>
</compare>

## 回调函数

| 函数            | 说明                    |
|---------------|-----------------------|
| on_blur       | 当该按钮失去焦点时调用           |
| on_click      | 当用户单击此按钮时调用           |
| on_focus      | 当该按钮获得焦点时调用           |
| on_hover      | 当鼠标指针进入或存在此按钮的响应区域时调用 |
| on_long_press | 长按此按钮时调用              |

