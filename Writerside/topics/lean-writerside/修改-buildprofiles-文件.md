# 修改 buildprofiles 文件

buildprofile.xml 是 Writerside 的核心配置文件，用于配置构建过程和输出，如页眉和页脚、搜索设置、快捷方式和版本切换器。

为了增加个性化，可能会需要修改 logo 等信息

Writerside 提供了 buildprofiles.xml 文件来进行个性化设置

## 基础修改

```XML

<variables>
    <!--
    自定义Logo
    高宽比在1.2和0.24之间。建议使用方形图。
    图片高度大于48px。
    -->
    <header-logo>logo.svg</header-logo>
    <!-- 为logo增加连接 -->
    <product-web-url>https://gitee.com</product-web-url>
    <!-- 设置favicon -->
    <custom-favicons>logo.svg</custom-favicons>
    <!-- 使用自定义的css文件，地址为Writerside/cfg/static -->
    <custom-css>custom.css</custom-css>
</variables>
```

favicon 和 logo 的区别：favicon 是一种缩略图；logo 是页面内表示公司/组织的图标。

在浏览器页面标签的位置会出现 *logo+页面名称* 的形式，此处的 logo 其实就是 favicon

更多需求可以查看官方文档中关于[buildprofiles.xml](https://www.jetbrains.com/help/writerside/buildprofiles-xml.html)的部分

## footer

```XML

<footer>
    <social type="email" href="2833171898@qq.com">Email</social>
    <social type="discord" href="#">discord</social>
    <social type="github" href="#">github</social>
    <social type="location" href="#">location</social><!-- 用于定位公司/组织办公地点的， -->
    <link href="#">友链 1</link>
    <link href="#">友链 2</link>
    <link href="#">友链 3</link>
    <copyright>2026 ZhanYu</copyright>
    <icp>ICP 备案</icp>
</footer>
```

<deflist>
<def title="Type">
social 标签提供了 25 中类型

- apple
- bilibili
- bitbucket
- blog
- bluesky
- bugtracker
- discord
- email
- facebook
- github
- gitlab
- googleapp
- instagram
- linkedin
- location
- mastodon
- reddit
- rss
- slack
- stackoverflow
- telegram
- twitter
- wechat
- weibo
- youtube
-
{columns=4}
</def>
</deflist>

## Bugs

### 1. 选项卡内容占用

在 2026.04.8711 版本中，CSS 渲染 Tbas 会出现选项卡内容占用的情况，导致靠后的选项卡内容也会往下降

![css-error](css-error.png)

在浏览器中通过 F12 检查到 css 中的 display 值应为 none ,但却使用了 block，将其改回效果和官网示例效果相同

现通过自定义 css 文件的形式强行修改样式

<tabs>
<tab title="custom.css">

修改 css 的时候一定要最小化修改样式，减少不必要的麻烦

```css
.tabs__content-wrapper[hidden=until-found] {
    display: none;
}
```

<deflist>
<def title="css位置">
Writerside/cfg/static/
</def>
</deflist>
</tab>
<tab title="buildprofiles.xml">

```XML

<variables>
    <custom-css>custom.css</custom-css>
</variables>
```

在 custom-css 中声明自定义的 css 文件即可生效
</tab>
</tabs>