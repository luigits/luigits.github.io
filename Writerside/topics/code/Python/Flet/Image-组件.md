# Image 组件

支持以下流行格式

- JPEG
- PNG
- SVG
- GIF
- 动画 GIF
- WebP
- 动画 WebP
- BMP
- WBMP

{columns=3}

## 示例

```Python
ft.Image(
    src="https://flet.dev/img/logo.svg",
    width=100,
    height=100,
)
```

### 属性

| 属性                             | 说明                                           |
|--------------------------------|----------------------------------------------|
| anti_alias                     | 是否对图像进行抗锯齿处理。                                |
| border_radius                  | 设置圆角                                         |
| cache_height                   | 图像应该被解码的大小 (高度)                              |
| cache_width                    | 图像应该被解码的大小（宽度)                               |
| color                          | 如果设置，该颜色将使用 color_blend_mode 与每个图像像素混合。      |
| color_blend_mode               | 用于将颜色与图像相结合                                  |
| error_content                  | 无法从提供的源加载图像时显示的回退控件                          |
| exclude_from_semantics         | 是否从语义中排除此图像                                  |
| fade_in_animation              | 图像在加载后出现的 src 淡入动画，替换 placeholder_src        |
| filter_quality                 | 图像的渲染质量                                      |
| fit                            | 定义如何在布局时将该图像记录到分配的空间中                        |
| gapless_playback               | 当图像提供者改变时，是继续显示旧图像 (True)，还是暂时什么都不显示 (False) |
| placeholder_fade_out_animation | 在 src 加载后，placeholder_src 的淡出动画              |
| placeholder_fit                | 定义如何将占位符嵌入其空间                                |
| placeholder_src                | 图像加载时显示的占位符                                  |
| repeat                         | 如何绘制布局边界中未被此图像覆盖的任何部分                        |
| semantics_label                | 该图像的语义描述                                     |
| src                            | 图像源                                          |

### assets 目录

当使用图片等外部资源时，可能会引发无法正常加载的问题，这是由于无法找到正确的资源目录导致。

```Python
# 使用felt程序正常显示
ft.run(main)
# 使用web无法加载
ft.run(main,assets_dir="src/assets")
```
在实际的使用下来发现，只要指定的 assets_dir 目录下有需要的外部资源文件即可，并不需要精确到在哪里，也就是说下面的两个设置都是能正常使用的

<compare first-title="精确目录" second-title="宽泛目录" type="left-right">
<code-block lang="python">
ft.run(main,assets_dir="src/assets")
</code-block>
<code-block lang="python">
ft.run(main,assets_dir="src")
</code-block>
</compare>

可以将 assets_dir 常驻，让外部资源能正常加载，即便不使用外部资源。
> 疑惑：不设置 assets_dir，直接使用其中的 icon.png 也正常加载了，而另一个则没有加载

{style="warning"}