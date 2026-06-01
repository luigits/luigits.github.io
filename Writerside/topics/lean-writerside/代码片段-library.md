# 代码片段 library

Writerside 默认提供一个实例 (instance) 和一个片段库 (snippets library)。

## 实例
没什么好说的，Writerside 的核心功能区，绝大部分的编写行为都在实例中完成

## 片段库

在学习 Markup 中 `<snippet>` 时进行了尝试，当时的选择是创建一个演示文档，将 `<snippet>` 在另一个文档中使用 `<include>` 引入

现在我们可以将 `<snippet>` 放入 snippets library 中，按照该库的说明，只有被使用的 `<snippet>` 才会生效编译，这会节省资源的开销

### 片段库原文

```markdown
Any topic file marked with `is-library: true` is considered a library topic.
任何标有 `is-library: true` 的主题文件都被视为库主题。

If you want an XML library, create an XML topic file with `is-library="true"` in the root `<topic>` element.
如果您想要一个 XML 库，请创建一个 XML 主题文件，在根 `<topic>` 元素中使用 `is-library="true"`。

Library topics should not be used in instances,which is why they do not require a title.
库主题不应该在实例中使用，这就是它们不需要标题的原因。

Writerside will not build library topics because they can contain markup that is not valid(as long as it is valid where it is included).
Writerside 不会构建 library 主题，因为它们可能包含无效的标记 (只有在包含它的地方是有效的)。

Although you can include any element with an ID from any topic file, it is recommended to wrap reusable elements with `<snippet>` and keep them in these dedicated library files.
虽然您可以使用 include 导入任何主题文件中带有 ID 的任何元素，但是建议您用 `<snippet>` 包装可重用的元素，并将它们保存在这些专用的 library 文件中。

You can have one library for the whole project or several libraries for different groups of snippets.
您可以为整个项目创建一个 library，也可以为不同的代码段组创建几个 library。

Do not add library topics as `<toc-element>` in tree files,except the dedicated tree file for the **Libraries** view.
不要将 library 主题作为`<toc-element>` 添加到 tree 文件中，除非是 **Libraries** 视图的专用 tree 文件。
 ```
 