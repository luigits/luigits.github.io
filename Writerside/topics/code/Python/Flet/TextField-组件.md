<show-structure depth="3"/>

# TextField 组件

TextField 组件是 GUI 交互的一大重点，因此在修改后会形成不同形式的文本框：密码框、多行文本框等组件

## 属性

```python
textField = ft.TextField(
	label="标题",  # 输入框标题
	value="默认文本",  # 输入框默认文本，需要大部分情况下需要删除
	hint_text="提示文本",  # 输入框提示文本，不影响新内容
	width=100,  # 组件宽度
	autofocus=True,  # 是否自动获取光标焦点，value 是否有值并不影响获取光标
	disabled=False,  # 是否将组件置于不可编辑状态，置灰，无法修改
	read_only=False,  # 是否只读，可选中，可复制，不可修改
	max_length=100,  # 最大字符数，输入框可接受的最大字符数量，超出部分输入框变红。默认值为 python 能接受的最大值
	text_align="CENTER",  # type: ignore
	shift_enter=True,  # 换行是 enter 还是 shift+enter
	
	# 设置为密码框
	password=False,  # 是否为密码框
	can_reveal_password=False,  # 是否可显示密码
	
	# 设置为多行文本框
	multiline=True, # 是否允许输入多行文本
	height=600,  # 组件的高度，会占用父组件的高度
	min_lines=4,  # 高度允许的情况下输入框显示的最小行数
	max_lines=10,  # 高度允许的情况下输入框显示的最大行数，行数不够会扩充，直到满足最大行数
)
```

### 注意

<tldr>

- 启用 `password` 属性，TextField 组件将会设置为密码框。

  配合 `can_reveal_password` 是现代标准的密码框效果
- 启用 `multiline` 属性，TextField 组件将会设置为多行文本框。

  搭配 `min_lines` 和 `max_lines` 效果会更好
- 如果要使用 `on_submit` 参数调用函数，必须启用 `shift_enter` 属性，否则无效果

</tldr>

## 回调函数

TextField 组件有 4 中回调函数，分别代表

- 获取光标前 (编辑前): `on_focus`
- 获取光标时 (编辑中): `on_change`
- 失去光标时 (编辑结束后): `on_blur`
- 提交内容时 (回车提交): `on_submit`

```python
def onchange(e: ft.Event):
    """
    想法：回车后将输入框的内容发送到文本组件
    问题：不知道如何将文本组件的变量传入
    原因：回调函数形参不接受额外参数的函数
    """
    print(e.control.value)
    e.control.update()
    pass


def main(page: ft.Page):
    textField = ft.TextField(
        on_change=onchange,  # 当内容改变时调用
        on_blur=onblur,  # 当失去焦点时调用
        on_focus=onfocus,  # 当获得焦点时调用
        on_submit=onsubmit,  # 当回车提交内容时调用，且要求 shift_enter 设置为 true
    )
    page.add(textField)
```

<tldr>
使用回调函数修改其他组件会有一个隐藏的问题：组件注册顺序是否会发生变化

当前版本 (0.85) 是允许被修改的组件后于回调函数所在组件注册的，但无法保证此形式是否会在后续版本更迭

推荐：优先将被修改的组件注册到页面中，在注册使用回调函数的组件
</tldr>