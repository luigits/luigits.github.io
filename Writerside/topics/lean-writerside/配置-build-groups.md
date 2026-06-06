# 配置 build groups

想要配置 `build groups` 就需要在 <ui-path>cfg/</ui-path> 中创建 <ui-path>build-groups.xml</ui-path>

## 配置 build-groups.xml

<tldr>

<format color="Tomato">先决条件</format>

- 至少两个实例 (instance)
- 创建了 <ui-path>cfg/build-groups.xml</ui-path>

</tldr>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE build-groups SYSTEM "https://resources.jetbrains.com/writerside/1.0/build-groups.dtd">
<build-groups xsi:noNamespaceSchemaLocation="https://resources.jetbrains.com/writerside/1.0/build-groups.xsd"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <build-group name="all-docs">
        <instance instance-id="lw" alias="学习Writerside" path="/learn-writerside"/>
        <instance instance-id="py" alias="python" path="/python"/>
    </build-group>
</build-groups>
```

<tabs>
<tab title="build-group">

<deflist>
<def title="name">群组编译时的实例名称，由群组名称完成实例编译</def>
</deflist>
</tab>
<tab title="instance">

<deflist>
<def title="instance-id">实例 id，用于将多个实例绑定到群组</def>
<def title="alias">别名，支持中文。切换实例时的名称</def>
<def title="path">在 web 中显示的实例所属路径</def>
</deflist>
</tab>
</tabs>

## Github Actions

编译好了文档自然要将其托管，GitHub 提供了 Github Pages 部署，这提供了两种方案：

- 静态页面：需手动将 HTML 文件 push 到远程仓库
- Github Actions：将文档 push 到远程仓库，Github Actions 自动化完成 build 并将其部署

> 也就是说在 GitHub Pages 中选择 构建和部署来源是 Github Actions 后就不需要管后续编译，只需要将文档 push 到 GitHub 即可。
>

### 编写 workflow

实现最方便的自然是 Github Actions，所以自然要配置 GitHub Actions。

> 难易度
> 
> 选择从分支部署只需要将 Writerside 编译后的页面 push 到正确的分支即可，没什么额外的难度。
> 
> 选择 Github Actions 需要配置 workflow 文件，这其实是一项有难度的任务。
>
{style="note"}

具体请查看[](Github-Actions.md)