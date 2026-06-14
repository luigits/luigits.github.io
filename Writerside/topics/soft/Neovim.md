# Neovim

NeoVim 是 Vim 的分支，更加现代化，同时支持使用 Lua 进行配置各项功能，对于习惯了 Vim 的人来说是不可多得的替代品

## init.lua

### 配置文件路径

NeoVim 配置文件的位置是需要检查的，因为安装之后未必会自动生成配置文件，仍需要手动生成

#### Windows

这个比较头疼，因为藏得比较深，毕竟 Windows 的 `APPDATA` 实在是让人头大。

首先进入 <ui-path>C:\Users\luigit\AppData\Local</ui-path>，先检查该目录下是否存在 <ui-path>nvim</ui-path> 这个文件夹。

若不存在则创建，若存在则进入，检查 `init.lua` 是否存在。同样不存在则创建。

这是一个用于检查上述提到的目录和文件的脚本，确保执行完之后一定存在。

```PowerShell 
# 设置目录和文件路径
$localAppData = $env:LOCALAPPDATA
$nvimPath = Join-Path $localAppData "nvim"
$initLuaPath = Join-Path $nvimPath "init.lua"

# 检查并创建 nvim 目录
if (-not (Test-Path $nvimPath)) {
    New-Item -Path $nvimPath -ItemType Directory -Force | Out-Null
    Write-Host "已创建目录: $nvimPath"
}

# 检查并创建 init.lua 文件
if (-not (Test-Path $initLuaPath)) {
    Set-Content -Path $initLuaPath -Value "" -Encoding UTF8
    Write-Host "已创建文件: $initLuaPath" -ForegroundColor Green
} else {
    Write-Host "文件已存在: $initLuaPath"
}
```

#### Linux/Mac

略

### 相关配置

确认 init.lua 存在后可以阅读相关文档进行基础的配置了

```lua
-- 基础设置
vim.opt.number = true                  -- 显示行号
vim.opt.mouse = 'a'                    -- 启用鼠标支持
vim.opt.autoindent = true              -- 自动缩进
vim.opt.hlsearch = true                -- 高亮搜索
vim.opt.backspace = 'indent,eol,start' -- 让退格键正常工作
```





---





[//]: # ()
[//]: # (# 在 neovim 中使用 Lua)

[//]: # ()
[//]: # (**在 neovim 0.9 版本中添加了 lua 教程，如果你使用该版本的 neovim 可以使用 `:h lua-guide` 查看**)

[//]: # (**对于英文不好的同学那么此文档对你仍然有用**)

[//]: # ()
[//]: # ([nvim-lua-guide]&#40;https://github.com/nanotee/nvim-lua-guide&#41; 中文版简易教程)

[//]: # ()
[//]: # (译者：Neovim Core Developer)

[//]: # ()
[//]: # (:arrow_upper_left: （感觉太多太杂乱？使用 Github TOC 来浏览大纲！）)

[//]: # ()
[//]: # (## 简介)

[//]: # ()
[//]: # (Lua 作为 Neovim 中的一等语言的集成正在成为它的杀手级特性之一。然而，学习如何用 Lua 编写插件的教程数量并不像用 Vimscript 编写插件那样多。这是一种尝试，试图提供一些基本信息，让人们可以使用 Lua 编写 Neovim 插件。)

[//]: # ()
[//]: # (本指南假定您使用的是 Neovim 0.5+)

[//]: # ()
[//]: # (### 学习 Lua)

[//]: # ()
[//]: # (不同于原版教程，以下资源适用于国内用户：)

[//]: # (- [在 Y 分钟内学习 X 关于 Lua 的页面]&#40;https://learnxinyminutes.com/docs/lua/&#41;)

[//]: # (- [Lua 菜鸟教程]&#40;https://www.runoob.com/lua/lua-tutorial.html&#41;)

[//]: # (- [Lua 用户维基]&#40;http://lua-users.org/wiki/LuaDirectory&#41;)

[//]: # (- [Lua 的官方参考手册]&#40;https://www.lua.org/manual/5.1/&#41;)

[//]: # ()
[//]: # (Lua 是一种非常干净和简单的语言。它很容易学习，特别是如果你有其他编程语言基础的例如 TypeScript / JavaScript 等，会更加容易上手 Lua。注意：Neovim 嵌入的 Lua 版本是 LuaJIT 2.1.0，它与 Lua 5.1 保持兼容（带有几个 5.2 扩展）)

[//]: # ()
[//]: # (### 现有的一些在 Neovim 中使用 Lua 的教程)

[//]: # ()
[//]: # (已经编写了一些教程来帮助人们用 Lua 编写插件。他们中的一些人在写这本指南时提供了不少的帮助。非常感谢它们的作者。)

[//]: # ()
[//]: # (- [teukka.tech - 从 init.vim 转到 init.lua]&#40;https://teukka.tech/luanvim.html&#41;)

[//]: # (- [2n.pl - 如何使用 Lua 编写 neovim 插件]&#40;https://www.2n.pl/blog/how-to-write-neovim-plugins-in-lua.md&#41;)

[//]: # (- [2n.pl - 如何使用 Lua 制作 neovim UI]&#40;https://www.2n.pl/blog/how-to-make-ui-for-neovim-plugins-in-lua&#41;)

[//]: # (- [ms-jpq - NeoVim 异步教程]&#40;https://ms-jpq.github.io/lua-async-await/index.html&#41;)

[//]: # ()
[//]: # (### 相关插件)

[//]: # ()
[//]: # (- [Vimpeccable]&#40;https://github.com/svermeulen/vimpeccable&#41; - Plugin to help write your .vimrc in Lua)

[//]: # (- [plenary.nvim]&#40;https://github.com/nvim-lua/plenary.nvim&#41; - All the lua functions I don't want to write twice)

[//]: # (- [popup.nvim]&#40;https://github.com/nvim-lua/popup.nvim&#41; - An implementation of the Popup API from vim in Neovim)

[//]: # (- [nvim_utils]&#40;https://github.com/norcalli/nvim_utils&#41;)

[//]: # (- [nvim-luadev]&#40;https://github.com/bfredl/nvim-luadev&#41; - REPL/debug console for nvim lua plugins)

[//]: # (- [nvim-luapad]&#40;https://github.com/rafcamlet/nvim-luapad&#41; - Interactive real time neovim scratchpad for embedded lua engine)

[//]: # (- [nlua.nvim]&#40;https://github.com/tjdevries/nlua.nvim&#41; - Lua Development for Neovim)

[//]: # (- [galaxyline.nvim]&#40;https://github.com/glepnir/galaxyline.nvim&#41; - neovim statusline plugin written in lua)

[//]: # (- [BetterLua.vim]&#40;https://github.com/euclidianAce/BetterLua.vim&#41; - Better Lua syntax highlighting in Vim/NeoVim)

[//]: # ()
[//]: # (## Lua 文件位置)

[//]: # ()
[//]: # (### init.lua)

[//]: # ()
[//]: # (Neovim 支持从 `init.lua` 文件加载配置而不是通常的 `init.vim` 文件。)

[//]: # ()
[//]: # (注意：`init.lua` 文件是完全可选的。Neovim 仍然支持从 `init.vim` 加载配置。请记住，Neovim 的一些功能还没有 100% 暴露给 Lua 模块部分。)

[//]: # ()
[//]: # (### 模块)

[//]: # ()
[//]: # (Lua 模块通常位于您的 `runtimepath` 中的 `lua/` 文件夹中（对于大多数用户来说，在 *nix 系统上为 `~/.config/nvim/lua`，在 Windows 系统上为 `~/appdata/Local/nvim/lua`&#41;。这意味着您可以 `require&#40;&#41;` 这些文件作为 Lua 模块)

[//]: # ()
[//]: # (我们以下面的文件夹结构为例：)

[//]: # ()
[//]: # (```text)

[//]: # (📂 ~/.config/nvim)

[//]: # (├── 📁 after)

[//]: # (├── 📁 ftplugin)

[//]: # (├── 📂 lua)

[//]: # (│  ├── 🌑 myluamodule.lua)

[//]: # (│  └── 📂 other_modules)

[//]: # (│     ├── 🌑 anothermodule.lua)

[//]: # (│     └── 🌑 init.lua)

[//]: # (├── 📁 pack)

[//]: # (├── 📁 plugin)

[//]: # (├── 📁 syntax)

[//]: # (└── 🇻 init.vim)

[//]: # (```)

[//]: # ()
[//]: # (下面的 Lua 代码将加载 `myluamodule.lua`)

[//]: # ()
[//]: # (```lua)

[//]: # (require&#40;'myluamodule'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (注意没有 `.lua` 扩展名。)

[//]: # ()
[//]: # (类似地，加载 `other_modules/anothermodule.lua` 的过程如下：)

[//]: # ()
[//]: # (```lua)

[//]: # (require&#40;'other_modules.anothermodule'&#41;)

[//]: # (-- or)

[//]: # (require&#40;'other_modules/anothermodule'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (路径分隔符可以用点 `.` 表示，也可以用斜杠 `/` 表示。)

[//]: # ()
[//]: # (文件夹如果包含 `init.lua` 文件，可以直接引用该文件夹而不必指定该文件的名称)

[//]: # ()
[//]: # (```lua)

[//]: # (require&#40;'other_modules'&#41; -- loads other_modules/init.lua)

[//]: # (```)

[//]: # ()
[//]: # (加载一个不存在的模块或者加载的模块有语法错误会直接中止当前正在执行的脚本。`pcall&#40;&#41;` 函数可以用来处理这类错误)

[//]: # ()
[//]: # (```lua)

[//]: # (local ok, _ = pcall&#40;require, 'module_with_error'&#41;)

[//]: # (if not ok then)

[//]: # (  -- not loaded)

[//]: # (end)

[//]: # (```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (* [`:help lua-require`]&#40;https://neovim.io/doc/user/lua.html#lua-require&#41;)

[//]: # ()
[//]: # (#### 提示)

[//]: # ()
[//]: # (多个 Lua 插件在它们的 `lua/` 文件夹中可能有相同的文件名。这可能会导致命名空间冲突。如果两个不同的插件有一个 `lua/main.lua` 文件，那么执行 `require&#40;'main'&#41;` 是不明确的：我们想要加载哪个文件？最好将您的配置或插件命名为顶级文件夹，)

[//]: # (例如这样的形式：`lua/plugin_name/main.lua`。)

[//]: # ()
[//]: # (### 运行时文件)

[//]: # ()
[//]: # (和 Vimscript 文件很像，位于 ` runtimepath` 中的一些特殊目录中的 Lua 文件可以被 Neovim 自动加载。目前有以下这些特殊目录：)

[//]: # ()
[//]: # (* `colors/`)

[//]: # (* `compiler/`)

[//]: # (* `ftplugin/`)

[//]: # (* `indent/`)

[//]: # (* `plugin/`)

[//]: # (* `syntax/`)

[//]: # ()
[//]: # (注意：在同一个运行时目录中，`*.vim` 文件会先于所有的 `*.lua` 文件被加载。)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (* [`:help 'runtimepath'`]&#40;https://neovim.io/doc/user/options.html#'runtimepath'&#41;)

[//]: # (* [`:help load-plugins`]&#40;https://neovim.io/doc/user/starting.html#load-plugins&#41;)

[//]: # ()
[//]: # (#### 提示)

[//]: # ()
[//]: # (因为运行时文件并不基于 Lua 模块系统，所以两个不同的插件都拥有 `plugins/main.lua` 文件是没有任何问题的。)

[//]: # ()
[//]: # (## 在 Vimscript 中使用 Lua)

[//]: # ()
[//]: # (### :lua)

[//]: # ()
[//]: # (该命令执行一段 Lua 代码)

[//]: # ()
[//]: # (```vim)

[//]: # (:lua require&#40;'myluamodule'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (可以使用以下语法编写多行脚本：)

[//]: # ()
[//]: # (```vim)

[//]: # (echo "Here's a bigger chunk of Lua code")

[//]: # ()
[//]: # (lua << EOF)

[//]: # (local mod = require&#40;'mymodule'&#41;)

[//]: # (local tbl = {1, 2, 3})

[//]: # ()
[//]: # (for k, v in ipairs&#40;tbl&#41; do)

[//]: # (    mod.method&#40;v&#41;)

[//]: # (end)

[//]: # ()
[//]: # (print&#40;tbl&#41;)

[//]: # (EOF)

[//]: # (```)

[//]: # ()
[//]: # (注意：每个 `:lua` 命令都有它自己独立的作用域，在一条 `:lua` 命令中使用 `local` 关键字声明的变量是无法在这条命令之外访问的。如下代码所示)

[//]: # ()
[//]: # (```vim)

[//]: # (:lua local foo = 1)

[//]: # (:lua print&#40;foo&#41;)

[//]: # (" prints 'nil' instead of '1')

[//]: # (```)

[//]: # ()
[//]: # (注意：Lua 中的 `print&#40;&#41;` 函数的行为类似于 `:echomsg` 命令。它的输出会被保存在消息历史中，可以使用 `:slient` 命令来抛弃输出。)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (- [`:help :lua`]&#40;https://neovim.io/doc/user/lua.html#Lua&#41;)

[//]: # (- [`:help :lua-heredoc`]&#40;https://neovim.io/doc/user/lua.html#:lua-heredoc&#41;)

[//]: # ()
[//]: # (### :luado)

[//]: # ()
[//]: # (该命令执行一段 Lua 代码，该代码作用于当前缓冲区中的选中的行。如果未指定范围，则改为使用整个缓冲区。从块 `return` 的任何字符串都用于确定应该用什么替换每行。)

[//]: # ()
[//]: # (以下命令会将当前缓冲区中的每一行替换为文本 `hello world`)

[//]: # ()
[//]: # (```vim)

[//]: # (:luado return 'hello world')

[//]: # (```)

[//]: # ()
[//]: # (提供了两个隐式的 `line` 和 `linenr` 变量。`line` 是被迭代的行的文本，而 `linenr` 是它的编号。以下命令将可以被 2 整除的行转成大写：)

[//]: # ()
[//]: # (```vim)

[//]: # (:luado if linenr % 2 == 0 then return line:upper&#40;&#41; end)

[//]: # (```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (- [`:help :luado`]&#40;https://neovim.io/doc/user/lua.html#:luado&#41;)

[//]: # ()
[//]: # (### 加载 Lua 文件)

[//]: # ()
[//]: # (Neovim 提供了三种执行命令来加载 Lua 文件)

[//]: # ()
[//]: # (* `:luafile`)

[//]: # (* `:source`)

[//]: # (* `:runtime`)

[//]: # ()
[//]: # (其中 `:luafile` 和 `:source` 非常类似：)

[//]: # ()
[//]: # (```vim)

[//]: # (:luafile ~/foo/bar/baz/myluafile.lua " 加载 myluafile.lua)

[//]: # (:luafile %                           " 加载当前正在处理的文件)

[//]: # (:source ~/foo/bar/baz/myluafile.lua)

[//]: # (:source %)

[//]: # (```)

[//]: # ()
[//]: # (两种命令都可以指定文件范围，可以只执行脚本的一部分)

[//]: # ()
[//]: # (```vim)

[//]: # (:1,10source)

[//]: # (```)

[//]: # ()
[//]: # (`:runtime` 与上述两种命令有所不同，它使用 `'runtimepath'` 选项来决定加载哪个文件。更多细节信息请参见  [`:help :runtime`]&#40;https://neovim.io/doc/user/repeat.html#:runtime&#41;)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (- [`:help :luafile`]&#40;https://neovim.io/doc/user/lua.html#:luafile&#41;)

[//]: # (- [`:help :source`]&#40;https://neovim.io/doc/user/repeat.html#:source&#41;)

[//]: # (- [`:help :runtime`]&#40;https://neovim.io/doc/user/repeat.html#:runtime&#41;)

[//]: # ()
[//]: # (####  加载 Lua 文件对比 require&#40;&#41;:)

[//]: # ()
[//]: # (您可能想知道调用 `require&#40;&#41;` 函数和使用上述命令加载之间的区别是什么，以及您是否应该使用其中一个而不是另一个。它们有不同的使用情形：)

[//]: # ()
[//]: # (- `require&#40;&#41;`:)

[//]: # (    - 是内置的 Lua 函数，它允许你使用 Lua 的模块系统)

[//]: # (    - 在 `'runtimepath'` 中的 `lua/` 文件夹中搜索模块)

[//]: # (    - 跟踪已加载的模块，并防止第二次解析和执行脚本。如果您更改包含某个模块代码的文件，并在 Neovim 运行时再次尝试 `required&#40;&#41;`，则该模块实际上不会更新)

[//]: # (- `:luafile`/`:source`/`:runtime`:)

[//]: # (    - 是一个执行命令，它不支持模块)

[//]: # (    - 执行脚本的内容，而不管该脚本以前是否执行过)

[//]: # (    - `:luafile`/`:source` 命令采用相对于当前窗口的工作目录的绝对或相对路径)

[//]: # (    - `:runtime` 命令使用 `'runtimepath'` 选项来寻找文件)

[//]: # ()
[//]: # (同时，通过 `:source`/`:runtime` 命令（不包括  `:luafile` ）加载或者从运行时目录被自动加载的文件会显示在 `:scriptnames` 和 `--startuptime` 中。)

[//]: # ()
[//]: # (### luaeval&#40;&#41;)

[//]: # ()
[//]: # (`luaeval&#40;&#41;` 是内置的 Vimscript 函数计算 Lua 表达式字符串并返回它的值。Lua 的数据类型自动转换为 Vimscript 类型（反之亦然）。)

[//]: # ()
[//]: # (```vim)

[//]: # (" 你可以将结果存储到一个变量中)

[//]: # (let variable = luaeval&#40;'1 + 1'&#41;)

[//]: # (echo variable)

[//]: # (" 2)

[//]: # (let concat = luaeval&#40;'"Lua".." is ".."awesome"'&#41;)

[//]: # (echo concat)

[//]: # (" 'Lua is awesome')

[//]: # ()
[//]: # (" Lua 中的 table 数组转成成 Vimscript 的 list)

[//]: # (let list = luaeval&#40;'{1, 2, 3, 4}'&#41;)

[//]: # (echo list[0])

[//]: # (" 1)

[//]: # (echo list[1])

[//]: # (" 2)

[//]: # (" 注意 Vimscript 的数组索引下标与 Lua 不同是从 0 开始的，Lua 中是从 1 开始)

[//]: # ()
[//]: # (" Lua 中类似 dict 的 table 会被转成 Vimscript 中的 dict)

[//]: # (let dict = luaeval&#40;'{foo = "bar", baz = "qux"}'&#41;)

[//]: # (echo dict.foo)

[//]: # (" 'bar')

[//]: # ()
[//]: # (" 布尔类型和 nil 是类似的)

[//]: # (echo luaeval&#40;'true'&#41;)

[//]: # (" v:true)

[//]: # (echo luaeval&#40;'nil'&#41;)

[//]: # (" v:null)

[//]: # ()
[//]: # (" 您可以为 Lua 函数创建 Vimscript 别名)

[//]: # (let LuaMathPow = luaeval&#40;'math.pow'&#41;)

[//]: # (echo LuaMathPow&#40;2, 2&#41;)

[//]: # (" 4)

[//]: # (let LuaModuleFunction = luaeval&#40;'require&#40;"mymodule"&#41;.myfunction'&#41;)

[//]: # (call LuaModuleFunction&#40;&#41;)

[//]: # ()
[//]: # (" 还可以将 Lua 函数作为值传递给 Vim 函数)

[//]: # (lua X = function&#40;k, v&#41; return string.format&#40;"%s:%s", k, v&#41; end)

[//]: # (echo map&#40;[1, 2, 3], luaeval&#40;"X"&#41;&#41;)

[//]: # (```)

[//]: # ()
[//]: # (`luaeval&#40;&#41;` 接受可选的第二个参数，该参数允许您将数据传递给表达式。然后您可以使用全局的 `_A` 从 Lua 访问该数据：)

[//]: # ()
[//]: # (```vim)

[//]: # (echo luaeval&#40;'_A[1] + _A[2]', [1, 1]&#41;)

[//]: # (" 2)

[//]: # ()
[//]: # (echo luaeval&#40;'string.format&#40;"Lua is %s", _A&#41;', 'awesome'&#41;)

[//]: # (" 'Lua is awesome')

[//]: # (```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # (- [`:help luaeval&#40;&#41;`]&#40;https://neovim.io/doc/user/lua.html#luaeval&#40;&#41;&#41;)

[//]: # ()
[//]: # (### v:lua)

[//]: # ()
[//]: # (这个全局 Vim 变量允许您直接从 Vimscript 调用全局 Lua 函数（[`_G`]&#40;https://www.lua.org/manual/5.1/manual.html#pdf-_G&#41;）。同样 Vim 数据类型被转换为 Lua 类型，反之亦然。)

[//]: # ()
[//]: # (```vim)

[//]: # (call v:lua.print&#40;'Hello from Lua!'&#41;)

[//]: # (" 'Hello from Lua!')

[//]: # ()
[//]: # (let scream = v:lua.string.rep&#40;'A', 10&#41;)

[//]: # (echo scream)

[//]: # (" 'AAAAAAAAAA')

[//]: # ()
[//]: # (" Requiring modules works)

[//]: # (call v:lua.require&#40;'mymodule'&#41;.myfunction&#40;&#41;)

[//]: # ()
[//]: # (" How about a nice statusline?)

[//]: # (lua << EOF)

[//]: # (function _G.statusline&#40;&#41;)

[//]: # (    local filepath = '%f')

[//]: # (    local align_section = '%=')

[//]: # (    local percentage_through_file = '%p%%')

[//]: # (    return string.format&#40;)

[//]: # (        '%s%s%s',)

[//]: # (        filepath,)

[//]: # (        align_section,)

[//]: # (        percentage_through_file)

[//]: # (    &#41;)

[//]: # (end)

[//]: # (EOF)

[//]: # ()
[//]: # (set statusline=%!v:lua.statusline&#40;&#41;)

[//]: # ()
[//]: # (" Also works in expression mappings)

[//]: # (lua << EOF)

[//]: # (function _G.check_back_space&#40;&#41;)

[//]: # (    local col = vim.fn.col&#40;'.'&#41; - 1)

[//]: # (    if col == 0 or vim.fn.getline&#40;'.'&#41;:sub&#40;col, col&#41;:match&#40;'%s'&#41; then)

[//]: # (        return true)

[//]: # (    else)

[//]: # (        return false)

[//]: # (    end)

[//]: # (end)

[//]: # (EOF)

[//]: # ()
[//]: # (inoremap <silent> <expr> <Tab>)

[//]: # (    \ pumvisible&#40;&#41; ? '\<C-n>' :)

[//]: # (    \ v:lua.check_back_space&#40;&#41; ? '\<Tab>' :)

[//]: # (    \ completion#trigger_completion&#40;&#41;)

[//]: # ()
[//]: # (" 通过使用单引号包围模块名并省略 require 的括号来调用 Lua 模块中的函数)

[//]: # (call v:lua.require'module'.foo&#40;&#41;)

[//]: # (```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # (- [`:help v:lua`]&#40;https://neovim.io/doc/user/eval.html#v:lua&#41;)

[//]: # (- [`:help v:lua-call`]&#40;https://neovim.io/doc/user/lua.html#v:lua-call&#41;)

[//]: # ()
[//]: # (#### Caveats)

[//]: # ()
[//]: # (`v:lua` 变量只能用于调用函数。以下代码将始终引发错误：)

[//]: # ()
[//]: # (```vim)

[//]: # (" 不适用于别名一个函数)

[//]: # (let LuaPrint = v:lua.print)

[//]: # ()
[//]: # (" 不适用于访问 dict)

[//]: # (echo v:lua.some_global_dict['key'])

[//]: # ()
[//]: # (" 不适用于将函数作为值使用)

[//]: # (echo map&#40;[1, 2, 3], v:lua.global_callback&#41;)

[//]: # (```)

[//]: # ()
[//]: # (#### 提示)

[//]: # ()
[//]: # (在配置文件中，可以通过设置 `let g:vimsyn_embed = 'l'` 实现 .vim 文件中的 Lua 语法高亮。关于此选项的更多信息请参见 [`:help g:vimsyn_embed`]&#40;https://neovim.io/doc/user/syntax.html#g:vimsyn_embed&#41;)

[//]: # ()
[//]: # (## Vim 命名空间)

[//]: # ()
[//]: # (Neovim 会暴露一个全局的 `vim` 变量来作为 Lua 调用 Vim 的 APIs 的入口。它还提供给用户一些额外的函数和子模块“标准库”)

[//]: # ()
[//]: # (一些比较实用的函数和子模块如下：)

[//]: # ()
[//]: # (- `vim.inspect`: 把 Lua 对象以更易读的方式打印（在打印 Lua table 时会很有用）)

[//]: # (- `vim.regex`: 在 Lua 中使用 Vim 正则表达式)

[//]: # (- `vim.api`: vim 暴露的 API &#40;`:h API`&#41; 模块（别的远程调用也是调用同样的 API&#41;)

[//]: # (- `vim.ui`: 可被插件覆写的 UI 相关函数)

[//]: # (- `vim.loop`: Neovim 的 event loop 模块（使用 LibUV&#41;)

[//]: # (- `vim.lsp`: 控制内置 LSP 客户端的模块)

[//]: # (- `vim.treesitter`: 暴露 tree-sitter 库中一些实用函数的模块)

[//]: # ()
[//]: # (上面列举功能的并不全面。如果你想知道更多可行的操作可以参见：[`:help lua-stdlib`]&#40;https://neovim.io/doc/user/lua.html#lua-stdlib&#41; 和 [`:help lua-vim`]&#40;https://neovim.io/doc/user/lua.html#lua-vim&#41;。你也可以通过 `:lua print&#40;vim.inspect&#40;vim&#41;&#41;` 获得所有可用模块。API 函数的详细文档请参见 [`:help api-global`]&#40;https://neovim.io/doc/user/api.html#api-global&#41;)

[//]: # ()
[//]: # (#### Tips)

[//]: # ()
[//]: # (每次你想检查一个对象时到要用 `print&#40;vim.inspect&#40;x&#41;&#41;` 是相当繁琐的。你可以你的配置中写一个全局的包装器函数来替代这个繁琐的过程（在 Neovim 0.7.0+ 中，你可以使用内建的 `vim.pretty_print&#40;&#41;` ，请参见 [`:help vim.pretty_print&#40;&#41;`]&#40;https://neovim.io/doc/user/lua.html#vim.pretty_print&#40;&#41;&#41;）)

[//]: # ()
[//]: # (```lua)

[//]: # (function _G.put&#40;...&#41;)

[//]: # (    local objects = {})

[//]: # (    for i = 1, select&#40;'#', ...&#41; do)

[//]: # (        local v = select&#40;i, ...&#41;)

[//]: # (        table.insert&#40;objects, vim.inspect&#40;v&#41;&#41;)

[//]: # (     end)

[//]: # ()
[//]: # (     print&#40;table.concat&#40;objects, '\n'&#41;&#41;)

[//]: # (     return ...)

[//]: # (end)

[//]: # (```)

[//]: # ()
[//]: # (之后你就可以使用如下命令来快速检查对象内容了)

[//]: # ()
[//]: # (```lua)

[//]: # (put&#40;{1, 2, 3}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (```vim)

[//]: # (:lua put&#40;vim.loop&#41;)

[//]: # (```)

[//]: # ()
[//]: # (此外，你也可以使用 `:lua` 命令通过在 Lua 表达式前加上 `=` 来美观地打印它)

[//]: # ()
[//]: # (```vim)

[//]: # (:lua =vim.loop)

[//]: # (```)

[//]: # ()
[//]: # (另外要注意的是，你可能会发现 Lua 会比其他语言少一些实用的内置函数（例如：`os.clock&#40;&#41;`，返回以秒为单位，而不是以毫秒为单位的值）。仔细阅读 Neovim 提供的标准库和 `vim.fn` （后续还会有更多内容），里面可以会有你想要的东西。)

[//]: # ()
[//]: # (## 在 Lua 中使用 Vimscript)

[//]: # ()
[//]: # (### vim.api.nvim_eval&#40;&#41;)

[//]: # ()
[//]: # (此函数计算 Vimscript 表达式字符串并返回其值。Vimscript 数据类型自动转换为 Lua 类型（反之亦然）。)

[//]: # ()
[//]: # (它等同于 vimscript 中的 `luaeval&#40;&#41;` 函数)

[//]: # ()
[//]: # (```lua)

[//]: # (-- Data types are converted correctly)

[//]: # (print&#40;vim.api.nvim_eval&#40;'1 + 1'&#41;&#41; -- 2)

[//]: # (print&#40;vim.inspect&#40;vim.api.nvim_eval&#40;'[1, 2, 3]'&#41;&#41;&#41; -- { 1, 2, 3 })

[//]: # (print&#40;vim.inspect&#40;vim.api.nvim_eval&#40;'{"foo": "bar", "baz": "qux"}'&#41;&#41;&#41; -- { baz = "qux", foo = "bar" })

[//]: # (print&#40;vim.api.nvim_eval&#40;'v:true'&#41;&#41; -- true)

[//]: # (print&#40;vim.api.nvim_eval&#40;'v:null'&#41;&#41; -- nil)

[//]: # (```)

[//]: # ()
[//]: # (#### Caveats)

[//]: # ()
[//]: # (与 `luaeval&#40;&#41;` 不同，`vim.api.nvim_eval&#40;&#41;` 不提供隐式 `_A` 变量来传递数据给表达式。)

[//]: # ()
[//]: # (### vim.api.nvim_exec&#40;&#41;)

[//]: # ()
[//]: # (此函数用于计算 Vimscript 代码块。它接受一个包含要执行的源代码的字符串和一个布尔值，以确定代码的输出是否应该由函数返回（例如，您可以将输出存储在变量中）。)

[//]: # ()
[//]: # (```lua)

[//]: # (local result = vim.api.nvim_exec&#40;)

[//]: # ([[)

[//]: # (let mytext = 'hello world')

[//]: # ()
[//]: # (function! MyFunction&#40;text&#41;)

[//]: # (    echo a:text)

[//]: # (endfunction)

[//]: # ()
[//]: # (call MyFunction&#40;mytext&#41;)

[//]: # (]],)

[//]: # (true&#41;)

[//]: # ()
[//]: # (print&#40;result&#41; -- 'hello world')

[//]: # (```)

[//]: # ()
[//]: # (#### Caveats)

[//]: # ()
[//]: # (在 Neovim 0.6.0 之前，`nvim_exec` 不支持 script-local 的变量（`s:`） 。)

[//]: # ()
[//]: # (### vim.api.nvim_command&#40;&#41;)

[//]: # ()
[//]: # (此函数执行一个 EX 命令。它接受包含要执行的命令的字符串。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_command&#40;'new'&#41;)

[//]: # (vim.api.nvim_command&#40;'wincmd H'&#41;)

[//]: # (vim.api.nvim_command&#40;'set nonumber'&#41;)

[//]: # (vim.api.nvim_command&#40;'%s/foo/bar/g'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (### vim.cmd&#40;&#41;)

[//]: # ()
[//]: # (`vim.api.nvim_exec&#40;&#41;`  的别名。只需要命令部分的参数，`output` 参数始终为 `false`)

[//]: # ()
[//]: # (```vim)

[//]: # (vim.cmd&#40;'buffers'&#41;)

[//]: # (vim.cmd&#40;[[)

[//]: # (let g:multiline_list = [)

[//]: # (            \ 1,)

[//]: # (            \ 2,)

[//]: # (            \ 3,)

[//]: # (            \ ])

[//]: # ()
[//]: # (echo g:multiline_list)

[//]: # (]]&#41;)

[//]: # (```)

[//]: # ()
[//]: # (#### Tips)

[//]: # ()
[//]: # (由于您必须将字符串传递给这些函数，因此通常需要添加转义的反斜杠：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.cmd&#40;'%s/\\Vfoo/bar/g'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (非转义字符串更方便使用，它们不需要额外的转义反斜杠：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.cmd&#40;[[%s/\Vfoo/bar/g]]&#41;)

[//]: # (```)

[//]: # ()
[//]: # (### vim.api.nvim_replace_termcodes&#40;&#41;)

[//]: # ()
[//]: # (这个 API 函数允许你转义终端代码和 Vim 键码。)

[//]: # ()
[//]: # (你可能见过这样的映射：)

[//]: # ()
[//]: # (```vim)

[//]: # (inoremap <expr> <Tab> pumvisible&#40;&#41; ? "\<C-N>" : "\<Tab>")

[//]: # (```)

[//]: # ()
[//]: # (在 Lua 中实现相同的功能是比较困难的。你可能会像这样实现)

[//]: # ()
[//]: # (```lua)

[//]: # (function _G.smart_tab&#40;&#41;)

[//]: # (    return vim.fn.pumvisible&#40;&#41; == 1 and [[\<C-N>]] or [[\<Tab>]])

[//]: # (end)

[//]: # ()
[//]: # (vim.api.nvim_set_keymap&#40;'i', '<Tab>', 'v:lua.smart_tab&#40;&#41;', {expr = true, noremap = true}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (然后你会发现这样实现上述映射只会单纯插入 `\<Tab>` 和 `\<C-N>` 两个字符串字面量……)

[//]: # ()
[//]: # (能够转义键码实际上是 Vimscript 的一项功能。除了许多编程语言常用的转义序列（如  `\r`、`\42` 或 `\x10`）外，Vimscript 中的 `expr-quotes`（用双引号括起来的字符串）允许你转义 Vim 键码的易读形式。)

[//]: # ()
[//]: # (Lua 中并没有内置这种功能。幸运的是，Neovim 提供了一个用来转义终端代码和 Vim 键码的 API 函数：`nvim_replace_termcodes&#40;&#41;`)

[//]: # ()
[//]: # (```lua)

[//]: # (print&#40;vim.api.nvim_replace_termcodes&#40;'<Tab>', true, true, true&#41;&#41;)

[//]: # (```)

[//]: # ()
[//]: # (这样调用过于冗长，我们可以创建一个方便重复调用的包装：)

[//]: # ()
[//]: # (```lua)

[//]: # (-- The function is called `t` for `termcodes`.)

[//]: # (-- You don't have to call it that, but I find the terseness convenient)

[//]: # (local function t&#40;str&#41;)

[//]: # (    -- Adjust boolean arguments as needed)

[//]: # (    return vim.api.nvim_replace_termcodes&#40;str, true, true, true&#41;)

[//]: # (end)

[//]: # ()
[//]: # (print&#40;t'<Tab>'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (回到先前的例子，我们可以这样实现那个映射)

[//]: # ()
[//]: # (```lua)

[//]: # (local function t&#40;str&#41;)

[//]: # (    return vim.api.nvim_replace_termcodes&#40;str, true, true, true&#41;)

[//]: # (end)

[//]: # ()
[//]: # (function _G.smart_tab&#40;&#41;)

[//]: # (    return vim.fn.pumvisible&#40;&#41; == 1 and t'<C-N>' or t'<Tab>')

[//]: # (end)

[//]: # ()
[//]: # (vim.api.nvim_set_keymap&#40;'i', '<Tab>', 'v:lua.smart_tab&#40;&#41;', {expr = true, noremap = true}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (如果我们使用 `vim.keymap.set&#40;&#41;` 函数（Neovim 0.7.0+）来设置的话就不需要转义键码，`vim.keymap.set&#40;&#41;` 默认会自动转义返回值中的键码（`expr` 选项设置为 `true`）：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.keymap.set&#40;'i', '<Tab>', function&#40;&#41;)

[//]: # (    return vim.fn.pumvisible&#40;&#41; == 1 and '<C-N>' or '<Tab>')

[//]: # (end, {expr = true}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (* [`:help keycodes`]&#40;https://neovim.io/doc/user/intro.html#keycodes&#41;)

[//]: # (* [`:help expr-quote`]&#40;https://neovim.io/doc/user/eval.html#expr-quote&#41;)

[//]: # (* [`:help nvim_replace_termcodes&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_replace_termcodes&#40;&#41;&#41;)

[//]: # ()
[//]: # (## 管理 vim 的设置选项)

[//]: # ()
[//]: # (### 使用 api 函数)

[//]: # ()
[//]: # (Neovim 提供了一组 API 函数来设置选项或获取其当前值：)

[//]: # ()
[//]: # (- 全局选项：)

[//]: # (    - `vim.api.nvim_set_option&#40;&#41;`)

[//]: # (    - `vim.api.nvim_get_option&#40;&#41;`)

[//]: # (- 缓冲区选项：)

[//]: # (    - `vim.api.nvim_buf_set_option&#40;&#41;`)

[//]: # (    - `vim.api.nvim_buf_get_option&#40;&#41;`)

[//]: # (- 窗口选项：)

[//]: # (    - `vim.api.nvim_win_set_option&#40;&#41;`)

[//]: # (    - `vim.api.nvim_win_get_option&#40;&#41;`)

[//]: # ()
[//]: # (它们接受一个字符串，其中包含要设置或者要获取的选项的名称以及要将其设置为的值。)

[//]: # ()
[//]: # (布尔选项（如 `&#40;no&#41;number`&#41; 必须设置为 `true` 或 `false`：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_set_option&#40;'smarttab', false&#41;)

[//]: # (print&#40;vim.api.nvim_get_option&#40;'smarttab'&#41;&#41; -- false)

[//]: # (```)

[//]: # ()
[//]: # (字符串选项必须设置为字符串：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_set_option&#40;'selection', 'exclusive'&#41;)

[//]: # (print&#40;vim.api.nvim_get_option&#40;'selection'&#41;&#41; -- 'exclusive')

[//]: # (```)

[//]: # ()
[//]: # (数字选项必须接受数字类型)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_set_option&#40;'updatetime', 3000&#41;)

[//]: # (print&#40;vim.api.nvim_get_option&#40;'updatetime'&#41;&#41; -- 3000)

[//]: # (```)

[//]: # ()
[//]: # (Buffer-local 和 Window-local 选项还需要缓冲区编号或窗口编号（使用 `0` 将设置 / 获取当前缓冲区 / 窗口的选项）：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_win_set_option&#40;0, 'number', true&#41;)

[//]: # (vim.api.nvim_buf_set_option&#40;10, 'shiftwidth', 4&#41;)

[//]: # (print&#40;vim.api.nvim_win_get_option&#40;0, 'number'&#41;&#41; -- true)

[//]: # (print&#40;vim.api.nvim_buf_get_option&#40;10, 'shiftwidth'&#41;&#41; -- 4)

[//]: # (```)

[//]: # ()
[//]: # (### 使用元访问器)

[//]: # ()
[//]: # (如果您想以更“惯用”的方式设置选项，可以使用一些元访问器。它们本质上包装了上述 API 函数，并允许您像处理变量一样操作选项：)

[//]: # ()
[//]: # (- [`vim.o`]&#40;https://neovim.io/doc/user/lua.html#vim.o&#41;: 行为类似于 `:let &{option-name}`)

[//]: # (- [`vim.go`]&#40;https://neovim.io/doc/user/lua.html#vim.go&#41;: 行为类似于 `:let &g:{option-name}`)

[//]: # (- [`vim.bo`]&#40;https://neovim.io/doc/user/lua.html#vim.bo&#41;: 适用于 buffer-local 选项，行为类似于 `:let &l:{option-name}`)

[//]: # (- [`vim.wo`]&#40;https://neovim.io/doc/user/lua.html#vim.wo&#41;: 适用于 window-local 选项，行为类似于 `:let &l:{option-name}`)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.o.smarttab = false -- let &smarttab = v:false)

[//]: # (print&#40;vim.o.smarttab&#41; -- false)

[//]: # (vim.o.isfname = vim.o.isfname .. ',@-@' -- on Linux: let &isfname = &isfname .. ',@-@')

[//]: # (print&#40;vim.o.isfname&#41; -- '@,48-57,/,.,-,_,+,,,#,$,%,~,=,@-@')

[//]: # ()
[//]: # (vim.bo.shiftwidth = 4)

[//]: # (print&#40;vim.bo.shiftwidth&#41; -- 4)

[//]: # (```)

[//]: # ()
[//]: # (您可以为缓冲区本地和窗口本地选项指定一个数字。如果未给出编号，则使用当前缓冲区 / 窗口：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.bo[4].expandtab = true -- same as vim.api.nvim_buf_set_option&#40;4, 'expandtab', true&#41;)

[//]: # (vim.wo.number = true -- same as vim.api.nvim_win_set_option&#40;0, 'number', true&#41;)

[//]: # (```)

[//]: # ()
[//]: # (这些访问器还有更为复杂的 `vim.opt*` 变体，它们为在 Lua 中设置选项提供了更为灵活便利的机制，就像你在 `init.vim` 中使用的 `:set`/`:setglobal`/`:setlocal`:)

[//]: # ()
[//]: # (* `vim.opt`: 行为类似于 `:set`)

[//]: # (* `vim.opt_global`: 行为类似于 `:setglobal`)

[//]: # (* `vim.opt_local`: 行为类似于 `:setlocal`)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.opt.smarttab = false)

[//]: # (print&#40;vim.opt.smarttab:get&#40;&#41;&#41; -- false)

[//]: # (```)

[//]: # ()
[//]: # (一些选项也可以通过 Lua 的 table 来设置：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.opt.completeopt = {'menuone', 'noselect'})

[//]: # (print&#40;vim.inspect&#40;vim.opt.completeopt:get&#40;&#41;&#41;&#41; -- { "menuone", "noselect" })

[//]: # (```)

[//]: # ()
[//]: # (对于类似于 list/map/set 的选项，它们对应的包装器还实现了各种方法与元方法，行为类似于 Vimscript 中的 `:set+=`/`:set^=`/`:set-=`。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.opt.shortmess:append&#40;{ I = true }&#41;)

[//]: # (-- alternative form:)

[//]: # (vim.opt.shortmess = vim.opt.shortmess + { I = true })

[//]: # ()
[//]: # (vim.opt.whichwrap:remove&#40;{ 'b', 's' }&#41;)

[//]: # (-- alternative form:)

[//]: # (vim.opt.whichwrap = vim.opt.whichwrap - { 'b', 's' })

[//]: # (```)

[//]: # ()
[//]: # (关于 `vim.opt` 的更多信息请参见：[`:help vim.opt`]&#40;https://neovim.io/doc/user/lua.html#vim.opt&#41;)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (- [`:help lua-vim-options`]&#40;https://neovim.io/doc/user/lua.html#lua-vim-options&#41;)

[//]: # ()
[//]: # (## 管理 vim 的内部变量)

[//]: # ()
[//]: # (### 使用 api 函数)

[//]: # ()
[//]: # (与选项非常的相似，内部变量也有自己的 api 函数：)

[//]: # ()
[//]: # (- 全局变量 &#40;`g:`&#41;:)

[//]: # (    - `vim.api.nvim_set_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_get_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_del_var&#40;&#41;`)

[//]: # (- 缓冲区变量 &#40;`b:`&#41;:)

[//]: # (    - `vim.api.nvim_buf_set_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_buf_get_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_buf_del_var&#40;&#41;`)

[//]: # (- 窗口变量 &#40;`w:`&#41;:)

[//]: # (    - `vim.api.nvim_win_set_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_win_get_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_win_del_var&#40;&#41;`)

[//]: # (- 选项卡变量 &#40;`t:`&#41;:)

[//]: # (    - `vim.api.nvim_tabpage_set_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_tabpage_get_var&#40;&#41;`)

[//]: # (    - `vim.api.nvim_tabpage_del_var&#40;&#41;`)

[//]: # (- 预定义的 vim 变量 &#40;`v:`&#41;:)

[//]: # (    - `vim.api.nvim_set_vvar&#40;&#41;`)

[//]: # (    - `vim.api.nvim_get_vvar&#40;&#41;`)

[//]: # ()
[//]: # (除了预定义的 Vim 变量外，还可以删除它们（等同于 Vimscript 中的 `:unlet`&#41;。局部变量 &#40;`l:`&#41;、脚本变量 &#40;`s:`&#41; 和函数参数 &#40;`a:`&#41; 不能操作，因为它们只在 Vim 脚本上下文中有意义，Lua 有自己的作用域规则。)

[//]: # (如果您不熟悉这些变量的作用，请参考 [`:help internal-variables`]&#40;https://neovim.io/doc/user/eval.html#internal-variables&#41; 对其进行详细介绍。)

[//]: # (这些函数接受一个字符串，该字符串包含要设置 / 获取 / 删除的变量的名称以及要将其设置为的值。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_set_var&#40;'some_global_variable', { key1 = 'value', key2 = 300 }&#41;)

[//]: # (print&#40;vim.inspect&#40;vim.api.nvim_get_var&#40;'some_global_variable'&#41;&#41;&#41; -- { key1 = "value", key2 = 300 })

[//]: # (vim.api.nvim_del_var&#40;'some_global_variable'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (范围为缓冲区、窗口或选项卡的变量会接受一个数字类型的参数 &#40;0 意味着设置 / 获取 / 删除当前缓冲区 / 窗口 / 选项卡页的变量）：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_win_set_var&#40;0, 'some_window_variable', 2500&#41;)

[//]: # (vim.api.nvim_tab_set_var&#40;3, 'some_tabpage_variable', 'hello world'&#41;)

[//]: # (print&#40;vim.api.nvim_win_get_var&#40;0, 'some_window_variable'&#41;&#41; -- 2500)

[//]: # (print&#40;vim.api.nvim_buf_get_var&#40;3, 'some_tabpage_variable'&#41;&#41; -- 'hello world')

[//]: # (vim.api.nvim_win_del_var&#40;0, 'some_window_variable'&#41;)

[//]: # (vim.api.nvim_buf_del_var&#40;3, 'some_tabpage_variable'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (### 使用元访问器)

[//]: # ()
[//]: # (使用这些元访问器可以更直观地操作内部变量：)

[//]: # ()
[//]: # (- [`vim.g`]&#40;https://neovim.io/doc/user/lua.html#vim.g&#41;: 全局变量)

[//]: # (- [`vim.b`]&#40;https://neovim.io/doc/user/lua.html#vim.b&#41;: 缓冲区变量)

[//]: # (- [`vim.w`]&#40;https://neovim.io/doc/user/lua.html#vim.w&#41;: 窗口变量)

[//]: # (- [`vim.t`]&#40;https://neovim.io/doc/user/lua.html#vim.t&#41;: 选项卡变量)

[//]: # (- [`vim.v`]&#40;https://neovim.io/doc/user/lua.html#vim.v&#41;: 预定义变量)

[//]: # (- [`vim.env`]&#40;https://neovim.io/doc/user/lua.html#vim.env&#41;: 环境变量)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.g.some_global_variable = {)

[//]: # (    key1 = 'value',)

[//]: # (    key2 = 300)

[//]: # (})

[//]: # ()
[//]: # (print&#40;vim.inspect&#40;vim.g.some_global_variable&#41;&#41; -- { key1 = "value", key2 = 300 })

[//]: # ()
[//]: # (-- 针对特定 buffer/window/tabpage 的变量 &#40;Neovim 0.6+&#41;)

[//]: # (vim.b[2].myvar = 1)

[//]: # (```)

[//]: # ()
[//]: # (一些变量名可能包含不能在 Lua 中用作标识符的字符。你可以使用以下语法操作这些变量：`vim.g['my#variable']`。)

[//]: # ()
[//]: # (> 在 Lua 中，`some_table.some_item` 本质上是 `some_table["some_item"]` 的语法糖，所以`vim.g['my#variable']` 也可以写为 `vim['g']['my#variable']`)

[//]: # (>)

[//]: # (> —— 译者注)

[//]: # ()
[//]: # (删除变量只需要将它的值设置为 nil)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.g.some_global_variable = nil)

[//]: # (```)

[//]: # ()
[//]: # (更多信息参见：)

[//]: # ()
[//]: # (* [`:help lua-vim-variables`]&#40;https://neovim.io/doc/user/lua.html#lua-vim-variables&#41;)

[//]: # ()
[//]: # (#### Caveats)

[//]: # ()
[//]: # (您不能从存储在这些变量之一的字典中添加 / 更新 / 删除键。例如，这段 Vimscript 代码不能按预期工作：)

[//]: # ()
[//]: # (```vim)

[//]: # (let g:variable = {})

[//]: # (lua vim.g.variable.key = 'a')

[//]: # (echo g:variable)

[//]: # (" {})

[//]: # (```)

[//]: # ()
[//]: # (可以使用一个临时变量来解决)

[//]: # ()
[//]: # (```vim)

[//]: # (let g:variable = {})

[//]: # (lua << EOF)

[//]: # (local tmp = vim.g.variable)

[//]: # (tmp.key = 'a')

[//]: # (vim.g.variable = tmp)

[//]: # (EOF)

[//]: # (echo g:variable)

[//]: # (" {'key': 'a'})

[//]: # (```)

[//]: # ()
[//]: # (这是个已知的问题)

[//]: # ()
[//]: # (- [Issue #12544]&#40;https://github.com/neovim/neovim/issues/12544&#41;)

[//]: # ()
[//]: # (## 调用 Vimscript 函数)

[//]: # ()
[//]: # (### vim.fn.{function}&#40;&#41;)

[//]: # ()
[//]: # (`vim.fn` 可以用来调用 Vimscript 函数。数据类型在 Lua 和 Vimscript 之间自动转换。)

[//]: # ()
[//]: # (```lua)

[//]: # (print&#40;vim.fn.printf&#40;'Hello from %s', 'Lua'&#41;&#41;)

[//]: # ()
[//]: # (local reversed_list = vim.fn.reverse&#40;{ 'a', 'b', 'c' }&#41;)

[//]: # (print&#40;vim.inspect&#40;reversed_list&#41;&#41; -- { "c", "b", "a" })

[//]: # ()
[//]: # (local function print_stdout&#40;chan_id, data, name&#41;)

[//]: # (    print&#40;data[1]&#41;)

[//]: # (end)

[//]: # ()
[//]: # (vim.fn.jobstart&#40;'ls', { on_stdout = print_stdout }&#41;)

[//]: # (```)

[//]: # ()
[//]: # (Hashes `#` 不是 Lua 中识别符的有效字符，因此必须使用以下语法调用 autoload 函数：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.fn['my#autoload#function']&#40;&#41;)

[//]: # (```)

[//]: # ()
[//]: # (`vim.fn` 的功能与 `vim.call` 完全相同，但看起来更像是原生 Lua 函数调用。)

[//]: # ()
[//]: # (和 `vim.api.nvim_call_function` 的不同之处在于，`vim.fn` 中数据类型的转换是自动的：对于浮点数类型，`vim.api.nvim_call_function` 会返回一个 table 并且它不支持 Lua 闭包作为参数；`vim.fn` 可以直接处理这些类型。)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (- [`:help vim.fn`]&#40;https://neovim.io/doc/user/lua.html#vim.fn&#41;)

[//]: # ()
[//]: # (#### Tips)

[//]: # ()
[//]: # (Neovim 有一个强大的内置函数库，这些函数对插件非常有用。按字母顺序排列的函数列表参见 [`:help vim-function`]&#40;https://neovim.io/doc/user/eval.html#vim-function&#41;，按主题分组的函数列表参见[`:help function-list`]&#40;https://neovim.io/doc/user/usr_41.html#function-list&#41;。)

[//]: # ()
[//]: # (Neovim 中的 API 函数可以通过 `vim.api.{..}` 的方式直接调用。更多信息请参见 [`:help api`]&#40;https://neovim.io/doc/user/api.html#API&#41;)

[//]: # ()
[//]: # (#### Caveats)

[//]: # ()
[//]: # (一些应该返回布尔值的 Vim 函数返回 `1` 或 `0`。这在 Vimscript 中不是问题，因为 `1` 是真的，而 `0` 是假的，支持如下结构：)

[//]: # ()
[//]: # (```vim)

[//]: # (if has&#40;'nvim'&#41;)

[//]: # (    " do something...)

[//]: # (endif)

[//]: # (```)

[//]: # ()
[//]: # (然而，在 Lua 中，只有 `false` 和 `nil` 被认为是假的，数字的计算结果总是 `true`，无论它们的值是什么。您必须显式检查 `1` 或 `0`：)

[//]: # ()
[//]: # (```lua)

[//]: # (if vim.fn.has&#40;'nvim'&#41; == 1 then)

[//]: # (    -- do something...)

[//]: # (end)

[//]: # (```)

[//]: # ()
[//]: # (## 定义映射)

[//]: # ()
[//]: # (### API 函数)

[//]: # ()
[//]: # (Neovim 提供了一系列的 api 函数来设置获取和删除映射：)

[//]: # ()
[//]: # (- 全局映射：)

[//]: # (    - `vim.api.nvim_set_keymap&#40;&#41;`)

[//]: # (    - `vim.api.nvim_get_keymap&#40;&#41;`)

[//]: # (    - `vim.api.nvim_del_keymap&#40;&#41;`)

[//]: # (- 缓冲区映射：)

[//]: # (    - `vim.api.nvim_buf_set_keymap&#40;&#41;`)

[//]: # (    - `vim.api.nvim_buf_get_keymap&#40;&#41;`)

[//]: # (    - `vim.api.nvim_buf_del_keymap&#40;&#41;`)

[//]: # ()
[//]: # (让我们从 `vim.api.nvim_set_keymap&#40;&#41;` 和 `vim.api.nvim_buf_set_keymap&#40;&#41;` 开始，传递给函数的第一个参数是一个包含映射生效模式名称的字符串：)

[//]: # ()
[//]: # (| String value           | Help page     | Affected modes                           | Vimscript equivalent |)

[//]: # (|------------------------|---------------|------------------------------------------|----------------------|)

[//]: # (| `''` &#40;an empty string&#41; | `mapmode-nvo` | Normal, Visual, Select, Operator-pending | `:map`               |)

[//]: # (| `'n'`                  | `mapmode-n`   | Normal                                   | `:nmap`              |)

[//]: # (| `'v'`                  | `mapmode-v`   | Visual and Select                        | `:vmap`              |)

[//]: # (| `'s'`                  | `mapmode-s`   | Select                                   | `:smap`              |)

[//]: # (| `'x'`                  | `mapmode-x`   | Visual                                   | `:xmap`              |)

[//]: # (| `'o'`                  | `mapmode-o`   | Operator-pending                         | `:omap`              |)

[//]: # (| `'!'`                  | `mapmode-ic`  | Insert and Command-line                  | `:map!`              |)

[//]: # (| `'i'`                  | `mapmode-i`   | Insert                                   | `:imap`              |)

[//]: # (| `'l'`                  | `mapmode-l`   | Insert, Command-line, Lang-Arg           | `:lmap`              |)

[//]: # (| `'c'`                  | `mapmode-c`   | Command-line                             | `:cmap`              |)

[//]: # (| `'t'`                  | `mapmode-t`   | Terminal                                 | `:tmap`              |)

[//]: # ()
[//]: # (第二个参数是包含映射左侧的字符串（触发映射中定义的命令的键或键集）。空字符串相当于 `<Nop>`，表示禁用键位。)

[//]: # ()
[//]: # (第三个参数是包含映射右侧（要执行的命令）的字符串。)

[//]: # ()
[//]: # (最后一个参数是一个表，包含 [`:help :map-arguments`]&#40;https://neovim.io/doc/user/map.html#:map-arguments&#41; 中定义的映射的布尔值选项（包括 `noremap`，不包括 `buffer`&#41;。)

[//]: # ()
[//]: # (缓冲区 - 本地映射也将缓冲区编号作为其第一个参数 &#40;`0` 设置当前缓冲区的映射）。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_set_keymap&#40;'n', '<leader><Space>', ':set hlsearch!<CR>', { noremap = true, silent = true }&#41;)

[//]: # (-- :nnoremap <silent> <leader><Space> :set hlsearch<CR>)

[//]: # ()
[//]: # (vim.api.nvim_buf_set_keymap&#40;0, '', 'cc', 'line&#40;"."&#41; == 1 ? "cc" : "ggcc"', { noremap = true, expr = true }&#41;)

[//]: # (-- :noremap <buffer> <expr> cc line&#40;'.'&#41; == 1 ? 'cc' : 'ggcc')

[//]: # ()
[//]: # (vim.api.nvim_set_keymap&#40;'n', '<Leader>ex', '', {)

[//]: # (    noremap = true,)

[//]: # (    callback = function&#40;&#41;)

[//]: # (        print&#40;'My example'&#41;)

[//]: # (    end,)

[//]: # (    -- Since Lua function don't have a useful string representation, you can use the "desc" option to document your mapping)

[//]: # (    desc = 'Prints "My example" in the message area',)

[//]: # (}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (`vim.api.nvim_get_keymap&#40;&#41;` 接受一个字符串，该字符串包含您想要映射列表的模式的短名称（见上表）。返回值是包含该模式的所有全局映射的表。)

[//]: # ()
[//]: # (```lua)

[//]: # (print&#40;vim.inspect&#40;vim.api.nvim_get_keymap&#40;'n'&#41;&#41;&#41;)

[//]: # (-- :verbose nmap)

[//]: # (```)

[//]: # ()
[//]: # (`vim.api.nvim_buf_get_keymap&#40;&#41;` 将缓冲区编号作为其第一个参数 &#40;`0` 将获取当前缓冲区的映射）)

[//]: # ()
[//]: # (```lua)

[//]: # (print&#40;vim.inspect&#40;vim.api.nvim_buf_get_keymap&#40;0, 'i'&#41;&#41;&#41;)

[//]: # (-- :verbose imap <buffer>)

[//]: # (```)

[//]: # ()
[//]: # (`vim.api.nvim_del_keymap&#40;&#41;` 获取映射左侧的模式。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_del_keymap&#40;'n', '<leader><Space>'&#41;)

[//]: # (-- :nunmap <leader><Space>)

[//]: # (```)

[//]: # ()
[//]: # (同样，`vim.api.nvim_buf_del_keymap&#40;&#41;` 以缓冲区编号作为第一个参数，其中 `0` 表示当前缓冲区。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_buf_del_keymap&#40;0, 'i', '<Tab>'&#41;)

[//]: # (-- :iunmap <buffer> <Tab>)

[//]: # (```)

[//]: # ()
[//]: # (### vim.keymap)

[//]: # ()
[//]: # (:warning: 本节讨论的函数仅适用于 Neovim 0.7.0+)

[//]: # ()
[//]: # (Neovim 提供了两个函数来设置 / 删除映射：)

[//]: # ()
[//]: # (* [`vim.keymap.set&#40;&#41;`]&#40;https://neovim.io/doc/user/lua.html#vim.keymap.set&#40;&#41;&#41;)

[//]: # (* [`vim.keymap.del&#40;&#41;`]&#40;https://neovim.io/doc/user/lua.html#vim.keymap.del&#40;&#41;&#41;)

[//]: # ()
[//]: # (它们是上述 API 函数的语法糖版本。)

[//]: # ()
[//]: # (`vim.keymap.set&#40;&#41;` 第一个参数是字符串，代表映射生效的模式，也可以是一个字符串 table，这样可以一次性定义多个模式下的映射：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.keymap.set&#40;'n', '<Leader>ex1', '<Cmd>lua vim.notify&#40;"Example 1"&#41;<CR>'&#41;)

[//]: # (vim.keymap.set&#40;{'n', 'c'}, '<Leader>ex2', '<Cmd>lua vim.notify&#40;"Example 2"&#41;<CR>'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (第二个参数是映射的左侧，是字符串类型)

[//]: # ()
[//]: # (第三个参数是映射的右侧，既可以是字符串也可以是一个 Lua 函数：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.keymap.set&#40;'n', '<Leader>ex1', '<Cmd>echomsg "Example 1"<CR>'&#41;)

[//]: # (vim.keymap.set&#40;'n', '<Leader>ex2', function&#40;&#41; print&#40;"Example 2"&#41; end&#41;)

[//]: # (vim.keymap.set&#40;'n', '<Leader>pl1', require&#40;'plugin'&#41;.plugin_action&#41;)

[//]: # (-- To avoid the startup cost of requiring the module, you can wrap it in a function to require it lazily when invoking the mapping:)

[//]: # (vim.keymap.set&#40;'n', '<Leader>pl2', function&#40;&#41; require&#40;'plugin'&#41;.plugin_action&#40;&#41; end&#41;)

[//]: # (```)

[//]: # ()
[//]: # (第四个参数是是一个选项的 table，它是可选的，对应于传递给 `vim.apt.nvim_set_keymap&#40;&#41;` 的选项，并添加了一些内容（完整列表请参见 [`:help vim.keymap.set&#40;&#41;`]&#40;https://neovim.io/doc/user/lua.html#vim.keymap.set&#40;&#41;&#41;）。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.keymap.set&#40;'n', '<Leader>ex1', '<Cmd>echomsg "Example 1"<CR>', {buffer = true}&#41;)

[//]: # (vim.keymap.set&#40;'n', '<Leader>ex2', function&#40;&#41; print&#40;'Example 2'&#41; end, {desc = 'Prints "Example 2" to the message area'}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (使用 Lua 函数定义映射不同于使用字符串。像 `:nmap <Leader>ex1`  之类的显示映射信息的常用方法不会输出有用的信息（映射到的字符串本身），而只会显示 Lua 函数。建议添加一个 `desc` 项来描述你的按键映射的行为。这对于记录插件映射尤其重要，用户可以更轻松地了解按键映射的用法。)

[//]: # ()
[//]: # (这个 API 的有用之处在于它消除了 Vim 映射的历史遗留问题：)

[//]: # ()
[//]: # (* 映射默认是 `noremap` 的，除非 `rhs` 是一个 `<Plug>` 映射。这意味着你很少需要考虑映射是否应该是递归的：)

[//]: # ()
[//]: # (    ```lua)

[//]: # (    vim.keymap.set&#40;'n', '<Leader>test1', '<Cmd>echo "test"<CR>'&#41;)

[//]: # (    -- :nnoremap <Leader>test <Cmd>echo "test"<CR>)

[//]: # ()
[//]: # (    -- 如果你确定要设置一个递归的映射，把 "remap" 选项设置为 "true")

[//]: # (    vim.keymap.set&#40;'n', '>', ']', {remap = true}&#41;)

[//]: # (    -- :nmap > ])

[//]: # ()
[//]: # (    -- 除非是递归映射，否则 <Plug> 映射不会起作用，vim.keymap.set&#40;&#41; 会自动为你处理)

[//]: # (    vim.keymap.set&#40;'n', '<Leader>plug', '<Plug>&#40;plugin&#41;'&#41;)

[//]: # (    -- :nmap <Leader>plug <Plug>&#40;plugin&#41;)

[//]: # (    ```)

[//]: # ()
[//]: # (* 在 `expr` 映射中，`nvim_replace_termcodes&#40;&#41;` 会被自动应用于 Lua 函数返回的字符串)

[//]: # ()
[//]: # (    ```lua)

[//]: # (    vim.keymap.set&#40;'i', '<Tab>', function&#40;&#41;)

[//]: # (        return vim.fn.pumvisible == 1 and '<C-N>' or '<Tab>')

[//]: # (    end, {expr = true}&#41;)

[//]: # (    ```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (* [`:help recursive_mapping`]&#40;https://neovim.io/doc/user/map.html#recursive_mapping&#41;)

[//]: # ()
[//]: # (`vim.keymap.del&#40;&#41;` 工作原理相同，但是效果是删除映射：)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.keymap.del&#40;'n', '<Leader>ex1'&#41;)

[//]: # (vim.keymap.del&#40;{'n', 'c'}, '<Leader>ex2', {buffer = true}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (## 定义用户命令)

[//]: # ()
[//]: # (:warning: 本节讨论的 API 函数仅适用于 Neovim 0.7.0+)

[//]: # ()
[//]: # (Neovim 提供了如下 API 函数来创建用户命令)

[//]: # ()
[//]: # (* 全局用户命令)

[//]: # (    * [`vim.api.nvim_create_user_command&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_create_user_command&#40;&#41;&#41;)

[//]: # (    * [`vim.api.nvim_del_user_command&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_del_user_command&#40;&#41;&#41;)

[//]: # (* Buffer-local 的用户命令)

[//]: # (    * [`vim.api.nvim_buf_create_user_command&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_buf_create_user_command&#40;&#41;&#41;)

[//]: # (    * [`vim.api.nvim_buf_del_user_command&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_buf_del_user_command&#40;&#41;&#41;)

[//]: # ()
[//]: # (以 `vim.api.nvim_create_user_command&#40;&#41;` 为例说明用法)

[//]: # ()
[//]: # (此函数的第一个参数是命令的名字（必须以大写字母开头）。)

[//]: # ()
[//]: # (第二个参数是调用该命令时要执行的代码。它可以是：)

[//]: # ()
[//]: # (一个字符串（在这种情况下它将作为 Vimscript 执行）。你可以像使用 `:command` 一样使用转义序列，譬如 `<q-args>`、`<range>` 等)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_create_user_command&#40;'Upper', 'echo toupper&#40;<q-args>&#41;', { nargs = 1 }&#41;)

[//]: # (-- :command! -nargs=1 Upper echo toupper&#40;<q-args>&#41;)

[//]: # ()
[//]: # (vim.cmd&#40;'Upper hello world'&#41; -- prints "HELLO WORLD")

[//]: # (```)

[//]: # ()
[//]: # (或者是一个 Lua 函数。它接受一个类似字典的 table，其中包含通常是由转义序列提供的数据（可用的所有键请参见 [`:help nvim_create_user_command&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_create_user_command&#40;&#41;&#41;）)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_create_user_command&#40;)

[//]: # (    'Upper',)

[//]: # (    function&#40;opts&#41;)

[//]: # (        print&#40;string.upper&#40;opts.args&#41;&#41;)

[//]: # (    end,)

[//]: # (    { nargs = 1 })

[//]: # (&#41;)

[//]: # (```)

[//]: # ()
[//]: # (第三个参数是一个包含命令属性的 table（请参见 [`:help command-attributes`]&#40;https://neovim.io/doc/user/map.html#command-attributes&#41;）。值得注意的是，由于你可以使用 `vim.api.nvim_buf_create_user_command&#40;&#41;` 来创建 buffer-local 的用户命令，所以 `-buffer` 不是一个有效的属性。)

[//]: # ()
[//]: # (此外还有两种属性可用：)

[//]: # ()
[//]: # (* `desc` 属性是你在定义为 Lua 回调函数的命令上运行 `:command {cmd}` 时显示的内容。与上文中的键盘映射类似，我们建议在定义为 Lua 函数的命令中加入此属性。)

[//]: # (* `force` 属性相当于调用 `:command!` 并且替换已经存在的同名命令。与 Vimscript 不同，它的默认值为 `true`)

[//]: # ()
[//]: # (除了 [`:help :command-complete`]&#40;https://neovim.io/doc/user/map.html#:command-complete&#41; 中列出的属性之外，`-complete` 还可以设置为一个 Lua 函数。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_create_user_command&#40;'Upper', function&#40;&#41; end, {)

[//]: # (    nargs = 1,)

[//]: # (    complete = function&#40;ArgLead, CmdLine, CursorPos&#41;)

[//]: # (        -- return completion candidates as a list-like table)

[//]: # (        return { 'foo', 'bar', 'baz' })

[//]: # (    end,)

[//]: # (}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (Buffer-local 的用户命令把 buffer 编号作为第一个参数。这比 `-buffer` 更优，后者只能为当前的 buffer 定义命令。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_buf_create_user_command&#40;4, 'Upper', function&#40;&#41; end, {}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (`vim.api.nvim_del_user_command&#40;&#41;` 的参数是要删除命令的名字。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_del_user_command&#40;'Upper'&#41;)

[//]: # (-- :delcommand Upper)

[//]: # (```)

[//]: # ()
[//]: # (同样的，`vim.api.nvim_buf_del_user_command&#40;&#41;` 也把 buffer 编号作为第一个参数，`0` 代表当前的 buffer。)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_buf_del_user_command&#40;4, 'Upper'&#41;)

[//]: # (```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (* [`:help nvim_create_user_command`]&#40;https://neovim.io/doc/user/api.html#nvim_create_user_command&#40;&#41;&#41;)

[//]: # (* [`:help 40.2`]&#40;https://neovim.io/doc/user/usr_40.html#40.2&#41;)

[//]: # (* [`:help command-attributes`]&#40;https://neovim.io/doc/user/map.html#command-attributes&#41;)

[//]: # ()
[//]: # (### Caveats)

[//]: # ()
[//]: # (`-complete=custom` 属性自动选出合适的候选补全并且支持内置的通配符（[`:help wildcard`]&#40;https://neovim.io/doc/user/editing.html#wildcard&#41;）)

[//]: # ()
[//]: # (```vim)

[//]: # (function! s:completion_function&#40;ArgLead, CmdLine, CursorPos&#41; abort)

[//]: # (    return join&#40;[)

[//]: # (        \ 'strawberry',)

[//]: # (        \ 'star',)

[//]: # (        \ 'stellar',)

[//]: # (        \ ], "\n"&#41;)

[//]: # (endfunction)

[//]: # ()
[//]: # (command! -nargs=1 -complete=custom,s:completion_function Test echo <q-args>)

[//]: # (" Typing `:Test st[ae]<Tab>` returns "star" and "stellar")

[//]: # (```)

[//]: # ()
[//]: # (把 `complete` 设置为 Lua 函数的话，它的行为会类似于 `customlist` ，Neovim 不会筛选函数返回的候选列表)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_create_user_command&#40;'Test', function&#40;&#41; end, {)

[//]: # (    nargs = 1,)

[//]: # (    complete = function&#40;ArgLead, CmdLine, CursorPos&#41;)

[//]: # (        return {)

[//]: # (            'strawberry',)

[//]: # (            'star',)

[//]: # (            'stellar',)

[//]: # (        })

[//]: # (    end,)

[//]: # (}&#41;)

[//]: # ()
[//]: # (-- Typing `:Test z<Tab>` returns all the completion results because the list was not filtered)

[//]: # (```)

[//]: # ()
[//]: # (## 定义自动命令)

[//]: # ()
[//]: # (（本节内容尚未完成）)

[//]: # ()
[//]: # (Neovim 0.7.0 为创建自动命令提供了相应的 API 函数。更多细节请参见 `:help api-autocmd`)

[//]: # ()
[//]: # (* [Pull request #14661]&#40;https://github.com/neovim/neovim/pull/14661&#41; &#40;lua: autocmds take 2&#41;)

[//]: # ()
[//]: # (## 定义语法高亮)

[//]: # ()
[//]: # (（本节内容尚未完成）)

[//]: # ()
[//]: # (Neovim 0.7.0 为处理高亮组提供了相应的 API 函数。更多信息请参见：)

[//]: # ()
[//]: # (* [`:help nvim_set_hl&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_set_hl&#40;&#41;&#41;)

[//]: # (* [`:help nvim_get_hl_by_id&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_get_hl_by_id&#40;&#41;&#41;)

[//]: # (* [`:help nvim_get_hl_by_name&#40;&#41;`]&#40;https://neovim.io/doc/user/api.html#nvim_get_hl_by_name&#40;&#41;&#41;)

[//]: # ()
[//]: # (## General tips and recommendations)

[//]: # ()
[//]: # (### 重新加载缓存的模块)

[//]: # ()
[//]: # (在 Lua 中，`require&#40;&#41;` 函数会缓存已加载的模块，这对提升性能是非常有用的。但是它会对插件的工作造成影响，因为已加载的模块不会在后续的 `require&#40;&#41;` 调用中更新。)

[//]: # ()
[//]: # (如果你想刷新某个特定模块的缓存，那么你必须去修改全局的 `package.loaded` :)

[//]: # ()
[//]: # (```lua)

[//]: # (package.loaded['modname'] = nil)

[//]: # (require&#40;'modname'&#41; -- loads an updated version of module 'modname')

[//]: # (```)

[//]: # ()
[//]: # ([nvim-lua/plenary.nvim]&#40;https://github.com/nvim-lua/plenary.nvim&#41; 插件提供了实现此功能的[自定义函数]&#40;https://github.com/nvim-lua/plenary.nvim/blob/master/lua/plenary/reload.lua&#41;)

[//]: # ()
[//]: # (### 不要填充 Lua 字符串！)

[//]: # ()
[//]: # (当使用双重中括号的字符串时，尽量不要填充多余的字符（如空格、制表符等）！当字符没有特殊意义的时候这样做无可非议；但是当字符具有特殊意义时，这样做可能会导致一些难以发现的问题)

[//]: # ()
[//]: # (```lua)

[//]: # (vim.api.nvim_set_keymap&#40;'n', '<Leader>f', [[ <Cmd>call foo&#40;&#41;<CR> ]], {noremap = true}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (在上面的例子中，`<Leader>f` 被映射到了 `<Space><Cmd>call foo&#40;&#41;<CR><Space>` ，而不是我们期望的 `<Cmd>call foo&#40;&#41;<CR>`。)

[//]: # ()
[//]: # (### 关于 Vimscript <-> Lua 之间类型转换的注意事项)

[//]: # ()
[//]: # (#### 变量转换时会创建一个副本)

[//]: # ()
[//]: # (这意味着你对对象（从 Lua 转换到 Vimscript 的对象或者反过来）的引用进行修改不会影响到原对象。)

[//]: # ()
[//]: # (例如，Vimscript 中的 `map&#40;&#41;` 函数就地修改了一个变量：)

[//]: # ()
[//]: # (```vim)

[//]: # (let s:list = [1, 2, 3])

[//]: # (let s:newlist = map&#40;s:list, {_, v -> v * 2}&#41;)

[//]: # ()
[//]: # (echo s:list)

[//]: # (" [2, 4, 6])

[//]: # (echo s:newlist)

[//]: # (" [2, 4, 6])

[//]: # (echo s:list is# s:newlist)

[//]: # (" 1)

[//]: # (```)

[//]: # ()
[//]: # (在 Lua 中调用这个函数，它改变的将会是参数的一个副本：)

[//]: # ()
[//]: # (```lua)

[//]: # (local tbl = {1, 2, 3})

[//]: # (local newtbl = vim.fn.map&#40;tbl, function&#40;_, v&#41; return v * 2 end&#41;)

[//]: # ()
[//]: # (print&#40;vim.inspect&#40;tbl&#41;&#41; -- { 1, 2, 3 })

[//]: # (print&#40;vim.inspect&#40;newtbl&#41;&#41; -- { 2, 4, 6 })

[//]: # (print&#40;tbl == newtbl&#41; -- false)

[//]: # (```)

[//]: # ()
[//]: # (#### 并不是总能进行类型转换)

[//]: # ()
[//]: # (这主要影响函数和 table：)

[//]: # ()
[//]: # (混合列表和字典的 Lua table 是无法转换的：)

[//]: # ()
[//]: # (```lua)

[//]: # (print&#40;vim.fn.count&#40;{1, 1, number = 1}, 1&#41;&#41;)

[//]: # (-- E5100: Cannot convert given lua table: table should either have a sequence of positive integer keys or contain only string keys)

[//]: # (```)

[//]: # ()
[//]: # (尽管你可以在 Lua 中通过 `vim.fn` 调用 VIm 函数，但是你不能保存对它们的引用。这可能会导致一些意外的行为：)

[//]: # ()
[//]: # (```lua)

[//]: # (local FugitiveHead = vim.fn.funcref&#40;'FugitiveHead'&#41;)

[//]: # (print&#40;FugitiveHead&#41; -- vim.NIL)

[//]: # ()
[//]: # (vim.cmd&#40;"let g:test_dict = {'test_lambda': {-> 1}}"&#41;)

[//]: # (print&#40;vim.g.test_dict.test_lambda&#41; -- nil)

[//]: # (print&#40;vim.inspect&#40;vim.g.test_dict&#41;&#41; -- {})

[//]: # (```)

[//]: # ()
[//]: # (把 Lua 函数作为参数传给 Vim 函数是可行的，但是不能把它们存在 Vim 变量中（Neovim 0.7.0+ 中修复了这个问题）：)

[//]: # ()
[//]: # (```lua)

[//]: # (-- This works:)

[//]: # (vim.fn.jobstart&#40;{'ls'}, {)

[//]: # (    on_stdout = function&#40;chan_id, data, name&#41;)

[//]: # (        print&#40;vim.inspect&#40;data&#41;&#41;)

[//]: # (    end)

[//]: # (}&#41;)

[//]: # ()
[//]: # (-- This doesn't:)

[//]: # (vim.g.test_dict = {test_lambda = function&#40;&#41; return 1 end} -- Error: Cannot convert given lua type)

[//]: # (```)

[//]: # ()
[//]: # (值得注意的是，在 Vimscript 中使用 `luaeval&#40;&#41;` 执行相同操作却是可行的：)

[//]: # ()
[//]: # (```vim)

[//]: # (let g:test_dict = {'test_lambda': luaeval&#40;'function&#40;&#41; return 1 end'&#41;})

[//]: # (echo g:test_dict)

[//]: # (" {'test_lambda': function&#40;'<lambda>4714'&#41;})

[//]: # (```)

[//]: # ()
[//]: # (#### Vim booleans)

[//]: # ()
[//]: # (在 Vim 脚本中，一种常见的情况是使用 `1` 或 `0` 来代表布尔值。事实上，直到版本 7.4.1154，Vim 才有单独的布尔类型。)

[//]: # ()
[//]: # (在 Vimscript 中，Lua 的布尔值会被转换为真正的布尔值，而不是数字：)

[//]: # ()
[//]: # (```vim)

[//]: # (lua vim.g.lua_true = true)

[//]: # (echo g:lua_true)

[//]: # (" v:true)

[//]: # (lua vim.g.lua_false = false)

[//]: # (echo g:lua_false)

[//]: # (" v:false)

[//]: # (```)

[//]: # ()
[//]: # (### 设置 linters/language servers)

[//]: # ()
[//]: # (如果你使用 liner 和 / 或 language server 来获得 Lua 项目的自动补全和错误检查，你可能需要为它们配置 Neovim 特定的设置。以下是一些流行工具的推荐配置：)

[//]: # ()
[//]: # (#### luacheck)

[//]: # ()
[//]: # (你可以通过把此配置放入 `~/.luacheckrc` （或 `$XDG_CONFIG_HOME/luacheck/.luacheckrc`）中来让 [luacheck]&#40;https://github.com/mpeterv/luacheck/&#41; 识别全局的 `vim` 变量：)

[//]: # ()
[//]: # (```lua)

[//]: # (globals = {)

[//]: # (    "vim",)

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # ([Alloyed/lua-lsp ]&#40;https://github.com/Alloyed/lua-lsp/&#41; 使用 `luacheck` 提供 linting 并读取相同的文件。)

[//]: # ()
[//]: # (有关如何配置 `luacheck` 的更多信息，请参见它的[文档]&#40;https://luacheck.readthedocs.io/en/stable/config.html&#41;)

[//]: # ()
[//]: # (#### sumneko/lua-language-server)

[//]: # ()
[//]: # ([nvim-lspconfig]&#40;https://github.com/neovim/nvim-lspconfig/&#41; 仓库中包含了如何配置 sumneko/lua-language-server 的[说明]&#40;https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#sumneko_lua&#41;（示例使用内置 LSP 客户端，但其他 LSP 客户端实现的配置应该相同）。)

[//]: # ()
[//]: # (有关如何配置 [sumneko/lua-language-server]&#40;https://github.com/sumneko/lua-language-server/&#41; 的更多信息，请参阅 ["Setting"]&#40;https://github.com/sumneko/lua-language-server/wiki/Setting&#41;)

[//]: # ()
[//]: # (#### coc.nvim)

[//]: # ()
[//]: # ([coc.nvim]&#40;https://github.com/neoclide/coc.nvim/&#41; 的 [rafcamlet/coc-nvim-lua]&#40;https://github.com/rafcamlet/coc-nvim-lua/&#41; 补全源为 Neovim 标准库提供了补全项。)

[//]: # ()
[//]: # (### 调试 Lua 代码)

[//]: # ()
[//]: # (你可以使用 [jbyuki/one-small-step-for-vimkind]&#40;https://github.com/jbyuki/one-small-step-for-vimkind&#41; 调试在单独的 Neovim 实例中运行的 Lua 代码)

[//]: # ()
[//]: # (该插件使用 [Debug Adapter Protocol]&#40;https://microsoft.github.io/debug-adapter-protocol/&#41;。连接到一个 debug adapter 需要 DAP 客户端，例如 [mfussenegger/nvim-dap]&#40;https://github.com/mfussenegger/nvim-dap/&#41; 或 [puremourning/vimspector]&#40;https://github.com/puremourning/vimspector/&#41;。)

[//]: # ()
[//]: # (### 调试 Lua 设置的按键映射 / 用户命令 / 自动命令)

[//]: # ()
[//]: # (`:verbose` 命令允许你查看 按键映射 / 用户命令 / 自动命令的定义位置：)

[//]: # ()
[//]: # (```vim)

[//]: # (:verbose map m)

[//]: # (```)

[//]: # ()
[//]: # (```vim)

[//]: # (n  m_          * <Cmd>echo 'example'<CR>)

[//]: # (        Last set from ~/.config/nvim/init.vim line 26)

[//]: # (```)

[//]: # ()
[//]: # (默认情况下，出于性能原因，此功能在 Lua 中是禁用的。你可以通过使用大于 0 的 verbose level 启动 Neovim 来启用它：)

[//]: # ()
[//]: # (```shell)

[//]: # (nvim -V1)

[//]: # (```)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (* [`:help 'verbose'`]&#40;https://neovim.io/doc/user/options.html#'verbose'&#41;)

[//]: # (* [`:help -V`]&#40;https://neovim.io/doc/user/starting.html#-V&#41;)

[//]: # (* [neovim/neovim#15079]&#40;https://github.com/neovim/neovim/pull/15079&#41;)

[//]: # ()
[//]: # (### 测试 Lua 代码)

[//]: # ()
[//]: # (* [plenary.nvim: test harness]&#40;https://github.com/nvim-lua/plenary.nvim/#plenarytest_harness&#41;)

[//]: # (* [notomo/vusted]&#40;https://github.com/notomo/vusted&#41;)

[//]: # ()
[//]: # (### 使用 Luarocks 包)

[//]: # ()
[//]: # ([wbthomason/packer.nvim]&#40;https://github.com/wbthomason/packer.nvim&#41; 支持 Luarocks 包。它的 [README]&#40;https://github.com/wbthomason/packer.nvim/#luarocks-support&#41; 中提供了有关如何设置的说明)

[//]: # ()
[//]: # (## Miscellaneous)

[//]: # ()
[//]: # (### vim.loop)

[//]: # ()
[//]: # (`vim.loop` 是暴露 LibUV 接口的模块。一些相关资源：)

[//]: # ()
[//]: # (- [Official documentation for LibUV]&#40;https://docs.libuv.org/en/v1.x/&#41;)

[//]: # (- [Luv documentation]&#40;https://github.com/luvit/luv/blob/master/docs.md&#41;)

[//]: # (- [teukka.tech - Using LibUV in Neovim]&#40;https://teukka.tech/vimloop.html&#41;)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # (- [`:help vim.loop`]&#40;https://neovim.io/doc/user/lua.html#vim.loop&#41;)

[//]: # ()
[//]: # (### vim.lsp)

[//]: # ()
[//]: # (`vim.lsp` 是内置的 lsp 库。官方的 lsp 配置插件 [neovim/nvim-lspconfig]&#40;https://github.com/neovim/nvim-lspconfig/&#41; 包含一些流行的 language server 的默认配置。)

[//]: # ()
[//]: # (客户端的行为可以使用 "sp-handlers" 进行配置。更多信息请参见：)

[//]: # ()
[//]: # (- [`:help lsp-handler`]&#40;https://neovim.io/doc/user/lsp.html#lsp-handler&#41;)

[//]: # (- [neovim/neovim#12655]&#40;https://github.com/neovim/neovim/pull/12655&#41;)

[//]: # (- [how to migrate from diagnostic-nvim]&#40;https://github.com/nvim-lua/diagnostic-nvim/issues/73#issue-737897078&#41;)

[//]: # ()
[//]: # (你可能还想了解一下一些基于 LSP 客户端构建的[插件]&#40;https://github.com/rockerBOO/awesome-neovim#lsp&#41;)

[//]: # ()
[//]: # (更多信息请参见：)

[//]: # ()
[//]: # (- [`:help lsp`]&#40;https://neovim.io/doc/user/lsp.html#LSP&#41;)

[//]: # ()
[//]: # (### vim.treesitter)

[//]: # ()
[//]: # (`vim.treesitter` 是 [Tree-sitter]&#40;https://tree-sitter.github.io/tree-sitter/&#41; 的 neovim 集成，如果你想了解更多可以参考 [presentation &#40;38:37&#41;]&#40;https://www.youtube.com/watch?v=Jes3bD6P0To&#41;.)

[//]: # ()
[//]: # (The [nvim-treesitter]&#40;https://github.com/nvim-treesitter/&#41; organisation hosts various plugins taking advantage of the library.)

[//]: # ()
[//]: # (> [glepnir]&#40;https://github.com/glepnir&#41; 制作的主题 [zephyr-nvim]&#40;https://github.com/glepnir/zephyr-nvim&#41; 语法高亮基于 nvim-treesitter)

[//]: # (>)

[//]: # (> —— 译者注)

[//]: # ()
[//]: # (See also:)

[//]: # (- [`:help lua-treesitter`]&#40;https://neovim.io/doc/user/treesitter.html#lua-treesitter&#41;)

[//]: # ()
[//]: # (### Transpilers)

[//]: # ()
[//]: # (使用 Lua 的一个优点是您实际上不必编写 Lua 代码！有许多其他语言可以转译到 Lua。)

[//]: # ()
[//]: # (- [Moonscript]&#40;https://moonscript.org/&#41;)

[//]: # ()
[//]: # (可能是 Lua 最著名的转译器之一。添加了许多方便的功能，如类、列表推导或函数字面量。  [svermeulen/nvim-moonmaker]&#40;https://github.com/svermeulen/nvim-moonmaker&#41; 插件允许您直接在 Moonscript 中编写 Neovim 插件和配置。)

[//]: # ()
[//]: # (- [Fennel]&#40;https://fennel-lang.org/&#41;)

[//]: # ()
[//]: # (可以编译为 Lua 的 lisp 方言。你可以使用 [Olical/aniseed]&#40;https://github.com/Olical/aniseed&#41; 或 [Hotpot]&#40;https://github.com/rktjmp/hotpot.nvim&#41; 插件在 Fennel 中为 Neovim  编写配置和插件。此外，[Olical/conjure]&#40;https://github.com/Olical/conjure&#41; 插件提供了一个支持 Fennel（以及其他语言）的交互式开发环境。)

[//]: # ()
[//]: # (* [Teal]&#40;https://github.com/teal-language/tl&#41;)

[//]: # ()
[//]: # (Teal 这个名字来自 TL（typed lua）的发音。这代表了它的目标——向 lua 添加强类型，同时语法尽量保持接近标准 lua 语法。  [nvim-teal-maker]&#40;https://github.com/svermeulen/nvim-teal-maker&#41; 插件可用于直接在 Teal 中编写 Neovim 插件或配置文件)

[//]: # ()
[//]: # (其他一些有趣的项目：)

[//]: # ()
[//]: # (- [TypeScriptToLua/TypeScriptToLua]&#40;https://github.com/TypeScriptToLua/TypeScriptToLua&#41;)

[//]: # (- [teal-language/tl]&#40;https://github.com/teal-language/tl&#41;)

[//]: # (- [Haxe]&#40;https://haxe.org/&#41;)

[//]: # (- [SwadicalRag/wasm2lua]&#40;https://github.com/SwadicalRag/wasm2lua&#41;)

[//]: # (- [hengestone/lua-languages]&#40;https://github.com/hengestone/lua-languages&#41;)