# PopupMenuButton 组件

在表现形式上，和 Dropdown 比较像，都是点击弹出列表选择其中的选项。

`PopupMenuButton` 本质上是一个按钮，它是功能的集合，在本 instance 中叫它**下拉菜单**

`Dropdown` 本质上只是多个选项的集合，它并不将多个功能集合起来，在实际应用中更多的是多选一的信息提交，在本 instance 中叫它**下拉列表**。

<compare first-title="Dropdown" second-title="PopupMenuButton">
<code-block lang="Python"> 
ft.Dropdown(
    options=[
        ft.DropdownOption(text="item1"), # key或text必须有值
        ft.DropdownOption(text="item2"),
        ft.DropdownOption(text="item3"),
    ]
)
</code-block>
<code-block lang="Python">
ft.PopupMenuButton(
    content="Button", # 不设置content时显示三个点
    items=[
        ft.PopupMenuItem(content="item1"),
        ft.PopupMenuItem(),  # 下拉菜单的分隔符
        ft.PopupMenuItem(content="item2"),
    ],
)
</code-block>
</compare>

