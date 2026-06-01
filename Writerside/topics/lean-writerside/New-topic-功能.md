# New-topic 功能

New topic 主要的作用是创建两种格式的 topic 文档：markdown 和 XML

![new-topic](new-topic.png)

从上到下分别是：

- 创建空白 MD 主题
- 创建空白 XML 主题
- 创建空白组
- API 参考
- 从模板创建主题
- 连接主题文件到 TOC (不知道 TOC 啥意思)
- 添加已有 Markdown 文件

常用的就头三个，后面的基本上不咋用 (大概)。

> **从模板创建主题**
>
> 如果要使用什么花哨的样式，可以使用“Topic from Template...”，创建一个样式丰富的文档来参考
>
{style="note"}

## 从本地导入需注意 {collapsible="true"}

在标准 markdown 格式下，H1 是必须存在且作为第一个标题的。

但在使用中会因为个人习惯等原因从 H2 甚至 H3 开始，这会影响 Writerside。

最好的解决方案是使用 Writerside 创建 topic，从原文档复制内容过来

当然也可以在文件目录中将缺失的 H1 补上，这也是能被 Writerside 正常识别的


<tabs>
<tab title="Markdown">
<code-block lang="markdown">
# a

Start typing here...
</code-block>
</tab>
<tab title="XML">
<code-block lang="XML">
<![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE topic
    SYSTEM "https://resources.jetbrains.com/writerside/1.0/xhtml-entities.dtd">
<topic xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="https://resources.jetbrains.com/writerside/1.0/topic.v2.xsd"
    title="a" id="a">
    <p>Start typing here...</p>
</topic>
]]>
</code-block>
</tab>
</tabs>

## Markdown VS XML

两种编写方式除了 WriterSide 自行开发的 markup 没 有对 markdown 特化实现外，markdown 的写法会更简洁一些，尽量选择 markdown
写法，虽然官方主推 XML。

> 文档报错
>
> 在编写时会因为各种原因提示有错误，盯一下 *Preview* 是否显示正确，若正确就可以忽略
