# 成绩管理系统-login

## 目录
```bash
src
├── apps
│   ├── base
│   │   ├── my_colors.py
│   │   └── my_controls.py
│   ├── conf
│   │   └── cfg.py
│   └── ui
│       └── login.py
├── assets
│   ├── icon.png
│   └── splash_android.png
└── main.py
```

## my_colors.py

```python
class MyColor:
    WHITE = "#FFFFFF"
    RED = "#FF0000"
    BLUE = "#0000FF"
    PINK = "#FF00FF"
    PEACOCK_BLUE = "#4994c4"
```

## my_controls.py

```Python
import flet as ft
from apps.base.my_colors import MyColor


class NewText(ft.Text):

    def __init__(
        self,
        value: str = "文本",
        text_align: str = "START",
        size: int = 16,
        weight: str = "NORMAL",
        color: str = MyColor.WHITE,
        width: int = 200,
        align: ft.Alignment = ft.Alignment.TOP_CENTER,
    ) -> None:
        super().__init__(
            value=value,
            text_align=text_align,  # type: ignore
            size=size,
            weight=weight,  # type: ignore
            color=color,
            width=width,
            # align=align,
        )


class NewTextFeild(ft.TextField):

    def __init__(
        self,
        value: str = "文本",
        label: str = "输入框标题",
        hint_text: str = "提示文本",
        width: int = 200,
        border_color: str = MyColor.PEACOCK_BLUE,
        border_radius: ft.BorderRadiusValue = 5,
        border_width: int = 1,
    ) -> None:
        super().__init__(
            value=value,
            label=label,
            hint_text=hint_text,
            width=width,
            border_color=border_color,
            border_radius=border_radius,
            border_width=border_width,
        )


class NewPasswd(ft.TextField):

    def __init__(
        self,
        value: str = "文本",
        label: str = "输入框标题",
        hint_text: str = "提示文本",
        width: int = 200,
        border_color: str = MyColor.PEACOCK_BLUE,
        border_radius: ft.BorderRadiusValue = 5,
        border_width: int = 1,
        password: bool = True,
        can_reveal_password: bool = True,
    ) -> None:
        super().__init__(
            value=value,
            label=label,
            hint_text=hint_text,
            width=width,
            password=password,
            can_request_focus=can_reveal_password,
            border_color=border_color,
            border_radius=border_radius,
            border_width=border_width,
        )


class NewButton(ft.OutlinedButton):

    def __init__(
        self,
        content: str = "按钮",
        icon: ft.IconDataOrControl = ft.Icons.LOGIN,
        col={"xs": 12, "sm": 6, "md": 6, "lg": 6, "xl": 6, "xxl": 6},
        style: ft.ButtonStyle = ft.ButtonStyle(
            side=ft.BorderSide(
                width=1,
                color=MyColor.PEACOCK_BLUE,
            ),
            shape=ft.RoundedRectangleBorder(radius=5),
        ),
        on_click: ft.ControlEventHandler | None = None,
    ) -> None:
        super().__init__(
            content=content, icon=icon, col=col, style=style, on_click=on_click
        )
```

## cfg.py

```Python
import flet as ft


def load_config(page: ft.Page):
    # 窗口标题
    page.title = "成绩管理系统"
    # 对齐方式
    page.vertical_alignment = "START"  # type: ignore
    page.horizontal_alignment = "START"  # type: ignore
    # 间距
    page.padding = 0
    page.spacing = 0
    # 窗口宽高
    page.window.width = 500
    page.window.height = 500
    # 主题色：跟随系统
    page.theme_mode = "SYSTEM"  # type: ignore


if __name__ == "__main__":
    ft.run(load_config)
```

## login.py
```Python
import flet as ft
from apps.conf import cfg

import apps.base.my_controls as mc
from apps.base.my_colors import MyColor


def login(page: ft.Page):
    cfg.load_config(page=page)
    # 顶部logo
    img_container = ft.Container(
        content=ft.Image(
            src="src/assets/icon.png",
            width=30,
            align=ft.Alignment.CENTER_RIGHT,
        ),
        # border=ft.Border().all(width=1, color=MyColor().RED),
    )
    # 系统大标题
    system_name = mc.NewText(
        "学生成绩管理系统",
        weight="BOLD",
        color=MyColor().PEACOCK_BLUE,
        size=20,
        text_align="CENTER",
    )

    user_name = mc.NewTextFeild(value="", label="用户名", hint_text="请输入用户名")
    passwd = mc.NewPasswd(value="", label="密码", hint_text="请输入密码")

    # 登录、注册按钮
    def login_click(e: ft.Event):
        page.clean()
        print("登录成功")
        pass

    def sigin_click(e: ft.Event):
        print("注册功能暂时关闭")
        pass

    login_button = mc.NewButton(content="登录", on_click=login_click)
    sigin_button = mc.NewButton(content="注册", on_click=sigin_click)
    res_row = ft.ResponsiveRow(controls=[login_button, sigin_button], width=200)

    ## dropdowno切换主题
    def on_select(e: ft.Event):
        if e.control.value == "system":
            page.theme_mode = ft.ThemeMode.SYSTEM
            print("System")
        elif e.control.value == "light":
            page.theme_mode = ft.ThemeMode.LIGHT
            print("Light")
        else:
            page.theme_mode = ft.ThemeMode.DARK
            print("Dark")
        page.update()

    drop = ft.Dropdown(
        options=[
            ft.DropdownOption(text="System", key="system"),
            ft.DropdownOption(text="Light", key="light"),
            ft.DropdownOption(text="Dark", key="dark"),
        ],
        label="切换主题",
        value="system",
        on_select=on_select,
        width=200,
    )

    column = ft.Column(
        controls=[
            system_name,
            user_name,
            passwd,
            res_row,
            drop,
        ],
        align=ft.Alignment.TOP_CENTER,
    )
    # 增加外边框用
    container = ft.Container(
        content=column,
        border=ft.Border().all(width=1, color=MyColor().PEACOCK_BLUE),
        expand=True,
        padding=50,
    )
    total_column = ft.Column(
        controls=[
            img_container,
            container,
        ],
        spacing=0,
        width=300,
        height=400,
    )
    page.horizontal_alignment = "CENTER"  # type: ignore
    page.vertical_alignment = "CENTER"  # type: ignore
    page.add(total_column)
```

## main.py

```python
import flet as ft

import apps.ui.login as ui


def main(page: ft.Page):
    ui.login(page=page)


if __name__ == "__main__":
    ft.run(main)
```
