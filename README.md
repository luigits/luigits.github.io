# README
> [!note] 
> 虽然仓库是luigits.github.io，但实际使用的是Note Library的文件。
> 也没什么高明的用法，只是给Note Library增加了一个远程链接而已。

NoteLibrary 是一个使用 Writerside 编写的文档库，它集中了我所有的学习记录，虽然没什么内容，但它会慢慢变多的。

目前的 NoteLibrary 包含以下内容：

- Learn-Writerside: 这是一个关于 Writerside 的文档，它介绍了我从官方手册中学到的 Writerside 使用方法。
- Python: Python 学习记录，但目前只是占位，还未进行迁移到 Writerside 中。
- CLI: 命令行程序软件使用记录，目前只记录了 Scoop 相关的一点内容，还未进行迁移到 Writerside 中。
- old: 旧的文档，long long ago 的文档，意外发现了。将其迁移到 Writerside 中，之后一些可能会过时的文档我也会转移过去。

## 组群编译

Writerside 支持使用 build groups 来编译多个实例到一个输出目录。

Writerside 可以搭配 Github Actions 来自动编译文档。但我还不知道如何做，先手动编译提交到 Github Pages 的仓库吧。

~~运气不错，在复制的仓库中实现了。倘若将其在主仓库也实现，我将会修改 README~~

目前已经实现将仓库push到GitHub时自动触发Action进行部署。

先了解两个概念：用户页面和项目页面

- 用户页面：归属于用户级别的页面，最直接的表现形式为必须使用 `<user_name>.github.io` 仓库生成静态网站
- 项目页面：归属所在项目，页面访问形式为 `<user_name>.github.io/<project_name>` ，区分大小写

最近一段时间我会更新相关内容。优先将使用 Writerside 通过 Github Actions 部署的内容更新完毕。
