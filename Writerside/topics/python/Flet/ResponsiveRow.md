<show-structure depth="3"/>

# ResponsiveRow

`ResponsiveRow` 是**响应式**的，因为它可以根据屏幕（页面、窗口）大小的变化来调整其子组件的大小。

## 示例

### `col` 是常数值 {id="ResponsiveRow-H3-col_constant"}

```python
ft.ResponsiveRow([
    ft.Column(col=6, controls=[ft.Text("Column 1")]),
    ft.Column(col=6, controls=[ft.Text("Column 2")])
])
```

在上面的示例中，`col` 属性是一个常数，它意味着子组件将在任何屏幕大小下跨越 6 列 (虚拟列)
`col` 属性的设置要在 `ResponsiveRow` 的子组件中，而非在其本身。

> 默认情况下，`ResponsiveRow` 将内部一整行分割为 12 列。
> 如果需要修改虚拟列的数量，可以使用 columns 进行设置，要求必须大于 0

### `col` 断点 {id="ResponsiveRow-H3-col_breakpoint"}

```Python

ft.ResponsiveRow(
    controls=[
        ft.Container(
            content=ft.Text("Column 1"),
            padding=5,
            bgcolor=ft.Colors.YELLOW,
            col={
                ft.ResponsiveRowBreakpoint.XS: 12, # 或者设置"XS":12 也可以
                ft.ResponsiveRowBreakpoint.MD: 6,
                ft.ResponsiveRowBreakpoint.LG: 3,
            },
        ),
        ft.Container(
            content=ft.Text("Column 2"),
            padding=5,
            bgcolor=ft.Colors.GREEN,
            col={
                ft.ResponsiveRowBreakpoint.XS: 12,
                ft.ResponsiveRowBreakpoint.MD: 6,
                ft.ResponsiveRowBreakpoint.LG: 3,
            },
        ),
        ft.Container(
            content=ft.Text("Column 3"),
            padding=5,
            bgcolor=ft.Colors.BLUE,
            col={
                ft.ResponsiveRowBreakpoint.XS: 12,
                ft.ResponsiveRowBreakpoint.MD: 6,
                ft.ResponsiveRowBreakpoint.LG: 3,
            },
        ),
    ],
)
```

| 断点  | 尺寸       |
|-----|----------|
| xs	 | <576px   |
| sm	 | ≥576px   |
| md	 | ≥768px   |
| lg	 | ≥992px   |
| xl	 | ≥1200px  |
| xxl | 	≥1400px |

> 我不理解为什么要叫做断点。。。

根据 `col` 的设置，当程序窗口的像素宽度小于 576 像素时，满足 xs 的条件，根据 xs 的值设置所在组件的值来修改组件大小。

拿上面的例子来说，container 设置 xs 的值为 12，而一行的虚拟列总数是 12，
所以当窗口宽度小于 576 像素时单独单满一行，当大于 576 而小于 992 时，按照 md 的值，占据 6 个虚拟列，也就是两个两个组件占据一行。
其他几个设置也是如此。

## 属性

| 属性                 | 说明                                                    |
|--------------------|-------------------------------------------------------|
| alignment          | 水平对齐方式                                                |
| vertical_alignment | 垂直对齐方式                                                |
| breakpoints        | 定义响应属性使用的每个断点键的最小宽度<br>（例如 col、spacing 和 run_spacing） |
| columns            | 用于布局子项的虚拟列数。                                          |
| controls           | 要显示的控件列表                                              |
| run_spacing        | 运行之间的间距                                               |
| spacing            | 控件之间的间距（以虚拟像素为单位）                                     |
