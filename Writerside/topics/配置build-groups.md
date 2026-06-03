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
