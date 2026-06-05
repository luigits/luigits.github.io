<show-structure depth="3"/>

# Ollama

ollama 是本地 AI 部署程序，通过 ollama 将 AI 在本地运行，但受制于本地硬件，需要各方取舍。

## 安装 Ollama

<tabs>
    <tab title="Scoop">
        <code-block lang="shell">scoop install ollama</code-block>
    </tab>
    <tab title="Winget">
        <code-block lang="shell">winget install ollama -i</code-block>
    </tab>
</tabs>

## 部署 AI 模型到本地

部署 AI 模型需要启动 Ollama 的服务

```shell
# 启动 ollama 服务
ollama serve
# 列出已下载的模型
ollama list
# 列出运行的模型
ollama ps

# 下载远程仓库中的模型
ollama pull qwen-3.5
# 删除模型
ollama rm qwem-3.5
# 运行模型
ollama run qwen-3.5 # 如果本地没有则会先下载再运行
# 停止运行的模型
ollama stop qwen-3.5
# 显示模型的详细信息
ollama show qwen-3.5
```

## 硬件支持表

llama 支持 CPU 和 GPU 两种推理方式，但速度差距显著。以下是各模型规模对应的硬件建议：

|    模型规模    |      最低显存       |               推荐配置                |         推理速度参考          |
|:----------:|:---------------:|:---------------------------------:|:-----------------------:|
|  1B–3B 参数  | 无需 GPU（CPU 可运行） |              8GB RAM              | 约 30–60 tok/s（Apple M2） |
|  7B–8B 参数  |     8GB 显存      |  NVIDIA RTX 3080 / Apple M2 Pro   |   约 40–80 tok/s（GPU）    |
| 13B–14B 参数 |     12GB 显存     | NVIDIA RTX 3080 Ti / Apple M3 Max |      约 25–45 tok/s      |
| 30B–34B 参数 |     24GB 显存     | NVIDIA RTX 4090 / Apple M2 Ultra  |      约 15–25 tok/s      |
|   70B 参数   |     48GB 显存     |    双卡 RTX 4090/ Apple M2 Ultra    |      约 8–15 tok/s       |

### 4G 显存已知可用模型

| name           | size  | context | input       |
|----------------|-------|---------|-------------|
| deepseek-r1:8b | 5.2GB | 128K    | Text        |
| gemma3n:e4b    | 7.5GB | 32K     | Text        |
| gemma3:4b      | 3.3GB | 128K    | Text, Image |
| qwen3.5:9b     | 6.6GB | 256K    | Text, Image |

在表中的所有模型中，能流畅运行的是`gemma3:4b`，其他模型多少都是会有隐患存在。

> **除非标明 4G，否则可能会有隐患导致硬件寿命缩减**
>
> 本人电脑是一台 2018 年购入的联想拯救者，除了偶尔看看视频和写写文档代码之类的，其他性能需求高的用途无法胜任
>
>因此在使用上并不觉得难受，毕竟它已经稳定运行 7 年了。
>
{style="warning"}