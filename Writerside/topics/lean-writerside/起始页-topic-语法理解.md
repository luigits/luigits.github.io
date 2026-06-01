<show-structure depth="3"/>

# 起始页 Topic 语法理解

本文档仅包含 **section-starting-page** 标记及其子元素

> 在本文档中出现的内容皆为本人理解，可能会出现各种错误
>
>本文档的出现背景仅仅是为了让我更好的上手 topic 文档
>
{style="warning"}

## 固定内容部分 {id="permanent"}

在 XML 语法中，第一行是标记语言的声明

```XML
<?xml version="1.0" encoding="UTF-8"?>
```

第二行就是 topic 的声明，如果有经历过 HTML 文档的编写，那么可以将其理解为 `<HTML>` 标记

```XML

<topic xmlns:xsi="一个schema链接"
   xsi:noNamespaceSchemaLocation="对XML特化的语法定义"
   title="标题名称" id="标题名称" help-id="Section-Starting-Page;WriterSide">
</topic>

```

> **在代码块中我将 id 解释为标题名称的原因**
>
> 一旦修改了创建时的 id 名称，软件会提示没有 H1 标题
>
> 最重要的是没找到在哪里引用了 id，只能改回原来的名称
>
{style="note"}

## 正文部分

> topic 采用的结构和 HTML 类似，因此可以在理解上将其等价

### topic 的身体 {id="t1"}

```XML

<section-starting-page></section-starting-page>
<!--等价于-->
<body></body>
```

一个全新空白~~并非空白~~的 topic 文件格式 如下

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE topic
        SYSTEM "https://resources.jetbrains.com/writerside/1.0/xhtml-entities.dtd">
<topic xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:noNamespaceSchemaLocation="https://resources.jetbrains.com/writerside/1.0/topic.v2.xsd"
       title="Empty XML Topic" id="Empty-XML-Topic">

    <p>Start typing here...</p>
</topic>
```

### topic 的标题 {id="t2"}

在使用过程中可以发现，topic 文件其实就是特化后的 Markdown，topic 将文档的每一部分进行分割，交给不同的标记来处理

```XML

<section-starting-page>
    <title>标题名称</title>
    <description>描述性文字</description>
    <spotlight>
        <card href="#" badge="start" summary="This overrides the card summary of the topic">D</card>
        <card href="#" badge="idea" summary="This overrides the card summary of the topic">E</card>
    </spotlight>
</section-starting-page>
```

完整的 `<section-starting-page>` 标记如下所示

<!--
    强制性子元素包括：
    <title>是节起始页的标题
    <description>是简要介绍该部分的段落
    <spotlight>定义了两个专题
    <primary>定义了本节中需要阅读的更多重要主题
    <secondary>定义了几个您希望在单独的组中突出显示的主题
-->

```XML

<section-starting-page>
    <title>页面标题</title>
    <description>页面说明</description>
    <spotlight>
        <card href="#" badge="start" summary="This overrides the card summary of the topic">D</card>
        <card href="#" badge="idea" summary="This overrides the card summary of the topic">E</card>
    </spotlight>
    <primary>
        <title>主内容链接块</title>
        <a href="#" summary="ABC">链接1</a>
        <a href="#" summary="DEF">链接2</a>
    </primary>
    <secondary>
        <title>次内容链接块</title>
        <a href="#" summary="HIJ">链接3</a>
        <a href="#" summary="MNQ">链接4</a>
    </secondary>
</section-starting-page>
```

> **`<section-starting-page>` 使用说明**
>> 代码块中的标记都是 `<section-starting-page>` 的强制子标记，
>>
>> 也就是说只能存在于 `<section-starting-page>` 中
>- title: <u>title 属于 H1 标题</u>
   >
   >     topic 声明中的标题名称会被`<section-starting-page>`直属子标题 title 强制修改
>
>- description: <u>description 属于 H2 标题</u>
   >
   >     同样的，`<primary>` 和 `<secondary>` 的直属标题也是 H2
>
>- a: <u>a 属于 H3 标题</u>
   >
   >     `<primary>` 和 `<secondary>` 的链接名称都是 H3，其中 summary 属性的内容为标题下的文本
>- spotlight: <u>spotlight 属于 H3 标题</u>
   >
   >     下属的 `card` 有且只能有两个

标记区分可以根据下方图片记忆，在这些卡片块中，越靠前的越重要。

![页面来自官网](section-starting-page.png)

### misc 自定义扩展

在 `<section-starting-page>` 中，除了前面说的那些标记，还有一个用于扩展自定义块和链接到起始页的标记：**`<misc>`**

misc 和前面的区别就是你可以根据需要在其中进行任何数量的扩展，而不受到影响。

```XML

<misc>
    <cards>
        <title>cards</title>
        <a href="#" summary="1">card1</a>
        <a href="#" summary="2">card2</a>
        <a href="#" summary="3">card3</a>
        <a href="#" summary="4">card4</a>
    </cards>
    <links>
        <title>links</title>
        <group>
            <title>More related topics</title>
            <a href="https://www.baidu.com" summary="百度">Baidu</a>
            <a href="https://www.qq.com" summary="腾讯">Tencent</a>
            <a href="https://www.alibaba.com" summary="阿里巴巴">Alibaba</a>
        </group>
    </links>
</misc>
```

下面的图片来自官网底部，上面标记的 `<links>` 实现的就是这一部分

`<cards>` 部分与前面提过的 `<primary>` 表现上是一致的，区别是标记不一样~~笑~~

![页面来自官网](image.png)

### 起始页结构

```XML
<?xml version="1.0" encoding="UTF-8"?>
<topic xmlns:xsi="一个schema链接"
       xsi:noNamespaceSchemaLocation="对XML特化的语法定义"
       title="标题名称" id="标题名称" help-id="Section-Starting-Page;WriterSide">
    <section-starting-page>
        <title>页面标题</title>
        <description>说明文字</description>
        <spotlight><!-- 只能有两个card --></spotlight>
        <primary></primary>
        <secondary></secondary>
        <misc>
            <cards></cards>
            <links>
                <title>links title</title>
                <group></group>
            </links>
        </misc>
    </section-starting-page>
</topic>
```
