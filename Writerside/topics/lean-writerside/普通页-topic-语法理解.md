<show-structure depth="3"/>

# 普通页 topic 语法理解

包含除 section-starting-page 及子元素之外的元素

本文档所有标记，除非标明，否则无法使用在 `<section-starting-page>` 中

## 固定内容部分

这里没什么好说的，直接看 [起始页 Topic 语法理解](起始页-topic-语法理解.md#permanent)

## 正文部分

> 在正式开始之前，先介绍一个标记`<br>`,该标记用于换行操作，除此之外别无其它用途，

一个全新的 topic 格式如下

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

是的，即使是起始页，也不过是将 `<p>` 删除，增加 `<section-starting-page>` 而已。也就是说，在哪个 topic 中定义了
`<section-starting-page>`，哪个文档就是起始页。

### show-structure

这个标记完美解决了 Writerside 只显示标题到 H2 的问题

<tabs>
<tab title="Topic">

```xml

<show-structure for="chapter,procedure" depth="3"/>
```

<deflist>
<def title="for">指定谁可以成为导航中的标题，只允许 chapter 和 procedure</def>
<def title="depth">指定导航最大标题深度，标题从 H1 到 H6，深度也是 1~6</def>
</deflist>

> 此方式本质上是将非 H2 标题添加到导航栏，不如 Markdown 有缩进，能看到章节之间是否有归属
> {style="note"}

</tab>
<tab title="Markdown">

```markdown
<show-structure depth="3"/>

# show-structure demo

## H2

### H3

#### H4

```

> 凡是控制页面结构/目录的标签（比如 show-structure），尽量放在文件最顶部或用 frontmatter（md 支持的 yaml 语法）
> {style=note}
</tab>
</tabs>

### quote

<tabs>
<tab title="note">

```XML

<note>这是一个note引用</note>
```

<note> 这是一个 note 引用 </note>

</tab>
<tab title="tip">

```XML

<tip>这是一个tip引用</tip>
```

<tip> 这是一个 tip 引用 </tip>

</tab>
<tab title="warning">

```XML

<waring>这是一个warning引用</waring>
```

<warning>这是一个 warning 引用</warning>

</tab>
</tabs>

### 超链接标签

#### 锚点

如果为一段文字增加了 id，那么可以通过`<a>`跳转到该段文字

```XML

<p id="anchor-p">this is anchored</p>
        <!-- 跳转到指定文件的指定锚点-->
<a href="one.topic" anchor="anchor-p">Link to anchor</a>
```

<deflist>
<def title="属性">

- href(可选): 指定文字所在文档
- anchor(必选): 指向 id，跳转到该 id

</def>
</deflist>

#### 外部链接

```XML

<a href="https://www.baidu.com">百度</a>
```

<deflist>
<def title="属性">

- href(必选) 打开指定的链接

> href 的值也可以是本地文档
</def>
</deflist>

### Code

在页面中嵌入代码是很正常的事情，而代码的嵌入分为两种：行内代码 `<code>` 和代码块` <code-block>`

<tabs>
<tab title="<code>">

```XML

<p>行内代码
    <code>foo()</code>
</p>
```

<p>行内代码 <code>foo()</code></p>

</tab>
<tab title="<code-block>">

```XML

<code-block lang="python">
    def add(a:int,b:int):
    return a+b
</code-block>
```

<br>
<code-block lang="python">
def add(a:int,b:int):
    return a+b
</code-block>

<deflist>
<def title="collapsible：折叠标记">

搭配 collapsed-title 可以指定折叠后的代码块的标题
> 这里写折叠标记而非折叠代码块是因为很多标记都支持使用 `collapsible`

```XML

<code-block lang="python" collapsed-title="add()" collapsible="true">
    def add(a:int,b:int):
    return a+b
</code-block>
```

<br>
<code-block lang="python" collapsed-title="add()" collapsible="true">
def add(a:int,b:int):
    return a+b
</code-block>
</def>
<def title="prompt: 提示标记">

当代码块中的内容是终端命令时这会是很好的提示信息，而且不会被随命令被复制

```XML

<code-block lang="bash" prompt="$">
    cat main.py
    def main():
    print("hello world!")
    pass

    if __name__=="__main__":
    main()
    pass
</code-block>
```

<br>
<code-block lang="bash" prompt="$">
cat main.py
def main():
    print("hello world!")
    pass

if __name__=="__main__":
main()
pass
</code-block>
</def>
</deflist>
</tab>
<tab title="compare">

除此之外还有一个用于比较的标记`<compare>`，该标记会将两个代码块放在一起进行行级别的对比

```XML

<compare>
    <code-block lang="java">
        public static void main(String[] args) {
        System.out.println("Hi!");
        }
    </code-block>
    <code-block lang="python">
        if __name__=="__main__":
        print("Hi")
    </code-block>
</compare>

```

<deflist>
<def title="type:排序方式">

- `left-right`:默认, 左右排列
- `top-bottom`: 上下排列

<br>
<compare type="top-bottom">

```java
public static void main(String[] args) {
    System.out.println("Hi!");
}
```

```python
if __name__=="__main__":
    print("Hi")
```

</compare>
</def>
<def title="标题">

- `first-title`: 表示第一个代码块的标题
- `second-title`: 表示第二个代码块的标题

<br>

<compare first-title="Java" second-title="Python">

```java
public static void main(String[] args) {
    System.out.println("Hi!");
}
```

```  python
if __name__=="__main__":
    print("Hi")
```

</compare>
</def>
</deflist>
</tab>
</tabs>

### control & path

<tabs>
<tab title="control">

效果等同于加粗，可以直接使用 markdown 的加粗替代

control、ui-path、b 是一样的，选然后字体大小是 16px

示例：指定配置文件:<control>path/to/config</control>

</tab>
<tab title="path">

`<path>` 标记是将文本半加粗的形式显示，此时将文件路径放入其中既能得到提示也不会和 `<control>` 混淆

path 渲染后的字体大小是 15px

```XML

<p>指定配置文件:
    <path>path/to/config</path>
</p>
```

指定配置文件:<path>path/to/config</path>

<deflist>
<def title="路径示例">

举例：我需要修改 idea 中的字体，其他人如何快速地完成找到字体设置的操作呢？

按照<path>文件 | 设置 | 编辑器 | 字体</path>就可以顺利找到字体设置进行修改

</def>
</deflist>

</tab>
</tabs>

### format

可以更改文本的字体样式和颜色，但不建议直接格式化文本

单独一行报错，如果作为一段文字的一部分就不报错，<format style="bold,italic" color="pink">离谱</format>！不过用来修改颜色倒合适

### img

<tabs>
<tab title="Topic">

```xml

<img src="image.png" alt="描述文字" thumbnail="true" style="inline|block" width="32"/>
```

</tab>
<tab title="Markdown">

```text
![描述文字](image.png){style="inline|block" width="32"  thumbnail="true"}
```

</tab>
</tabs>

<deflist>
<def title=" thumbnail='true'">
该属性能让小图片放大，以最大比例显示，可以作为缩略图来显示

使用 markdown 方式不能让标签被嵌套，否则无效
</def>
<def title="style">
<deflist>
    <def title="block">
<img src="education.svg" alt="aaa" style="block" width="32" /> asdd
</def>
<def title="inline">
<img src="education.svg"  alt="aaa" style="inline" width="32" /> asdd
</def>
</deflist> 无论是使用 block 还是 inline，都必须使用 width 才能达到效果
</def>
</deflist>

### include

`<include>` 可以插入部分文档到指定位置，使用 element-id 来决定是那一部分

除了文档，还可以搭配 `<snippet>` 来将设定好的片段插入到文档中，从使用上此方式更加灵活

<tabs>
<tab title="插入文档片段">

```XML

<include from="普通页topic语法理解.md" element-id="switcher-key"/>
```

此元素的效果是将归属 element-id 的所有内容全部插入到当前文档。类似于 Obsidian 的 `![[]]` 效果

<deflist>
    <def title="include">
        <include from="New-topic-功能.md" element-id="markdown-vs-xml"/>
    </def>
</deflist>
</tab>
<tab title="插入 snippet 片段">

使用`<snippet>` 片段需要先定义

```xml

<snippet id="settings">
    <p>You can change the following settings:</p>
    <list>
        <li>Volume</li>
        <li>Brightness</li>
        <li>Dimensions</li>
    </list>
</snippet>
```

通过 include 插入，语法上和使用文档片段是一样的

```XML

<include from="定义片段的文档" element-id="snippet的id"/>
```

<deflist>
<def title="include">
<include from="lw-library.md" element-id="demo"/>
</def>
</deflist>

</tab>
</tabs>


---
更多语法，请查看 [](普通页1.md)