# rust 杂记

VS Code + Rust 的 `settings.json` 配置。

### 📄 完整配置（直接复制）

在项目根目录创建 `.vscode/settings.json`，或使用配置文件功能创建 Rust 专门的配置项，设置软件级的 `settings.json` 粘贴以下内容：

```json
{
  // 📐 格式化与保存自动化
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "rust-lang.rust-analyzer",
  "editor.codeActionsOnSave": {
    "source.fixAll.rust-analyzer": "explicit",
    "source.organizeImports.rust-analyzer": "explicit"
  },
  // 🔍 Rust Analyzer 核心诊断增强
  "rust-analyzer.checkOnSave.command": "clippy",
  "rust-analyzer.procMacro.enable": true,
  "rust-analyzer.cargo.loadOutDirsFromCheck": true,
  "rust-analyzer.inlayHints.enable": true,
  "rust-analyzer.inlayHints.chainingHints.enable": true,
  "rust-analyzer.diagnostics.disabled": [],
  "rust-analyzer.hover.actions.references.enable": true,
  // 🐧 性能优化（防卡顿关键）
  "files.watcherExclude": {
    "**/target/**": true,
    "**/.git/objects/**": true,
    "**/.git/subtree-cache/**": true
  },
  "search.exclude": {
    "**/target/**": true
  },
  // 💻 终端与编辑体验
  "terminal.integrated.defaultProfile.linux": "bash",
  "terminal.integrated.profiles.linux": {
    "bash": {
      "path": "bash",
      "args": [
        "-l"
      ]
    }
  },
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,
  "editor.rulers": [
    100
  ],
  "editor.wordWrap": "on"
}
```

### 🔑 核心配置解读（为什么这么设？）

| 配置区块                                       | 作用                      | 对你的实际帮助                                               |
|:-------------------------------------------|:------------------------|:------------------------------------------------------|
| `checkOnSave.command: "clippy"`            | 保存时自动运行 `cargo clippy`  | 替代手动敲命令，实时提示“不地道写法”（如 `unwrap()` 滥用、冗余类型标注）           |
| `inlayHints.enable`                        | 在代码中显示隐含类型/生命周期         | Python 转 Rust 时极大缓解“这变量到底是什么类型？”的焦虑，编译错误前就给你答案        |
| `files.watcherExclude`                     | 排除 `target/` 目录监听       | WSL 文件监听在大型项目易卡死，排除编译输出目录后 IDE 响应速度提升 3~5 倍           |
| `terminal.integrated.defaultProfile.linux` | 默认登录式 shell (`bash -l`) | 自动加载 `~/.bashrc`，确保 `rustup` / `cargo` 环境变量在终端开箱即用    |
| `autoSave: afterDelay (1000ms)`            | 1 秒无操作自动保存              | Rust 编译极快，配合 `formatOnSave` 可养成“写完即规整”的肌肉记忆，避免 Git 冲突 |

### 💡 WSL 专属避坑提醒

| 现象                              | 原因                                | 解决                                                       |
|:--------------------------------|:----------------------------------|:---------------------------------------------------------|
| 终端报错 `command not found: cargo` | `rustup` 环境变量未写入登录 shell          | 在 WSL 执行：`echo 'source "$HOME/.cargo/env"' >> ~/.bashrc` |
| 编译时 `target` 目录权限混乱             | 从 Windows 资源管理器修改过 WSL 文件         | 始终在 WSL 终端内操作 `cargo`，勿跨文件系统直接拖拽                         |
| `rust-analyzer` 持续转圈            | 项目放在 `/mnt/c/...` 或 `target/` 未排除 | 确认代码在 `~/` 下，且 `settings.json` 已生效                       |

### 📈 下一步建议

配置完成后，你的开发环境已达到 **生产级 Rust 入门标准**。建议按以下顺序验证：

1. 终端运行 `cargo new hello_wsl && cd hello_wsl`
2. 在 `main.rs` 输入 `fn main() { let x = 5; println!("{x}"); }`
3. `Ctrl+S` 保存 → 观察底部终端自动执行 `clippy`，右下角弹出 Inlay Hint 类型提示
4. 输入错误语法（如 `let y = x + "str";`）→ 体验秒级红线诊断与快速修复（`Ctrl+.`）