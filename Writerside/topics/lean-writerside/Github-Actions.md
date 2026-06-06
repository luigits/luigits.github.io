<show-structure depth="3"/>

# Github Actions

本页面以当前正在使用的 workflow
为例，所有解释均为个人理解，以[GitHub 官方文档](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions)
为准。

```yaml 
name: build no search # 没什么好说的，就是当前工作流的名字
on: # 工作流的触发条件
  push: # 当推送到远程仓库时触发
    branches: [ 'master' ] # 必须指明推送到的是那个分支
  workflow_dispatch: # 允许手动触发在 Github 项目的 Action 中可以手动运行
  schedule: # 计划，属于和 cron 绑定的关键字
    - cron: '0 0 * * 1' # 分 时 天 月 周，当前设定为 UTC 时间每周 1 的凌晨 0 点触发工作流

permissions: # 工作流的权限，属于全局设置的权限
  contents: read # 读取仓库代码的权限
  id-token: write # 如果要部署网站，建议带上，设置为 none，此处报错了可以再改
  pages: write # 允许 GITHUB_TOKEN 对 Github Pages 的写入 (部署) 权限

concurrency: # 并发控制，确保只允许一个部署任务在运行，且取消正在运行的任务
  group: ${{ github.workflow }}-${{ github.ref }} # 每个 workflow 实例和每个 ref 实例的并发控制 
  cancel-in-progress: true # 取消正在运行的任务，确保只运行最新版本的任务

env: # 工作流的全局环境变量
  INSTANCE: 'Writerside/luigits-docs'# 实例名，若启用了 IS_GROUP，必须将此处改为群组实例名
  DOCKER_VERSION: 'latest' # docker 的版本号，可以指定某个版本，会更加稳定些
  IS_GROUP: 'true' # 是否启用群组编译

# 这里才是重头戏
jobs:
  build: # 一个 job，名称可以随便起。只要不冲突就行
    runs-on: ubuntu-24.04 # 运行在何种环境中
    outputs: # 输出的变量
      # artifact 所表示的变量是编译后的 Writerside 压缩包
      artifact: ${{ steps.define-ids.outputs.artifact }}
    steps: # 步骤，每个步骤都单独列出
      - name: Checkout repository # 名称随意，运行的时候你能知道这是干啥就行
        uses: actions/checkout@v6 # 使用的 Action 组件，此 Action 用于克隆代码到环境中
      
      - name: Define instance id and artifacts
        id: define-ids # 方便再次使用而设置的 id
        run: |
          # run 归属的部分是一个整体，不能和 uses 同时使用。
          # 下面的内容不用动，JetBrains 已经设置好了
          # 这里我删除了有关 Algolia 的部分，因此与 Action 库中的示例有所出入
          INSTANCE=${INSTANCE#*/}
          INSTANCE_ID_UPPER=$(echo "$INSTANCE" | tr '[:lower:]' '[:upper:]')
          ARTIFACT="webHelp${INSTANCE_ID_UPPER}2-all.zip"
          # Print the values
          echo "INSTANCE_ID_UPPER: $INSTANCE_ID_UPPER"
          echo "ARTIFACT: $ARTIFACT"
          # Set the environment variables and outputs
          echo "INSTANCE_ID_UPPER=$INSTANCE_ID_UPPER" >> $GITHUB_ENV
          echo "ARTIFACT=$ARTIFACT" >> $GITHUB_ENV
          echo "artifact=$ARTIFACT" >> $GITHUB_OUTPUT
      
      - name: Build docs using Writerside Docker builder
        uses: JetBrains/writerside-github-action@v4 # 核心：处理不好这里会报 Docker 找不到文件的错误
        with: # 给该 Action 的参数
          instance: ${{env.INSTANCE}} # 实例名，来自 env 的设置
          docker-version: ${{ env.DOCKER_VERSION }} # docker 版本号
          is-group: ${{ env.IS_GROUP }} # 是否启用群组编译
          artifact: ${{ steps.define-ids.outputs.artifact }} # 编译后的压缩包名
      
      - name: Save artifact with build results
        uses: actions/upload-artifact@v7 # 上传上个 Action 生成的工件，用于多个环境直接的数据交换
        with:
          name: docs # 目录，可以理解为将编译后的压缩包上传到了一个名为 docs 的共享文件夹中
          # 这里的 path 很恶心，不要按照自己的理解添加注释，这是一个整体，添加了就会找不到文件
          path: |
            artifacts/${{ steps.define-ids.outputs.artifact }}
            artifacts/report.json
  deploy: # 另一个 job，上一个 job 运行完其环境会立即销毁
    environment: # 设置 job 级别的变量，作用范围是当前 job
      name: github-pages # 
      url: ${{ steps.deployment.outputs.page_url }} # 该 url 用于返回部署后的网站链接
    needs: build # 多个 job 之间默认并行运行，使用 needs 关键字使其有运行的先后顺序，此处设置需要 build 执行完成 deploy 才会运行
    runs-on: ubuntu-24.04
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v8 # 下载上一个 job 上传的工件
        with:
          name: docs # 与上传名字要相同，毕竟人家上传到了这个位置，我就要从这里下载
          path: artifacts # 下载到的目录
      
      - name: Unzip artifact
        # 解压下载的文件到 dir
        run: unzip -O UTF-8 -qq "artifacts/${{ needs.build.outputs.artifact }}" -d dir
      
      - name: Setup Pages
        uses: actions/configure-pages@v6 # 配置 Github Pages，准备部署
      
      - name: Upload GitHub Pages artifact
        uses: actions/upload-pages-artifact@v5 # 上传到 Pages
        with:
          path: dir # 从 dir 上传
      
      - name: Deploy to GitHub Pages
        id: deployment # 设置 id，方便输出 url
        uses: actions/deploy-pages@v5 # 部署 Github Pages
```

{collapsible="true" collapsed-title="workflow 示例"}

<deflist>
<def title="contents:read">
如果工作流上传下载工件发生错误，例如找不到文件，但实际存在。

可以通过将 contents 值设为 write 来自行设置命令

换句话说就是当值为 write 时，拥有对文件的读写权限。
</def>
<def title="IS_GROUP:'true'">
开启后允许群组编译，需要在 <ui-path><a href="配置-build-groups.md">build-groups.xml</a></ui-path> 中设置。

其 INSTANCE 也需要设置为 `<build-group>` 的 name 参数，也就是群组实例名
</def>

<def title="algolia">
algolia 是 Writerside 官方手册中提供的搜索服务提供商，也可以自行设置。

其设置一直无法搜索，因此直接将设置的内容删除，xml 相关部分注释。直到解决此问题
</def>
</deflist>

> 官方文档中的 workflow 与 Github Action 中的 workflow 需要结合起来，相互印证
>
> 反正单独看我是迷迷糊糊的，还是直接 diff 对比才逐渐了解

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
runs-on: [ self-hosted, linux ]
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
| `env`  | 设置环境变量          | job 的环境变量作用域仅在当前 job                  |
| `if`   | 条件执行            | `if: github.ref == 'refs/heads/main'` |

### Checkout 详解

```yaml
uses: actions/checkout@<commit-sha>  # 推荐用 SHA 而非 tag，更安全
```

- 作用：将仓库代码克隆到 runner
- **强烈建议使用完整 commit SHA**（如 `@de0fac2e...`）而非 `@v6`，防止 tag
  被篡改

> 完整 commit SHA 获取
>
> 打开 Action 仓库，点击 tags，点击需要的版本 commit 记录
>
> 此时地址栏最后部分显示的就是需要的 SHA
>
> <img src="commit-SHA.png" width="200" thumbnail="true" alt="commit-SHA"/>

### `uses` 的三种引用方式

```yaml
# 1. 官方 Action（推荐用 SHA）
uses: actions/checkout@<sha>

# 2. 同组织/用户的 Action
uses: actions/checkout@main  # ⚠️ 用分支名有安全风险

# 3. 本地复用 workflow（.github/workflows/ 下）
uses: ./.github/workflows/reusable.yml
```

|   写法   | 示例                                                                |         特点         |      推荐度      |
|:------:|:------------------------------------------------------------------|:------------------:|:-------------:|
|   标签   | actions/checkout@v4                                               | 可读性好，但标签可能被覆盖或指向变更 | ⚠️ 仅限测试/非关键流程 |
|   分支   | actions/checkout@main                                             |  始终拉取最新代码，极易破坏流水线  |   ❌ 生产环境禁用    |
| 完整 SHA | actions/checkout@a5ac<br>7e51b41094c92402da3<br>b24376905380afc29 |    不可变、防篡改、可追溯     |   ✅ 官方强烈推荐    |
{style='both'}

### `env` 环境变量作用域

```yaml
# 全局变量
env:
  GLOBAL_VAR: value

jobs:
  job1:
    # job 级变量
    env:
      JOB_VAR: value
    steps:
      # steps 级变量
        env:
          STEP_VAR: value
```

在这些变量中，控制程度越精细，优先级越高

### 其他常用 secrets

```yaml
env:
  API_KEY: ${{ secrets.MY_API_KEY }}  # 需在 Repo Settings → Secrets 中配置
```
和 Linux 的用户密码一样，想要修改时无法看到之前设定的值


---

## 附：完整语法结构速查表

```yaml
name: Optional Workflow Name          # 显示在 Actions 标签页

on: # 触发条件（必需）
  push:
    branches: [ main ]
  workflow_dispatch:                  # 手动触发

permissions: # 权限控制（推荐显式声明）
  contents: read

env: # 全局环境变量
  NODE_ENV: production

jobs:
  job_id: # job 唯一 ID
    name: Human-readable Name         # 可选，美化显示
    runs-on: ubuntu-latest            # 必需，运行环境
    timeout-minutes: 30               # 可选，超时自动取消
    permissions: # job 级权限（覆盖全局）
      contents: write
    
    strategy: # 矩阵策略（并行测试多环境）
      matrix:
        os: [ ubuntu, windows ]
        node: [ 16, 18, 20 ]
    
    steps: # 执行步骤（按顺序）
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

> 💡 **调试技巧**
> 
> 创建 secrets，开启 debug 模式：
> 1. ACTIONS_RUNNER_DEBUG：开启运行器详细日志 
> 2. ACTIONS_STEP_DEBUG：开启每个 Step 的详细调试日志
> 
>> 注意：在创建 secrets 时这是两个不同的 secret
>
> 在 GitHub 仓库 → Actions → 选择 workflow → 点击 "Run workflow" 手动触发，实时查看日志排查问题。
> 