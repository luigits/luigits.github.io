<show-structure depth="3"/>

# Github Actions

结合 [GitHub 官方文档](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions)，以下面的内容为 workflow 示例：

<code-block lang="yaml" collapsible="true" collapsed-title="workflow示例">
on:
  workflow_dispatch:      # 支持 GitHub Actions 页面手动触发
  schedule:
    - cron: '0 0 * * *'  # 每周天 UTC 00:00 执行（北京时间 8 点）

permissions:
  contents: write

jobs:
  excavate:
    name: Excavate
    runs-on: windows-latest
    steps:
    - name: Checkout
      uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
    - name: Excavate
      uses: ScoopInstaller/GithubActions@main
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        SKIP_UPDATED: '1'
        DEFAULT_BRANCH: 'develop'
</code-block>

## `on:` - 触发条件

```yaml
on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * *'
```

<deflist>
<def title="workflow_dispatch">

**作用**：允许在 GitHub 网页上**手动触发** workflow

![run workflow](run-workflow.png)

**使用场景**：调试、临时执行、需要人工干预的任务
</def>

<def title="schedule + cron">

**作用**：按定时任务自动触发

**Cron 语法**（5 个字段）：
  ```
  ┌───────────── 分钟 (0-59)
  │ ┌───────────── 小时 (0-23)
  │ │ ┌───────────── 日期 (1-31)
  │ │ │ ┌───────────── 月份 (1-12)
  │ │ │ │ ┌───────────── 星期 (0-6, 0=周日)
  │ │ │ │ │
  * * * * *
  ```

示例

| 表达式            | 含义               |
|----------------|------------------|
| `0 0 * * *`    | 每天 00:00 UTC 执行  |
| `0 3 * * 1`    | 每周一 03:00 UTC 执行 |
| `*/15 * * * *` | 每 15 分钟执行一次      |
</def>
</deflist>

## `permissions:` - 权限控制

```yaml
permissions:
  contents: write
```

<deflist>
<def title="作用">

控制 `GITHUB_TOKEN` 的默认权限范围（最小权限原则）

可设置为 `read` / `write` / `none`
</def>
</deflist>

### 常用权限示例
```yaml
permissions:
  contents: read          # 读取仓库内容（如克隆代码）
  pull-requests: write    # 创建/评论 PR
  issues: write           # 创建/评论 Issue
  packages: write         # 发布 GitHub Packages
  id-token: write         # OIDC 认证（用于云厂商部署）
```

---

## `jobs:` - 任务定义

```yaml
jobs:
  excavate:
    name: Excavate
    runs-on: windows-latest
```

### 关键字段说明

| 字段         | 作用                 | 示例                                                             |
|------------|--------------------|----------------------------------------------------------------|
| `excavate` | job 的唯一 ID（用于引用）   | 任意合法标识符                                                        |
| `name`     | 在 GitHub UI 中显示的名称 | `"Build & Deploy"`                                             |
| `runs-on`  | 运行环境（Runner）       | `ubuntu-latest`, `windows-latest`, `macos-latest`, 或自托管 runner |

### 常见 `runs-on` 值
```yaml
# 最新 Ubuntu（推荐，最快）
runs-on: ubuntu-latest

# 指定 Windows 版本
runs-on: windows-2022

# 自托管 runner + 标签过滤
runs-on: [self-hosted, linux]
```

## `steps:` - 执行步骤

```yaml
steps:
- name: Checkout
  uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
```

### Step 核心字段

| 字段     | 说明              | 示例                                    |
|--------|-----------------|---------------------------------------|
| `name` | 步骤名称（日志中显示）     | `"Install dependencies"`              |
| `uses` | 调用官方/第三方 Action | `actions/setup-node@v4`               |
| `run`  | 直接执行 shell 命令   | `npm install`                         |
| `with` | 向 Action 传递参数   | `with: { node-version: '18' }`        |
| `env`  | 设置环境变量          | 见下文                                   |
| `if`   | 条件执行            | `if: github.ref == 'refs/heads/main'` |

### Checkout 详解
```yaml
uses: actions/checkout@<commit-sha>  # 推荐用 SHA 而非 tag，更安全
```
- 作用：将仓库代码克隆到 runner
- **强烈建议使用完整 commit SHA**（如 `@de0fac2e...`）而非 `@v6`，防止 tag 被篡改（[安全最佳实践](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions#using-third-party-actions)）

## 调用自定义 Action + 环境变量

```yaml
- name: Excavate
  uses: ScoopInstaller/GithubActions@main
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    SKIP_UPDATED: '1'
    DEFAULT_BRANCH: 'develop'
```

### `uses` 的三种引用方式
```yaml
# 1. 官方 Action（推荐用 SHA）
uses: actions/checkout@<sha>

# 2. 同组织/用户的 Action
uses: ScoopInstaller/GithubActions@main   # ⚠️ 用分支名有安全风险

# 3. 本地复用 workflow（.github/workflows/ 下）
uses: ./.github/workflows/reusable.yml
```

### `env` 环境变量作用域
```yaml
# ① Workflow 级别（所有 job 可用）
env:
  GLOBAL_VAR: value

jobs:
  job1:
    # ② Job 级别（覆盖 workflow 级）
    env:
      JOB_VAR: value
    steps:
      - # ③ Step 级别（优先级最高）
        env:
          STEP_VAR: value
```

### `${{ secrets.GITHUB_TOKEN }}` 是什么？
- GitHub 自动生成的**短期凭证**，权限由 `permissions` 控制
- 有效期：单次 workflow 运行期间（最长 24 小时）
- **不要硬编码 token**，始终通过 `secrets` 上下文引用

### 其他常用 secrets
```yaml
env:
  API_KEY: ${{ secrets.MY_API_KEY }}  # 需在 Repo Settings → Secrets 中配置
```

---

## 附：完整语法结构速查表

```yaml
name: Optional Workflow Name          # 显示在 Actions 标签页

on:                                   # 触发条件（必需）
  push:
    branches: [ main ]
  workflow_dispatch:                  # 手动触发

permissions:                          # 权限控制（推荐显式声明）
  contents: read

env:                                  # 全局环境变量
  NODE_ENV: production

jobs:
  job_id:                             # job 唯一 ID
    name: Human-readable Name         # 可选，美化显示
    runs-on: ubuntu-latest            # 必需，运行环境
    timeout-minutes: 30               # 可选，超时自动取消
    permissions:                      # job 级权限（覆盖全局）
      contents: write
    
    strategy:                         # 矩阵策略（并行测试多环境）
      matrix:
        os: [ubuntu, windows]
        node: [16, 18, 20]
    
    steps:                            # 执行步骤（按顺序）
      - name: Step Name
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
        env:
          STEP_ENV: value
        if: success()                 # 条件执行
        continue-on-error: false      # 出错是否继续
        timeout-minutes: 10           # 步骤级超时
```

---

## 最佳实践建议

1. ✅ **固定 Action 版本**：用 commit SHA 替代 `@main`/`@v1`
2. ✅ **最小权限原则**：`permissions` 只开必要权限
3. ✅ **敏感信息用 secrets**：绝不硬编码 token/password
4. ✅ **添加超时保护**：`timeout-minutes` 防止卡死
5. ✅ **使用条件判断**：`if` 控制步骤执行逻辑
6. ✅ **注释关键配置**：特别是 cron 表达式、权限变更处

---

> 💡 **调试技巧**：在 GitHub 仓库 → Actions → 选择 workflow → 点击 "Run workflow" 手动触发，实时查看日志排查问题。
