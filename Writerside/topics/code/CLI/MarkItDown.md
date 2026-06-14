# MarkItDown

[MarkItDown](https://github.com/microsoft/markitdown) 是一款轻量级的 Python 工具，用于将各种文件转换为 Markdown。

markitdown 定位是 **内容提取**，非保真排版。它类似于 textract，但更注重保留重要的文档结构和内容（包括：标题、列表、表格、链接等）。

## 安装

MarkItDown 需要 Python 3.10 或更高版本

```Bash
# 不安装依赖
pip install markitdown

# 安装额外依赖
pip install 'markitdown[pdf, docx, pptx]'

# 安装所有依赖
pip install 'markitdown[all]'
```

<deflist>
<def title="可用依赖">

- `all` 安装所有可选依赖
- `pptx` 安装 PowerPoint 文件的依赖
- `docx` 安装 Word 文件的依赖
- `xlsx` 安装 Excel 文件的依赖
- `xls`安装旧版 Excel 文件的依赖
- `pdf`安装 PDF 文件的依赖
- `outlook`安装 Outlook 消息的依赖
- `az-doc-intel` 安装 Azure Document Intelligence 的依赖
- `az-content-understanding` 安装 Azure Content Understanding 的依赖
- `audio-transcription` 安装 wav 和 mp3 文件音频转录的依赖
- `youtube-transcription`安装依赖以获取 YouTube 视频转录

</def>
</deflist>

### 命令行转换

HTML 转换为 Markdown 是内置的，不需要额外安装

```bash
# 基础转换（输出到终端）
markitdown input.html

# 输出到文件
markitdown input.html -o output.md

# 转换目录下所有 HTML 文件
markitdown ./docs/*.html -o ./docs/
```

### python api

#### 单文件转换

```Python
from markitdown import MarkItDown

# 初始化转换器
md_converter = MarkItDown()

# 转换单个文件
result = md_converter.convert("input.html")
markdown_text = result.text_content

# 保存结果
with open("output.md", "w", encoding="utf-8") as f:
    f.write(markdown_text)

print("✅ 转换完成")
```

#### 批量转换
```Python
import os
from pathlib import Path
from markitdown import MarkItDown

converter = MarkItDown()
input_dir = Path("./html_files")
output_dir = Path("./md_files")
output_dir.mkdir(exist_ok=True)

for html_file in input_dir.glob("*.html"):
    result = converter.convert(str(html_file))
    md_path = output_dir / f"{html_file.stem}.md"
    md_path.write_text(result.text_content, encoding="utf-8")
    print(f"📄 {html_file.name} -> {md_path.name}")
```

### 问题
markitdown 依赖 HTML 的语义标签。若网页使用 `<div>` 模拟表格或重度依赖 JS 渲染，建议先用 html5lib 或 readability-lxml 预处理提取正文。