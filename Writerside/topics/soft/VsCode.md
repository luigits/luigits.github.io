<show-structure depth="3"/>

# VsCode

VsCode 支持配置文件进行 settings.json 和插件的隔离，之前的工作区只隔离了插件，可以说控制精度更细致了一些

可以通过搭配设置全局的插件和各配置的插件

- 优点：隔离了 settings.json 和插件
- 缺点：无法在 settings.json 中设置全局配置

## remote ssh

<tldr>

- [x] 提供远程开发功能：Remote - SSH
- [x] 资源管理器操作文件：Remote Explorer
- [x] 高亮ssh配置文件：Remote - SSH: Editing Configuration Files

</tldr>

Vs Code 通过 ssh 连接远程服务器/虚拟机也有一定几率报错。在安装完所需插件之后，记得清理可能存在的系统残留。

```Bash
# ssh -p 端口号 用户名@远程ip 
ssh -p 2222 zhanyu@Rocky8
```

因为我使用的是 VirtualBox，需要设置端口转发才能连接，因此才修改为了 2222。如果一切正常，那么此时已经连接成功。

### 免密登录

#### 生成密钥对

```Bash
# ssh-keygen -t 指定算法 -b 密钥长度
ssh-keygen -t ed25519 -b 4096 # 推荐使用

ssh-keygen -t rsa -b 4096 # 备用
```

输入指令后全程回车即可。

#### 传输公钥到虚拟机

```Bash
ssh-copy-id -i ~/.ssh/公钥文件.pub 用户名@服务器地址
```

如果无法使用，可以先使用 ssh 登录到远程主机，然后将公钥粘贴过去

1. 查看公钥的内容
2. 登录远程主机，将公钥的内容添加到 <ui-path>~/.ssh/authorized_keys</ui-path> 末尾

#### 设置文件权限

<tldr>
不要盲目设置高权限
</tldr>

```bash
# 1. home目录权限（不能是 777，推荐 755）
chmod 755 ~

# 2. .ssh 目录权限（必须严格为 700）
chmod 700 ~/.ssh

# 3. authorized_keys 权限（必须为 600）
chmod 600 ~/.ssh/authorized_keys

# 4. 确保所有文件所有者是当前用户（防止残留 root 权限）
chown -R zhanyu:zhanyu ~/.ssh
```

#### 配置 ssh 配置文件

编辑 <ui-path>~/.ssh/config</ui-path> 文件，或直接使用 Vs Code 创建新远程连接：

> Windows 在 <ui-path>C:\Users\用户名\.ssh\config</ui-path>
>
> PowerShell 也支持使用 <ui-path>~/.ssh/config</ui-path>跳转到 <ui-path>C:\Users\用户名\.ssh\config</ui-path>

```Bash
Host 主机别名
  HostName 服务器IP或域名
  User 远程用户名
  Port 端口号
  PreferredAuthentications publickey
  IdentityFile "~/.ssh/密钥文件名" # 不带.pub的那个
```

设置完毕后应该就可以不输入密码直接登录到远程主机了。

## Settings.json

### 全局设置

```JSON
{
  // 禁用和隐藏AI功能
  "chat.disableAIFeatures": true,
  //?  =========== 工作台 ===========
  // 设置vscode的icon主题
  "workbench.iconTheme": "vscode-icons",
  // 设置vscode的颜色主题
  "workbench.colorTheme": "Dark Modern",
  //右侧智能体是否显示
  "workbench.secondarySideBar.defaultVisibility": "hidden",
  //?  =========== 文  件 ===========
  // 焦点变化时自动保存
  "files.autoSave": "onFocusChange",
  // 文件行尾序列为LF
  "files.eol": "\n",
  //  设置文件关联
  "files.associations": {
    // vscode中支持json使用注释
    "*.json": "jsonc"
  },
  //?  =========== 编辑器 ===========
  // 保存时格式化
  "editor.formatOnSave": true,
  // 粘贴时格式化  
  "editor.formatOnPaste": true,
  // 设置字体
  "editor.fontFamily": "Maple Mono",
  //启用连字(需要字体支持)
  "editor.fontLigatures": true,
  // 制表符宽度
  "editor.tabSize": 4,
  // 按 Tab 键时插入空格
  "editor.insertSpaces": true,
  //显示空格字符
  "editor.renderWhitespace": "boundary",
  //显示缩略图
  "editor.minimap.enabled": true,
  // 括号成对高亮着色
  "editor.bracketPairColorization.enabled": true,
  // 为括号对增加参考线高亮
  "editor.guides.bracketPairs": "active",
  // 摁tab自动填入推荐值
  "editor.tabCompletion": "on",
  // "source.sort.json": "always", //自动排序 JSON 文件中的键（Key）
  // 忽略空格
  "diffEditor.ignoreTrimWhitespace": false,
  // 自定义编辑器中语法高亮的颜色
  "editor.tokenColorCustomizations": {
    // 注释的高亮色
    "comments": "#5F7A61"
  },
  //不突出显示的Unicode字符
  "editor.unicodeHighlight.allowedCharacters": {
    "，": true,
    "zh-hans": true
  },
  //?  =========== 终  端 ===========
  // 设置终端配置
  "terminal.integrated.profiles.windows": {
    // 增加PowerShell 7的终端配置
    "PowerShell 7": {
      "path": "D:/SoftWare/Scoop/shims/pwsh.exe",
      "icon": "terminal-powershell"
    }
  },
  // Windows默认终端
  "terminal.integrated.defaultProfile.windows": "PowerShell 7",
  // Linux默认终端
  "terminal.integrated.defaultProfile.linux": "bash",
  // MacOS默认终端
  "terminal.integrated.defaultProfile.osx": "zsh",
  // 终端字体
  "terminal.integrated.fontFamily": "Maple Mono NF CN"
}
```

### 编程语言设置

#### python

```JSON
{
  "[python]": {
    // 默认格式化程序
    "editor.defaultFormatter": "ms-python.black-formatter",
    //当保存时需要进行的配置
    "editor.codeActionsOnSave": {
      // 仅自动修复 pylance 能处理的错误
      "source.fixAll.pylance": "always",
      // 仅自动整理导入语句
      "source.organizeImports": "explicit",
      // 自动重命名与 Python 标准库同名的导入
      "source.renameShadowedStdlibImports": "always",
      // 自动删除未使用的导入
      "source.unusedImports": "always"
    }
  },
  // isort插件，可以自动排序python包，isort插件要求项目中必须存在isort库
  "isort.args": [
    "--profile",
    "black"
  ],
  // "python.terminal.activateEnvironment": false, // 是否自动进入虚拟环境
  //?  ========== pylance插件 ==========
  //支持文档字符串模板
  "python.analysis.supportDocstringTemplate": true,
  // * ========== 自动导入 ==========
  // 使用Pylance作为语言服务器，以保证自动导入包的功能
  "python.languageServer": "Pylance",
  // 自动导入需要的库
  "python.analysis.autoImportCompletions": true,
  "editor.quickSuggestions": {
    "strings": true
  },
  "python.analysis.aiCodeActions": {
    "convertFormatString": false,
    "generateSymbol": true,
    "implementAbstractClasses": false
  },
  // 字符串内使用"{}"时自动为字符串增加f前缀
  "python.analysis.autoFormatStrings": true,
  "python.analysis.completeFunctionParens": true,
  // 启用并行索引,以便可能更快地索引大型项目
  "python.analysis.enableParallelIndexing": true,
  "python.analysis.enableTroubleshootMissingImports": true,
  "python.analysis.generateWithTypeAnnotation": true,
  "python.analysis.inlayHints.functionReturnTypes": true,
  "python.analysis.inlayHints.variableTypes": true,
  "python.analysis.typeCheckingMode": "standard",
  //推断 dict 的类型时，使用严格的类型假设
  "python.analysis.typeEvaluation.strictDictionaryInference": true,
  // 推断 list 的类型时，使用严格的类型假设
  "python.analysis.typeEvaluation.strictListInference": true,
  // 推断 set 的类型时，使用严格的类型假设
  "python.analysis.typeEvaluation.strictSetInference": true
}
```

### 插件设置

#### Toml 配置

```JSON
{
  "[toml]": {
    // toml的默认格式化程序
    "editor.defaultFormatter": "tamasfe.even-better-toml"
  },
  // 关闭不对齐等号，避免大量空白变更
  "evenBetterToml.formatter.alignEntries": true,
  // 垂直对齐条目和项目后的连续注释
  "evenBetterToml.formatter.alignComments": true,
  // 允许的连续空行的最大数量
  "evenBetterToml.formatter.allowedBlankLines": 1
}
```

#### markdown 配置

```JSON
{
  "[markdown]": {
    "editor.defaultFormatter": "yzhang.markdown-all-in-one"
  },
  // 对其行尾注释
  "explorer.confirmDelete": false
}
```

#### liveServer 配置

```JSON
{
  // 不验证标签
  "liveServer.settings.donotVerifyTags": true,
  // 不显示Info消息
  "liveServer.settings.donotShowInfoMsg": true
}
```

#### git 配置

```JSON
{
  // 同步是否弹出提示，个人项目可使用false，团队项目推荐使用true，避免手贱导致的问题
  "git.confirmSync": false,
  // 自动触发fetch
  "git.autofetch": true
}
```

#### FileHead 配置

这是一个给所有文件添加头文件的插件，常用于自动添加个人信息

```JSON
{
  // author信息
  "fileheader.author": "ZhanYu",
  //其他配置信息
  "fileheader.other_config": {
    "email": "2833171898@qq.com",
    "github": "https://github.com/luigit",
    "blog": "https://luigit.github.io"
    "editor.unicodeHighlight.allowedLocales": {
      "zh-hans": true
    }
  }
```

需要修改模板才能让 *`other_config` 的配置正常显示

#### PowerShell 配置

```JSON
{
  // 选择预设的代码风格
  "powershell.codeFormatting.preset": "OTBS",
  // }后是否换行
  "powershell.codeFormatting.newLineAfterCloseBrace": false,
  //是否将单行代码块保持在一行内
  "powershell.codeFormatting.ignoreOneLineBlock": true,
  //取消扩展终端的自启动
  "powershell.integratedConsole.showOnStartup": false,
  // powerShell附加执行路径
  "powershell.powerShellAdditionalExePaths": {
    // 当PowerShell找不到格式化程序时
    "PowerShell 7": "D:/SoftWare/Scoop/shims/pwsh.exe"
  },
  // powershell的默认版本
  "powershell.powerShellDefaultVersion": "PowerShell 7"
}
```

#### Better Comments 插件

```json
{
  "better-comments.tags": [
    {
      "tag": "!",
      "color": "#FF2D00",
      "strikethrough": false,
      "underline": false,
      "backgroundColor": "transparent",
      "bold": false,
      "italic": false
    },
    {
      "tag": "?",
      "color": "#3498DB",
      "strikethrough": false,
      "underline": false,
      "backgroundColor": "transparent",
      "bold": false,
      "italic": false
    },
    {
      "tag": "//",
      "color": "#474747",
      "strikethrough": true,
      "underline": false,
      "backgroundColor": "transparent",
      "bold": false,
      "italic": false
    },
    {
      "tag": "todo",
      "color": "#FF8C00",
      "strikethrough": false,
      "underline": false,
      "backgroundColor": "transparent",
      "bold": false,
      "italic": false
    },
    {
      "tag": "*",
      "color": "#98C379",
      "strikethrough": false,
      "underline": false,
      "backgroundColor": "transparent",
      "bold": false,
      "italic": false
    },
    //> 自定义注释模式
    {
      "tag": ">",
      "color": "#D1E340",
      //删除线
      "strikethrough": false,
      //下划线
      "underline": false,
      "backgroundColor": "transparent",
      // 加粗
      "bold": false,
      // 斜体
      "italic": false
    }
  ]
}
```