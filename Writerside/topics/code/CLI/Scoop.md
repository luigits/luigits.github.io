<show-structure depth="3"/>

# Scoop

<primary-label ref="cli-primary-scoop"/>
<secondary-label ref="cli-secondary-scoop"/>
<secondary-label ref="cli-secondary-bucket"/>

## 安装 Scoop {id="scoop-install-scoop"}

这是我自己写的脚本，可能会遇到各种问题，欢迎邮件告诉我:2833171898@qq.com

## 使用 scoop {id="scoop-using-scoop"}

```PowerShell
# 搜索软件名
scoop search <APP_NAME>
# 查看软件的详细信息
scoop info <APP_NAME>
# 下载软件
scoop install <APP_NAME>
```

## 自建 bucket {id="scoop-create-bucket"}

相信各位在浏览这部分内容的时候已经知道 scoop 社区提供了一个[模板](https://github.com/ScoopInstaller/BucketTemplate)用于自建
bucket。

<format color="Red">但我要说的是，不要使用它的模板。</format>

该模板已经 2 年没更新了，但不同的是，官方维护的 main 库一直与时俱进。是的，直接克隆
<ui-path>[main](https://github.com/ScoopInstaller/Main)</ui-path>
效果会更好。

<procedure>

1. [克隆 <ui-path>main</ui-path> 库](#scoop-text-clone_main)
2. [删除 <ui-path>.git</ui-path> 文件夹](#scoop-text-remove_git)
3. [删除 <ui-path>bucket</ui-path> 文件夹下不需要的软件](#scoop-text-del_manifest)
4. [修改库名称，重新初始化当前 bucket](#scoop-text-git_init)
5. [上传到远程仓库](#scoop-text-push_repo)
6. [添加到 scoop bucket](#scoop-text-add_bucket)
7. [正常使用](#scoop-text-use_scoop)

</procedure>

### 克隆 main 库 {id="scoop-text-clone_main"}

```bash
gh repo clone ScoopInstaller/Main
```

<format id="scoop-text-remove_git" color="Pink" style="bold">移除 .git</format>

克隆完仓库之后记得删除远程仓库链接，甚至一步到位删除<ui-path>.git</ui-path>文件夹。

```Bash
# 查看远程链接
git remote -v

# 删除远程链接
git remote remove <remote-name>
```

### 删除不需要的清单文件 {id="scoop-text-del_manifest"}

如果你想制作一个完全自用的仓库，就看一下 <ui-path>main/bucket</ui-path> 里面有没有你需要的软件，保留需要的软件，其他的 json
文件全部删掉。

### 重新初始化 git {id="scoop-text-git_init"}

如果直接删除了 <ui-path>.git</ui-path>，那么这一部分就是给你看的。

1. 退到上一级文件夹，将 <ui-path>main</ui-path> 修改为你想要取的名字
2. 进入此文件夹
3. 执行命令

    ```bash
    # 初始化仓库 
    git init
   
    # 将文件加入暂存区
    git add .
   
    # 提交到本地
    git commit -m "initialize repository"
    
    ```

### 上传到远程库 {id="scoop-text-push_repo"}

1. 从<tooltip term="GitHub">GitHub</tooltip>/<tooltip term="Gitee">Gitee</tooltip> 创建一个空白仓库
2. 复制 git 地址
3. 添加链接到本地仓库

   ```bash
    # 添加远程链接
    git remote add origin <remote-url>
    ```

4. 上传

   ```bash
   # 上传仓库
   git push origin <branch_name>
   ```

### 添加到 scoop bucket {id="scoop-text-add_bucket"}

上传完毕后可以直接使用 scoop 将 bucket 添加到本地。

```bash
scoop bucket add <bucket_name> <remote_url>
```

### 正常使用 {id="scoop-text-use_scoop"}

我想不需要我说如何安装软件。

### 自动更新 manifest

在经过一段时间的使用，你发现了一个问题：别人自建的仓库会及时更新 manifest，难道别人这么闲吗？

实际上这是由 <tooltip term="Github Actions">`Github Actions`</tooltip> 自动完成的。`GitHub Actions` 按照配置文件的设置会自动完成一些列任务。

此时直接复制 <ui-path>main</ui-path> 的好处来了：不需要自己手动配置，只需要确保
<ui-path>.github/workflows/Excavator.yml</ui-path> 存在即可。

```yaml
name: Excavator

on:
  workflow_dispatch:      # 支持 GitHub Actions 页面手动触发
  schedule:
    - cron: '0 0 * * *'  # 每周一 北京时间 08:00 执行（可按需调整）

permissions:
  contents: write

jobs:
  excavate:
    name: Excavate
    runs-on: windows-latest
    steps:
      - name: Checkout
        uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd
      - name: Excavate
        uses: ScoopInstaller/GithubActions@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SKIP_UPDATED: '1'
          DEFAULT_BRANCH: 'develop' # 自动化成功后可以将其删除
```

从上面的配置文件中可以看出，除了有注释的地方可能需要更改之外，其他的默认就好。

当 `Excavator` 自动化流程执行成功之时就代表你的仓库会定期检查 <ui-path>bucket/</ui-path> 下的软件是否有更新，若有则由机器人修改文件实现自动更新的效果。

### 编写符合要求的 Manifest {id="scoop-H3-writer_manifest"}

在 scoop 中，所有的软件信息都保存在 <ui-path>bucket/</ui-path> 下的 json 文件中。而在 scoop 中我们叫它清单 (Manifest)。
每个 json 文件表示一个软件，也就是说每个软件都有他自己的清单。而我们要编写的就是符合 `Github Actions` 要求的清单文件。

> 不符合要求 `Github Actions` 无法更新清单文件


以`Github Store`为例，他是托管在 Github 上的，这回为我们减少很多麻烦。

<procedure>

- [复制 GitHub store 最新版本的下载链接](#scoop-H4-copy_url)
- [在 <ui-path>bucket</ui-path> 中创建 <ui-path>github-store.json</ui-path>](#scoop-H4-create_githubstore)
- [编写 Manifest](#scoop-H4-writer_manifest)
- [同步远程库](#scoop-H4-sync_repo)

</procedure>

#### 复制 GitHub store 最新版本的下载链接 {id="scoop-H4-copy_url"}

```http
https://github.com/OpenHub-Store/GitHub-Store/releases/download/v1.9.0/GitHub-Store-1.9.0.exe
```

在 [GitHub store 下载页](https://github-store.org/download/)可以看到 GitHub sotre 提供了多种版本以供选择

- NSIS 安装程序
- MSI 安装程序
- 包管理器

> 使用包管理器方案需要将其仓库保存到本地，其仓库中只有这一个软件清单，并且这不是我们的目的，遂放弃。
>
> NSIS 有一个特点，就是可以直接用 7z 格式直接打开
> 删除掉其中的 `$PLUGINSDIR` 文件夹，剩下的是软件本体，如果遇到这种安装程序不必考虑，使用这种最方便了

#### 在 <ui-path>bucket</ui-path> 中创建 <ui-path>github-store.json</ui-path> {id="scoop-H4-create_githubstore"}

没什么好说的，需要注意一点：Manifest 的名称。

如果文件名是 Github-Store.json，那么使用 scoop 下载的时候软件名就是 Github-Store；  
如果文件名是 github-store.json，那么使用 scoop 下载的时候软件吗就是 github-store。

#### 编写 Manifest {id="scoop-H4-writer_manifest"}

<code-block collapsible="true" lang="json" collapsed-title="Manifest 格式">
{
  "homepage": "",//  官网链接
  "description": "", //  描述
  "license": "", //  协议
  "version": "", //  版本
  "url": "", //  下载链接
  "hash": "", //  哈希值
  "shortcuts": [],  //  快捷方式
  "checkver": {}, // 版本检查
  "autoupdate": {}, // 自动更新
  "post_install": "" //  后安装操作
}
# 注意编写的时候不要将注释保存，这可能导致解析 json 失败
</code-block>

完成的 Manifest 文件如下

```json5
{
  // 找到官网粘贴进去就好了
  "homepage": "https://github-store.org/",
  // 搜一下这是个什么软件就知道了
  "description": "GitHub Store is a free, open-source app store for GitHub releases.",
  // 既然开源了那就能在仓库里看到，否则将用户协议链接贴上去，参考 bilibili 清单文件
  "license": "Apache-2.0",
  // 软件版本，复制的链接里有
  "version": "1.9.0",
  // 复制的链接粘贴到这，因为选择了 NSIS 安装程序，这里后面加上 #dl.7z 就好
  "url": "https://github.com/OpenHub-Store/GitHub-Store/releases/download/v1.9.0/GitHub-Store-1.9.0.exe#/dl.7z",
  // 第一次填写除非 release 提供了 hash 文件，否则需要自己下载下来自己生成，推荐使用 Powershell 的 Get-FileHash
  "hash": "293BF6C7E8BFAE042A61A83900A15D5D0091155C93735F6990729ED5897CF0B5",
  // 将启动程序和要创建什么样的快捷方式加进来就好
  "shortcuts": [
    [
      "github-store.exe",
      "GitHub Store"
    ]
  ],
  // 如果是从 GitHub 获取就直接将仓库地址放上就行，scoop 会自己解析，获取最新版本号
  "checkver": {
    "github": "https://github.com/OpenHub-Store/GitHub-Store"
  },
  "autoupdate": {
    // 使用$version 来表示获取到的版本号，也就是最新版本
    "url": "https://github.com/OpenHub-Store/GitHub-Store/releases/download/v$version/GitHub-Store-$version.exe#/dl.7z"
  },
  // post_install 表示安装结束后执行，在没有这行命令的情况下，scoop 实际上是将压缩包解压到了指定位置，使用该命令可以将 NSIS 的文件夹删除，只保留软件本体
  "post_install": "Remove-Item \"$dir\\`$PLUGINSDIR\" -Force -Recurse -ErrorAction SilentlyContinue"
}
```

如果软件不是从 GitHub 这类代码托管平台获取的，那么就得好好解析版本号规律了，需要将正则写完整放入 checkvar 中。比如
bilibili、qq-nt

#### 同步远程库 {id="scoop-H4-sync_repo"}

```bash
git add .
git commit -m "add github store"
git push origin <branch_name>
```
