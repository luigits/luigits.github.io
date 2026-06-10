# Row

[Row 官方文档](https://flet.dev/docs/controls/row/)
行布局组件，在使用上和 [](Column.md) 区别不大。

在表现上 Column 会按照列的方式纵向排列放入的组件；Row 会按照排的方式横向排列放入的组件。

在属性上 Row 与 Column 相同，使用 `spacing` 属性进行调整组件内部的间距，没有 `padding` 属性，除此之外没有其他区别。

使用 Column 的部分文档内容用作参考，也可以直接查看[](Column.md)

<include from="Column.md" element-id="column-attribute"/>

| 属性                 | 含义                                                                     | 示例                                                               |
|--------------------|------------------------------------------------------------------------|------------------------------------------------------------------|
| controls           | 存入容器的组件                                                                | controls: list[Control] = [login_button,sigin_button]            |
| spacing            | 子控件之间的间距                                                               | spacing=20,                                                      |
| tight              | 是否占据所有可用的水平空间 (True)，<br/>只占据其子控件所需的空间 (False)。<br/>当 warp 为 False 时有效 | tight: bool = False                                              |
| vertical_alignment | 定义子控件应如何垂直放置                                                           | vertical_alignment: MainAxisAlignment = "START",  # type: ignore |
| alignment          | 定义子控件的水平放置方式。                                                          | alignment: MainAxisAlignment = "START",  # type: ignore          |
