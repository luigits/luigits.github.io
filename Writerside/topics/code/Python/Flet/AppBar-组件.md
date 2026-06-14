# AppBar 组件

`AppBar` 组件是 flet 自带的用于导航栏功能的组件，

## 属性

| 属性            | 说明                               |
|---------------|----------------------------------|
| leading       | 最左侧的控件，通常是 icon 或 iconButton 控件。 |
| leading_width | leading 使用的控件无法设置大小，必须使用它进行设置    |
| title         | AppBar 标题，当前页面是什么页面可以设置为 title   |
| center_title  | 是否将 AppBar 标题居中                  |
| actions       | AppBar 的行为，右对齐 (切换主题色或下拉菜单等)     |

```Python
page.appbar = ft.AppBar(
    leading=ft.Image(
        src="icon.png"
    ),
    leading_width=30,
    title="AppBar Title",  # AppBar的标题，在leading的右侧
    bgcolor="#77ffbf",
    center_title=True,  # 标题在AppBar中居中显示
    actions=[  # actions的控件，右对齐
        ft.IconButton(icon=ft.Icons.LIGHT_MODE, on_click=on_click, key="theme"),
        ft.PopupMenuButton(
            items=[
                ft.PopupMenuItem(content="item 1", on_click=popup_click),
                ft.PopupMenuItem(),
                ft.PopupMenuItem(content="item 3", on_click=popup_click),
            ]
        ),
    ],
)
```

需要注意一点：使用 page.clean() 清理的是使用 page.add() 注册的组件，而导航栏是使用 page.appbar 属性注册的

> 使用 page.add() 也可以注册，但这会让导航栏变成一个普通组件，会因为注册顺序而影响渲染的位置
>
> 而导航栏的特殊之处也只是不使用 page.add() 注册

## 其他种类的导航栏

| 导航栏   | 属性              | 类                     |
|-------|-----------------|-----------------------|
| 顶部导航栏 | page.appbar     | ft.AppBar()           |
| 左抽屉   | page.drawer     | ft.NavigationDrawer() |
| 右抽屉   | page.end_drawer | ft.NavigationDrawer() |

- 导航抽屉
    - 左抽屉 (page.drawer)：
- 侧边导航栏
- 底部导航栏:page.bottom_appbar



# Flet 侧边栏与导航栏控件详解

根据 [Flet 官方文档](https://flet.dev/docs/)，以下是关于侧边栏和导航相关控件的详细说明：

---

## 🔹 侧边栏控件

Flet 中**没有名为 `Sidebar` 的独立控件**，实现侧边栏效果主要使用以下控件：

### `NavigationDrawer`（导航抽屉）⭐ 最常用
> Material Design 风格的侧边滑出面板，从视图左侧或右侧滑入显示主要导航项 [[8]]

```python
ft.NavigationDrawer(
    controls=[
        ft.NavigationDrawerDestination(icon=ft.Icons.HOME_OUTLINED, label="首页"),
        ft.NavigationDrawerDestination(icon=ft.Icons.SETTINGS_OUTLINED, label="设置"),
    ],
    on_change=handle_change,  # 选中项变化时触发
    on_dismiss=handle_dismissal  # 抽屉关闭时触发
)
```

**关键属性：**
| 属性 | 说明 |
|------|------|
| `controls` | 抽屉内显示的控件列表（通常用 `NavigationDrawerDestination`） |
| `selected_index` | 当前选中项的索引 |
| `width` | 抽屉宽度（逻辑像素） |
| `bgcolor` / `elevation` | 背景色/阴影高度 |

**使用方式：**
```python
# 左侧抽屉
page.drawer = ft.NavigationDrawer(...)
await page.show_drawer()    # 显示
await page.close_drawer()   # 关闭

# 右侧抽屉
page.end_drawer = ft.NavigationDrawer(...)
await page.show_end_drawer()
await page.close_end_drawer()
```

---

## 🔹 导航栏控件汇总

Flet 提供多种导航控件，适用于不同场景：

| 控件 | 位置 | 适用场景 | 文档链接 |
|------|------|----------|----------|
| **`NavigationBar`** | 底部 | 移动端/窄屏，3-5个主要目的地 [[11]] | [文档](https://flet.dev/docs/controls/navigationbar/) |
| **`NavigationRail`** | 左侧/右侧垂直 | 桌面端/宽屏，3-5个主要目的地 [[15]] | [文档](https://flet.dev/docs/controls/navigationrail/) |
| **`NavigationDrawer`** | 侧边滑出 | 隐藏式侧边栏，适合多级导航 [[8]] | [文档](https://flet.dev/docs/controls/navigationdrawer/) |
| **`AppBar`** | 顶部 | 应用标题+操作按钮+抽屉触发器 | [文档](https://flet.dev/docs/controls/appbar/) |
| **`BottomAppBar`** | 底部 | 配合 FloatingActionButton 使用 | [文档](https://flet.dev/docs/controls/bottomappbar/) |
| **`MenuBar`** | 顶部/任意 | 级联菜单系统（类似传统桌面菜单）[[5]] | [文档](https://flet.dev/docs/controls/menubar/) |
| **`CupertinoNavigationBar`** | 顶部 | iOS 风格导航栏 | [文档](https://flet.dev/docs/controls/cupertinoNavigationBar/) |

---

## 🔹 自适应导航方案（官方推荐）

Flet 官方示例展示了如何根据屏幕宽度自动切换导航模式 [[14]]：

```python
def render():
    # 宽屏：使用 NavigationRail + 右侧 NavigationDrawer
    if page.width >= 450:
        page.navigation_bar = None
        page.end_drawer = build_end_drawer()
        page.add(build_drawer_layout())
    # 窄屏：使用底部 NavigationBar
    else:
        page.end_drawer = None
        page.navigation_bar = build_navigation_bar()
        page.add(build_bottom_bar_layout())
    page.update()

page.on_resize = lambda e: render()  # 监听窗口变化
```

---

## 🔹 快速选择建议

```
📱 移动端优先 → NavigationBar（底部标签栏）
💻 桌面端优先 → NavigationRail（垂直侧边栏）
🗂️ 多级/隐藏导航 → NavigationDrawer（滑动抽屉）
🍎 iOS 风格 → CupertinoNavigationBar
🖥️ 传统菜单 → MenuBar
```

> 💡 **提示**：所有导航控件都支持 `on_change` 事件来响应用户选择，配合 `Page` 的路由系统可实现完整的页面跳转逻辑 [[14]]。

如需查看完整代码示例，建议直接访问 [Flet Controls 目录](https://flet.dev/docs/controls/) 或各控件文档页面的 "Examples" 部分。