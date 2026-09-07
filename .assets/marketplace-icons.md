# Marketplace 图标与依赖升级记录

设计包：`marketplace-icons.zip`，导出日期为 2026-09-07。
压缩包 SHA-256：`4adfdb1442986b3f40c61c50bfc6689cd18f11293252aa1dca345c91bf91f724`。

## 构成与主题判定

| 分类 | SVG 数量 | 亮色 | 暗色 |
| --- | ---: | ---: | ---: |
| `langgenius` | 326 | 163 | 163 |
| `rag` | 18 | 12 | 6 |
| `rag_data_source` | 47 | 24 | 23 |
| 合计 | 391 | 199 | 192 |

另有 README、CSV / JSON 映射清单及 4 张预览图。
全部 SVG 均为 40×40，其中 71 个内嵌位图，没有外部图片依赖。
`_light` / `_dark` 表示设计稿所在列，不代表两份文件一定存在视觉差异。

在 Chromium 中以 160×160 透明画布逐一渲染 192 对图标，57 对逐像素一致，19 对只存在导出坐标精度与抗锯齿差异，这 76 对按亮暗共用处理。
其余 116 对存在实际颜色、透明度、背景或形状差异，保留新版暗色。
7 个仅亮色文件包括两个分类的 `aws_s3`、`dify_extractor`、`docs_extractor` 和三个 chunker。

## 落地规则

- 每个已匹配插件仅使用 `_assets/icon.svg`，存在实际主题差异时再增加 `_assets/icon-dark.svg`。
- 共用图标省略 `icon_dark` / `icon_small_dark` / `icon_large_dark`，使用现有的亮色回退。
- manifest、provider、现有工具定义与模型定义中的图标引用同步更新。
- 模型大小图及语言版本指向同一份新版素材，删除旧尺寸、旧语言、旧格式与旧暗色图标。
- 数据源 identity 不支持暗色字段，因此仅在它的 manifest 中声明暗色；不添加无效配置。
- 不同插件分别打包，同品牌插件各保留自己的资源；不引入跨插件路径或符号链接。
- 清理失去引用的旧图标，保留文档截图，并同步 Mixedbread README 与 Baserow / Pushover 的代码图标引用。
- 200 个复核无异议插件的 manifest 顶层版本提升一个 patch；原本与 manifest 版本一致的项目版本同步更新。
- 运行、开发与构建依赖升级至 2026-09-07 PyPI 最新稳定版本，并重新生成 `uv.lock`。

## 覆盖范围

| 插件类别 | 已更新 | 两个主题文件 | 单文件共用 | 待补素材或确认 |
| --- | ---: | ---: | ---: | ---: |
| `models` | 56 | 36 | 20 | 15 |
| `tools` | 111 | 66 | 45 | 38 |
| `datasources` | 16 | 10 | 6 | 1 |
| `triggers` | 10 | 6 | 4 | 7 |
| `extensions` | 7 | 3 | 4 | 0 |
| `agent-strategies` | 0 | 0 | 0 | 2 |
| 合计 | 200 | 121 | 79 | 63 |

共落地 321 个 SVG，使用 175 组源图标。
原有 280 个图标文件被覆盖或清理，其中 171 个旧路径删除。
79 个共用图标插件中，74 个省略视觉重复的暗色素材，5 个源设计只有亮色。
`tools/aihubmix_image/_assets/icon-dark.svg` 已删除，其暗色显示回退到新版亮色图标。

## 映射中的特殊情况

- `autonavi` → `tools/gaode`；`aws_tools` → `tools/aws`；`xai` → `models/x`；`zhipu` → `models/zhipuai` 和 `tools/zhipuai`。
- `maas` 对应 Huawei MaaS 及其香港版本；Volcengine MaaS 使用 `volcengine`，两者不能混淆。
- `nvidia_nim` 与 `nvidia`、Volcengine 的两个模型插件原本共享同品牌图案，统一迁移。
- 同品牌模型、图片 / 视频 / 语音工具、触发器及机器人复用相应新版图案；Google Calendar、Gmail 等独立产品没有使用 Google 通用标识代替。
- AWS S3 使用绿色 `aws_s3_light.svg`；红色 `aws_s3_classic` 是另一套款式，不能拿它的 dark 配绿色亮色。
- OpenAI 工具已涵盖 GPT Image 与 Deep Research，使用 OpenAI 新图标；没有使用范围较窄的 `dalle` 图案。
- `mistralai_pure` 是另一款无背景图案，现有 Mistral 插件使用 `mistralai`。
- DevDocs 的透明度、Moonshot 的渐变、Feishu / Lark 的背景颜色存在实际差异，保留各自暗色。
- SiliconFlow 暗色主体比亮色大约 2.47%，按源稿保留；这不是抗锯齿噪声。
- `tools/chart` 对应的 `chart_generator` 素材实际是 Hugging Face 图案，暂留原图标并等待修正。

## 完整落点

源列省略 `_light.svg` / `_dark.svg` 后缀。
所有目标均位于对应插件的 `_assets/` 中。
“逐像素共用”表示亮暗渲染相同，“等价共用”表示只有导出精度差异，“仅亮色”表示源稿没有配套暗色。

| 插件目录 | 源图标 | 主题处理 | 版本 |
| --- | --- | --- | --- |
| [datasources/aws_s3_storage](../datasources/aws_s3_storage/_assets/icon.svg) | `rag_data_source/aws_s3` | 仅亮色 | `0.3.12` → `0.3.13` |
| [datasources/azure_blob](../datasources/azure_blob/_assets/icon.svg) | `rag_data_source/azure_blob` | 等价共用 | `0.2.14` → `0.2.15` |
| [datasources/box_datasource](../datasources/box_datasource/_assets/icon.svg) | `rag_data_source/box` | 逐像素共用 | `0.1.7` → `0.1.8` |
| [datasources/brightdata_datasource](../datasources/brightdata_datasource/_assets/icon.svg) | `rag_data_source/bright_data` | 等价共用 | `0.1.10` → `0.1.11` |
| [datasources/confluence_datasource](../datasources/confluence_datasource/_assets/icon.svg) | `rag_data_source/confluence` | 逐像素共用 | `0.2.9` → `0.2.10` |
| [datasources/dropbox_datasource](../datasources/dropbox_datasource/_assets/icon.svg) | `rag_data_source/dropbox` | 逐像素共用 | `0.2.8` → `0.2.9` |
| [datasources/firecrawl_datasource](../datasources/firecrawl_datasource/_assets/icon.svg) | `rag_data_source/firecrawl` | 亮 + 暗 | `0.2.13` → `0.2.14` |
| [datasources/github](../datasources/github/_assets/icon.svg) | `rag_data_source/github` | 亮 + 暗 | `0.4.7` → `0.4.8` |
| [datasources/gitlab_datasource](../datasources/gitlab_datasource/_assets/icon.svg) | `rag_data_source/gitlab` | 亮 + 暗 | `0.3.12` → `0.3.13` |
| [datasources/google_cloud_storage](../datasources/google_cloud_storage/_assets/icon.svg) | `rag_data_source/google_cloud` | 亮 + 暗 | `0.2.13` → `0.2.14` |
| [datasources/google_drive](../datasources/google_drive/_assets/icon.svg) | `rag_data_source/google_drive` | 亮 + 暗 | `0.1.13` → `0.1.14` |
| [datasources/jina_datasource](../datasources/jina_datasource/_assets/icon.svg) | `rag_data_source/jina` | 亮 + 暗 | `0.0.10` → `0.0.11` |
| [datasources/notion_datasource](../datasources/notion_datasource/_assets/icon.svg) | `rag_data_source/notion` | 亮 + 暗 | `0.1.21` → `0.1.22` |
| [datasources/onedrive](../datasources/onedrive/_assets/icon.svg) | `rag_data_source/onedrive` | 亮 + 暗 | `1.0.0` → `1.0.1` |
| [datasources/sharepoint_datasource](../datasources/sharepoint_datasource/_assets/icon.svg) | `rag_data_source/share_point` | 亮 + 暗 | `1.0.0` → `1.0.1` |
| [datasources/tavily_datasource](../datasources/tavily_datasource/_assets/icon.svg) | `rag_data_source/tavily` | 亮 + 暗 | `0.1.10` → `0.1.11` |
| [extensions/aws_bedrock_knowledge_base](../extensions/aws_bedrock_knowledge_base/_assets/icon.svg) | `langgenius/aws_bedrock_knowledge_base` | 亮 + 暗 | `0.0.9` → `0.0.10` |
| [extensions/badapple](../extensions/badapple/_assets/icon.svg) | `langgenius/badapple` | 逐像素共用 | `0.0.6` → `0.0.7` |
| [extensions/llamacloud](../extensions/llamacloud/_assets/icon.svg) | `langgenius/llamacloud` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [extensions/oaicompat_dify_model](../extensions/oaicompat_dify_model/_assets/icon.svg) | `langgenius/oaicompat_dify_model` | 逐像素共用 | `0.0.10` → `0.0.11` |
| [extensions/openai_compatible](../extensions/openai_compatible/_assets/icon.svg) | `langgenius/oaicompat_dify_app` | 逐像素共用 | `0.0.15` → `0.0.16` |
| [extensions/slack_bot](../extensions/slack_bot/_assets/icon.svg) | `langgenius/slack` | 亮 + 暗 | `0.0.11` → `0.0.12` |
| [extensions/wecom_bot](../extensions/wecom_bot/_assets/icon.svg) | `langgenius/wecom` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [models/aihubmix](../models/aihubmix/_assets/icon.svg) | `langgenius/aihubmix` | 等价共用 | `0.0.45` → `0.0.46` |
| [models/anthropic](../models/anthropic/_assets/icon.svg) | `langgenius/anthropic` | 逐像素共用 | `0.3.30` → `0.3.31` |
| [models/azure_ai_studio](../models/azure_ai_studio/_assets/icon.svg) | `langgenius/azure_ai_studio` | 亮 + 暗 | `0.0.19` → `0.0.20` |
| [models/azure_openai](../models/azure_openai/_assets/icon.svg) | `langgenius/azure_openai` | 亮 + 暗 | `0.0.69` → `0.0.70` |
| [models/baichuan](../models/baichuan/_assets/icon.svg) | `langgenius/baichuan` | 亮 + 暗 | `0.0.11` → `0.0.12` |
| [models/bedrock](../models/bedrock/_assets/icon.svg) | `langgenius/bedrock` | 亮 + 暗 | `0.0.83` → `0.0.84` |
| [models/chatglm](../models/chatglm/_assets/icon.svg) | `langgenius/chatglm` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [models/cohere](../models/cohere/_assets/icon.svg) | `langgenius/cohere` | 亮 + 暗 | `0.0.18` → `0.0.19` |
| [models/deepseek](../models/deepseek/_assets/icon.svg) | `langgenius/deepseek` | 逐像素共用 | `0.0.21` → `0.0.22` |
| [models/fireworks](../models/fireworks/_assets/icon.svg) | `langgenius/fireworks` | 逐像素共用 | `0.0.13` → `0.0.14` |
| [models/fishaudio](../models/fishaudio/_assets/icon.svg) | `langgenius/fish_audio_tool` | 亮 + 暗 | `0.0.9` → `0.0.10` |
| [models/gemini](../models/gemini/_assets/icon.svg) | `langgenius/gemini` | 亮 + 暗 | `0.9.8` → `0.9.9` |
| [models/gitee_ai](../models/gitee_ai/_assets/icon.svg) | `langgenius/gitee_ai` | 逐像素共用 | `0.1.10` → `0.1.11` |
| [models/gpustack](../models/gpustack/_assets/icon.svg) | `langgenius/gpustack` | 逐像素共用 | `0.0.15` → `0.0.16` |
| [models/huaweicloud_maas](../models/huaweicloud_maas/_assets/icon.svg) | `langgenius/maas` | 等价共用 | `0.0.26` → `0.0.27` |
| [models/huaweicloud_maas_hk](../models/huaweicloud_maas_hk/_assets/icon.svg) | `langgenius/maas` | 等价共用 | `0.0.9` → `0.0.10` |
| [models/huggingface_hub](../models/huggingface_hub/_assets/icon.svg) | `langgenius/huggingface_hub` | 亮 + 暗 | `0.0.11` → `0.0.12` |
| [models/huggingface_tei](../models/huggingface_tei/_assets/icon.svg) | `langgenius/huggingface_tei` | 亮 + 暗 | `0.1.10` → `0.1.11` |
| [models/hunyuan](../models/hunyuan/_assets/icon.svg) | `langgenius/hunyuan` | 亮 + 暗 | `0.0.13` → `0.0.14` |
| [models/jina](../models/jina/_assets/icon.svg) | `langgenius/jina` | 亮 + 暗 | `0.0.23` → `0.0.24` |
| [models/llama_api](../models/llama_api/_assets/icon.svg) | `langgenius/llama_api` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [models/localai](../models/localai/_assets/icon.svg) | `langgenius/localai` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [models/minimax](../models/minimax/_assets/icon.svg) | `langgenius/minimax` | 亮 + 暗 | `0.0.26` → `0.0.27` |
| [models/mistralai](../models/mistralai/_assets/icon.svg) | `langgenius/mistralai` | 等价共用 | `0.0.13` → `0.0.14` |
| [models/mixedbread](../models/mixedbread/_assets/icon.svg) | `langgenius/mixedbread` | 亮 + 暗 | `0.0.9` → `0.0.10` |
| [models/modelscope](../models/modelscope/_assets/icon.svg) | `langgenius/modelscope` | 亮 + 暗 | `0.0.16` → `0.0.17` |
| [models/moonshot](../models/moonshot/_assets/icon.svg) | `langgenius/moonshot` | 亮 + 暗 | `0.1.12` → `0.1.13` |
| [models/nomic](../models/nomic/_assets/icon.svg) | `langgenius/nomic` | 等价共用 | `0.0.9` → `0.0.10` |
| [models/novita](../models/novita/_assets/icon.svg) | `langgenius/novita` | 亮 + 暗 | `0.0.10` → `0.0.11` |
| [models/nvidia](../models/nvidia/_assets/icon.svg) | `langgenius/nvidia` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [models/nvidia_nim](../models/nvidia_nim/_assets/icon.svg) | `langgenius/nvidia` | 亮 + 暗 | `0.0.10` → `0.0.11` |
| [models/oci](../models/oci/_assets/icon.svg) | `langgenius/oci` | 逐像素共用 | `0.0.17` → `0.0.18` |
| [models/ollama](../models/ollama/_assets/icon.svg) | `langgenius/ollama` | 亮 + 暗 | `1.0.1` → `1.0.2` |
| [models/openai](../models/openai/_assets/icon.svg) | `langgenius/openai` | 亮 + 暗 | `1.0.4` → `1.0.5` |
| [models/openllm](../models/openllm/_assets/icon.svg) | `langgenius/openllm` | 亮 + 暗 | `0.0.10` → `0.0.11` |
| [models/openrouter](../models/openrouter/_assets/icon.svg) | `langgenius/openrouter` | 亮 + 暗 | `0.1.7` → `0.1.8` |
| [models/replicate](../models/replicate/_assets/icon.svg) | `langgenius/replicate` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [models/sagemaker](../models/sagemaker/_assets/icon.svg) | `langgenius/sagemaker` | 逐像素共用 | `0.0.18` → `0.0.19` |
| [models/siliconflow](../models/siliconflow/_assets/icon.svg) | `langgenius/siliconflow` | 亮 + 暗 | `0.0.59` → `0.0.60` |
| [models/stepfun](../models/stepfun/_assets/icon.svg) | `langgenius/stepfun` | 亮 + 暗 | `0.1.2` → `0.1.3` |
| [models/tencent](../models/tencent/_assets/icon.svg) | `langgenius/tencent` | 逐像素共用 | `0.0.9` → `0.0.10` |
| [models/togetherai](../models/togetherai/_assets/icon.svg) | `langgenius/togetherai` | 等价共用 | `0.0.8` → `0.0.9` |
| [models/tongyi](../models/tongyi/_assets/icon.svg) | `langgenius/tongyi` | 亮 + 暗 | `0.2.18` → `0.2.19` |
| [models/triton_inference_server](../models/triton_inference_server/_assets/icon.svg) | `langgenius/triton_inference_server` | 逐像素共用 | `0.0.9` → `0.0.10` |
| [models/upstage](../models/upstage/_assets/icon.svg) | `langgenius/upstage` | 逐像素共用 | `0.0.11` → `0.0.12` |
| [models/vertex_ai](../models/vertex_ai/_assets/icon.svg) | `langgenius/vertex_ai` | 亮 + 暗 | `0.0.63` → `0.0.64` |
| [models/vessl_ai](../models/vessl_ai/_assets/icon.svg) | `langgenius/vessl_ai` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [models/volcengine](../models/volcengine/_assets/icon.svg) | `langgenius/volcengine` | 亮 + 暗 | `0.0.17` → `0.0.18` |
| [models/volcengine_maas](../models/volcengine_maas/_assets/icon.svg) | `langgenius/volcengine` | 亮 + 暗 | `0.0.53` → `0.0.54` |
| [models/voyage](../models/voyage/_assets/icon.svg) | `langgenius/voyage` | 亮 + 暗 | `0.0.13` → `0.0.14` |
| [models/wenxin](../models/wenxin/_assets/icon.svg) | `langgenius/wenxin` | 亮 + 暗 | `0.1.7` → `0.1.8` |
| [models/x](../models/x/_assets/icon.svg) | `langgenius/xai` | 亮 + 暗 | `0.0.25` → `0.0.26` |
| [models/xinference](../models/xinference/_assets/icon.svg) | `langgenius/xinference` | 亮 + 暗 | `0.0.15` → `0.0.16` |
| [models/yi](../models/yi/_assets/icon.svg) | `langgenius/yi` | 逐像素共用 | `0.0.10` → `0.0.11` |
| [models/zhinao](../models/zhinao/_assets/icon.svg) | `langgenius/zhinao` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [models/zhipuai](../models/zhipuai/_assets/icon.svg) | `langgenius/zhipu` | 亮 + 暗 | `0.0.34` → `0.0.35` |
| [tools/aihubmix_image](../tools/aihubmix_image/_assets/icon.svg) | `langgenius/aihubmix` | 等价共用 | `0.1.5` → `0.1.6` |
| [tools/aliyuque](../tools/aliyuque/_assets/icon.svg) | `langgenius/aliyuque` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [tools/alphavantage](../tools/alphavantage/_assets/icon.svg) | `langgenius/alphavantage` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [tools/apitemplate](../tools/apitemplate/_assets/icon.svg) | `langgenius/apitemplate` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [tools/arxiv](../tools/arxiv/_assets/icon.svg) | `langgenius/arxiv` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [tools/aws](../tools/aws/_assets/icon.svg) | `langgenius/aws_tools` | 亮 + 暗 | `0.0.30` → `0.0.31` |
| [tools/azuredalle](../tools/azuredalle/_assets/icon.svg) | `langgenius/azuredalle` | 亮 + 暗 | `0.0.9` → `0.0.10` |
| [tools/baidu_translate](../tools/baidu_translate/_assets/icon.svg) | `langgenius/baidu_translate` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [tools/baserow](../tools/baserow/_assets/icon.svg) | `langgenius/baserow` | 亮 + 暗 | `0.0.6` → `0.0.7` |
| [tools/bing](../tools/bing/_assets/icon.svg) | `langgenius/bing` | 亮 + 暗 | `0.0.12` → `0.0.13` |
| [tools/bitbucket](../tools/bitbucket/_assets/icon.svg) | `langgenius/bitbucket` | 亮 + 暗 | `0.0.9` → `0.0.10` |
| [tools/brave](../tools/brave/_assets/icon.svg) | `langgenius/brave` | 亮 + 暗 | `0.0.10` → `0.0.11` |
| [tools/cogview](../tools/cogview/_assets/icon.svg) | `langgenius/cogview` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/comfyui](../tools/comfyui/_assets/icon.svg) | `langgenius/comfyui` | 逐像素共用 | `0.3.11` → `0.3.12` |
| [tools/confluence](../tools/confluence/_assets/icon.svg) | `langgenius/confluence` | 等价共用 | `0.1.0` → `0.1.1` |
| [tools/crossref](../tools/crossref/_assets/icon.svg) | `langgenius/crossref` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/devdocs](../tools/devdocs/_assets/icon.svg) | `langgenius/devdocs` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [tools/did](../tools/did/_assets/icon.svg) | `langgenius/did` | 等价共用 | `0.0.8` → `0.0.9` |
| [tools/dify_extractor](../tools/dify_extractor/_assets/icon.svg) | `rag/dify_extractor` | 仅亮色 | `0.1.0` → `0.1.1` |
| [tools/dingtalk](../tools/dingtalk/_assets/icon.svg) | `langgenius/dingtalk` | 逐像素共用 | `0.0.9` → `0.0.10` |
| [tools/discord](../tools/discord/_assets/icon.svg) | `langgenius/discord` | 等价共用 | `0.0.8` → `0.0.9` |
| [tools/dropbox](../tools/dropbox/_assets/icon.svg) | `langgenius/dropbox` | 逐像素共用 | `0.0.5` → `0.0.6` |
| [tools/duckduckgo](../tools/duckduckgo/_assets/icon.svg) | `langgenius/duckduckgo` | 亮 + 暗 | `0.0.11` → `0.0.12` |
| [tools/e2b](../tools/e2b/_assets/icon.svg) | `langgenius/e2b` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/echarts](../tools/echarts/_assets/icon.svg) | `langgenius/echarts` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/email](../tools/email/_assets/icon.svg) | `langgenius/email` | 逐像素共用 | `0.0.16` → `0.0.17` |
| [tools/fal](../tools/fal/_assets/icon.svg) | `langgenius/fal` | 等价共用 | `0.0.8` → `0.0.9` |
| [tools/feishu](../tools/feishu/_assets/icon.svg) | `langgenius/feishu` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/feishu_base](../tools/feishu_base/_assets/icon.svg) | `langgenius/feishu_base` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/feishu_calendar](../tools/feishu_calendar/_assets/icon.svg) | `langgenius/feishu_calendar` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/feishu_document](../tools/feishu_document/_assets/icon.svg) | `langgenius/feishu_document` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/feishu_message](../tools/feishu_message/_assets/icon.svg) | `langgenius/feishu_message` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/feishu_spreadsheet](../tools/feishu_spreadsheet/_assets/icon.svg) | `langgenius/feishu_spreadsheet` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/feishu_task](../tools/feishu_task/_assets/icon.svg) | `langgenius/feishu_task` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/feishu_wiki](../tools/feishu_wiki/_assets/icon.svg) | `langgenius/feishu_wiki` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/firecrawl](../tools/firecrawl/_assets/icon.svg) | `langgenius/firecrawl` | 亮 + 暗 | `0.2.2` → `0.2.3` |
| [tools/fishaudio](../tools/fishaudio/_assets/icon.svg) | `langgenius/fish_audio_tool` | 亮 + 暗 | `0.0.14` → `0.0.15` |
| [tools/gaode](../tools/gaode/_assets/icon.svg) | `langgenius/autonavi` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [tools/gemini_image](../tools/gemini_image/_assets/icon.svg) | `langgenius/gemini` | 亮 + 暗 | `0.1.9` → `0.1.10` |
| [tools/gemini_video](../tools/gemini_video/_assets/icon.svg) | `langgenius/gemini` | 亮 + 暗 | `0.0.15` → `0.0.16` |
| [tools/general_chunk](../tools/general_chunk/_assets/icon.svg) | `rag/general_chunker` | 仅亮色 | `0.0.13` → `0.0.14` |
| [tools/gitee_ai](../tools/gitee_ai/_assets/icon.svg) | `langgenius/gitee_ai_tool` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [tools/github](../tools/github/_assets/icon.svg) | `langgenius/github` | 亮 + 暗 | `0.3.9` → `0.3.10` |
| [tools/gitlab](../tools/gitlab/_assets/icon.svg) | `rag_data_source/gitlab` | 亮 + 暗 | `0.0.13` → `0.0.14` |
| [tools/google](../tools/google/_assets/icon.svg) | `langgenius/google` | 亮 + 暗 | `0.1.6` → `0.1.7` |
| [tools/google_translate](../tools/google_translate/_assets/icon.svg) | `langgenius/google_translate` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [tools/gpustack](../tools/gpustack/_assets/icon.svg) | `langgenius/gpustack_tools` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [tools/hap](../tools/hap/_assets/icon.svg) | `langgenius/hap` | 等价共用 | `0.1.6` → `0.1.7` |
| [tools/hubspot](../tools/hubspot/_assets/icon.svg) | `langgenius/hubspot` | 逐像素共用 | `0.2.0` → `0.2.1` |
| [tools/jina](../tools/jina/_assets/icon.svg) | `langgenius/jina` | 亮 + 暗 | `0.0.13` → `0.0.14` |
| [tools/jira](../tools/jira/_assets/icon.svg) | `langgenius/jira` | 等价共用 | `0.0.9` → `0.0.10` |
| [tools/json_process](../tools/json_process/_assets/icon.svg) | `langgenius/json_process` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/judge0ce](../tools/judge0ce/_assets/icon.svg) | `langgenius/judge0ce` | 等价共用 | `0.0.8` → `0.0.9` |
| [tools/lark_base](../tools/lark_base/_assets/icon.svg) | `langgenius/lark_base` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/lark_calendar](../tools/lark_calendar/_assets/icon.svg) | `langgenius/lark_calendar` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/lark_document](../tools/lark_document/_assets/icon.svg) | `langgenius/lark_document` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/lark_message_and_group](../tools/lark_message_and_group/_assets/icon.svg) | `langgenius/lark_message_and_group` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/lark_spreadsheet](../tools/lark_spreadsheet/_assets/icon.svg) | `langgenius/lark_spreadsheet` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/lark_task](../tools/lark_task/_assets/icon.svg) | `langgenius/lark_task` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/lark_wiki](../tools/lark_wiki/_assets/icon.svg) | `langgenius/lark_wiki` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/linear](../tools/linear/_assets/icon.svg) | `langgenius/linear` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [tools/llama_parse](../tools/llama_parse/_assets/icon.svg) | `langgenius/llama_parse` | 等价共用 | `0.0.9` → `0.0.10` |
| [tools/mineru](../tools/mineru/_assets/icon.svg) | `langgenius/mineru` | 逐像素共用 | `0.5.7` → `0.5.8` |
| [tools/minimax_tts](../tools/minimax_tts/_assets/icon.svg) | `langgenius/minimax` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/monday](../tools/monday/_assets/icon.svg) | `langgenius/monday` | 亮 + 暗 | `0.0.6` → `0.0.7` |
| [tools/nextcloud](../tools/nextcloud/_assets/icon.svg) | `langgenius/nextcloud` | 等价共用 | `0.1.5` → `0.1.6` |
| [tools/nominatim](../tools/nominatim/_assets/icon.svg) | `langgenius/nominatim` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [tools/notion](../tools/notion/_assets/icon.svg) | `langgenius/notion` | 亮 + 暗 | `0.1.0` → `0.1.1` |
| [tools/novitaai](../tools/novitaai/_assets/icon.svg) | `langgenius/novitaai` | 逐像素共用 | `0.0.9` → `0.0.10` |
| [tools/onebot](../tools/onebot/_assets/icon.svg) | `langgenius/onebot` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [tools/onedrive](../tools/onedrive/_assets/icon.svg) | `rag_data_source/onedrive` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/openai](../tools/openai/_assets/icon.svg) | `langgenius/openai` | 亮 + 暗 | `0.1.10` → `0.1.11` |
| [tools/outlook](../tools/outlook/_assets/icon.svg) | `langgenius/outlook` | 亮 + 暗 | `0.5.0` → `0.5.1` |
| [tools/parent_child_chunk](../tools/parent_child_chunk/_assets/icon.svg) | `rag/parent_child_chunker` | 仅亮色 | `0.0.13` → `0.0.14` |
| [tools/perplexity](../tools/perplexity/_assets/icon.svg) | `langgenius/perplexity` | 逐像素共用 | `1.0.8` → `1.0.9` |
| [tools/podcast_generator](../tools/podcast_generator/_assets/icon.svg) | `langgenius/podcast_generator` | 逐像素共用 | `0.0.11` → `0.0.12` |
| [tools/pushover](../tools/pushover/_assets/icon.svg) | `langgenius/pushover` | 逐像素共用 | `0.0.6` → `0.0.7` |
| [tools/qa_chunk](../tools/qa_chunk/_assets/icon.svg) | `rag/qa_chunker` | 仅亮色 | `0.0.13` → `0.0.14` |
| [tools/qrcode](../tools/qrcode/_assets/icon.svg) | `langgenius/qrcode` | 逐像素共用 | `0.1.6` → `0.1.7` |
| [tools/rapidapi](../tools/rapidapi/_assets/icon.svg) | `langgenius/rapidapi` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [tools/regex](../tools/regex/_assets/icon.svg) | `langgenius/regex` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [tools/salesforce](../tools/salesforce/_assets/icon.svg) | `rag_data_source/salesforce` | 亮 + 暗 | `0.0.6` → `0.0.7` |
| [tools/searchapi](../tools/searchapi/_assets/icon.svg) | `langgenius/searchapi` | 等价共用 | `0.0.8` → `0.0.9` |
| [tools/searxng](../tools/searxng/_assets/icon.svg) | `langgenius/searxng` | 亮 + 暗 | `0.0.12` → `0.0.13` |
| [tools/siliconflow](../tools/siliconflow/_assets/icon.svg) | `langgenius/siliconflow` | 亮 + 暗 | `0.0.13` → `0.0.14` |
| [tools/slack](../tools/slack/_assets/icon.svg) | `langgenius/slack` | 亮 + 暗 | `0.4.0` → `0.4.1` |
| [tools/slidespeak](../tools/slidespeak/_assets/icon.svg) | `langgenius/slidespeak` | 亮 + 暗 | `1.0.5` → `1.0.6` |
| [tools/smartsheet](../tools/smartsheet/_assets/icon.svg) | `langgenius/smartsheet` | 逐像素共用 | `0.0.6` → `0.0.7` |
| [tools/spark](../tools/spark/_assets/icon.svg) | `langgenius/spark` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/spider](../tools/spider/_assets/icon.svg) | `langgenius/spider` | 亮 + 暗 | `0.0.9` → `0.0.10` |
| [tools/stability](../tools/stability/_assets/icon.svg) | `langgenius/stability` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/stablediffusion](../tools/stablediffusion/_assets/icon.svg) | `langgenius/stablediffusion` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/stackexchange](../tools/stackexchange/_assets/icon.svg) | `langgenius/stackexchange` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/stepfun](../tools/stepfun/_assets/icon.svg) | `langgenius/stepfun_tool` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [tools/supabase](../tools/supabase/_assets/icon.svg) | `langgenius/supabase` | 亮 + 暗 | `0.1.5` → `0.1.6` |
| [tools/tavily](../tools/tavily/_assets/icon.svg) | `langgenius/tavily` | 亮 + 暗 | `0.1.11` → `0.1.12` |
| [tools/teams](../tools/teams/_assets/icon.svg) | `rag_data_source/teams` | 亮 + 暗 | `0.0.1` → `0.0.2` |
| [tools/telegraph](../tools/telegraph/_assets/icon.svg) | `langgenius/telegraph` | 逐像素共用 | `0.0.10` → `0.0.11` |
| [tools/todoist](../tools/todoist/_assets/icon.svg) | `langgenius/todoist` | 等价共用 | `0.0.6` → `0.0.7` |
| [tools/transcript](../tools/transcript/_assets/icon.svg) | `langgenius/transcript` | 亮 + 暗 | `0.1.0` → `0.1.1` |
| [tools/trello](../tools/trello/_assets/icon.svg) | `langgenius/trello` | 逐像素共用 | `0.0.8` → `0.0.9` |
| [tools/twilio](../tools/twilio/_assets/icon.svg) | `langgenius/twilio` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [tools/vanna](../tools/vanna/_assets/icon.svg) | `langgenius/vanna` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [tools/vectorizer](../tools/vectorizer/_assets/icon.svg) | `langgenius/vectorizer` | 逐像素共用 | `0.0.7` → `0.0.8` |
| [tools/websearch](../tools/websearch/_assets/icon.svg) | `langgenius/websearch` | 等价共用 | `0.0.7` → `0.0.8` |
| [tools/wecom](../tools/wecom/_assets/icon.svg) | `langgenius/wecom` | 亮 + 暗 | `0.0.10` → `0.0.11` |
| [tools/wikipedia](../tools/wikipedia/_assets/icon.svg) | `langgenius/wikipedia` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [tools/wolframalpha](../tools/wolframalpha/_assets/icon.svg) | `langgenius/wolframalpha` | 亮 + 暗 | `0.0.9` → `0.0.10` |
| [tools/yahoo](../tools/yahoo/_assets/icon.svg) | `langgenius/yahoo` | 逐像素共用 | `0.0.10` → `0.0.11` |
| [tools/youtube](../tools/youtube/_assets/icon.svg) | `langgenius/youtube` | 亮 + 暗 | `0.0.7` → `0.0.8` |
| [tools/zhipuai](../tools/zhipuai/_assets/icon.svg) | `langgenius/zhipu` | 亮 + 暗 | `0.0.8` → `0.0.9` |
| [triggers/discord_trigger](../triggers/discord_trigger/_assets/icon.svg) | `langgenius/discord` | 等价共用 | `0.0.1` → `0.0.2` |
| [triggers/github_trigger](../triggers/github_trigger/_assets/icon.svg) | `langgenius/github` | 亮 + 暗 | `1.5.0` → `1.5.1` |
| [triggers/google_drive_trigger](../triggers/google_drive_trigger/_assets/icon.svg) | `rag_data_source/google_drive` | 亮 + 暗 | `1.4.0` → `1.4.1` |
| [triggers/lark_trigger](../triggers/lark_trigger/_assets/icon.svg) | `langgenius/feishu` | 亮 + 暗 | `0.1.0` → `0.1.1` |
| [triggers/linear_trigger](../triggers/linear_trigger/_assets/icon.svg) | `langgenius/linear` | 逐像素共用 | `0.6.0` → `0.6.1` |
| [triggers/notion_trigger](../triggers/notion_trigger/_assets/icon.svg) | `langgenius/notion` | 亮 + 暗 | `0.2.0` → `0.2.1` |
| [triggers/outlook_trigger](../triggers/outlook_trigger/_assets/icon.svg) | `langgenius/outlook` | 亮 + 暗 | `1.0.1` → `1.0.2` |
| [triggers/slack_trigger](../triggers/slack_trigger/_assets/icon.svg) | `langgenius/slack` | 亮 + 暗 | `0.3.0` → `0.3.1` |
| [triggers/twilio_trigger](../triggers/twilio_trigger/_assets/icon.svg) | `langgenius/twilio` | 逐像素共用 | `0.1.0` → `0.1.1` |
| [triggers/zendesk_trigger](../triggers/zendesk_trigger/_assets/icon.svg) | `rag_data_source/zend` | 逐像素共用 | `1.1.0` → `1.1.1` |

## 未更新插件

以下 56 个插件在设计包中没有可确认的同产品图标，保留现状，等待补齐素材。
`tools/chart` 另因源图案错误暂缓更新。
以上插件与下列六个暂缓插件均不包含在本次 200 个插件的图标、依赖或版本改动中。

| 分类 | 待补素材的插件 |
| --- | --- |
| `models` | `ant_ling`、`byteplus`、`cometapi`、`deerapi`、`funasr`、`gmicloud`、`groq`、`lemonade`、`longcat`、`mimo`、`openai_api_compatible`、`regolo`、`tokener` |
| `tools` | `attio`、`bailian_memory`、`deepl`、`dicom_reader`、`dingo`、`ernie_image`、`frontapp`、`gmail`、`google_calendar`、`google_contacts`、`google_tasks`、`hackernews`、`jiandaoyun`、`maths`、`microsoft_excel_365`、`microsoft_todo`、`neo4j`、`openweather`、`oracle_ai_db`、`paddleocr`、`plivo_sms`、`plivo_verify`、`pubmed`、`seltz`、`serper`、`snowflake`、`somark`、`spotify`、`sqlite`、`twitter`、`unstructured`、`whatsapp-bot`、`zoom` |
| `datasources` | `tencent_cos_storage` |
| `triggers` | `airtable_trigger`、`gmail_trigger`、`google_calendar_trigger`、`rsshub_trigger`、`telegram_trigger`、`typeform_trigger`、`woocommerce_trigger` |
| `agent-strategies` | `cot_agent`、`self_refine_agent` |

## 交叉复核与暂缓范围

三组独立复核比较了实际 SVG 渲染、原图、插件身份和 API 域名，并针对不确定图案查阅官方站点或品牌资源。
本次仅提交复核无异议的 200 个插件。
以下六个插件等待设计来源或品牌版本确认，整个插件目录暂缓提交。

| 插件 | 暂缓原因 |
| --- | --- |
| `models/perfxcloud` | 尚未找到能证明新版渐变 X 图案来源的官方素材。 |
| `models/sambanova` | 提供的橙色 S 属于历史品牌图案，与当前官方紫色标识不同。 |
| `tools/getimgai` | 新图案尚缺可确认的官方来源。 |
| `tools/nocodb` | 新版细线 N 与官方现有品牌资源不同，来源待确认。 |
| `tools/tianditu` | 折叠 M 图案与官方天地图标识不同，来源待确认。 |
| `tools/azure_openai_tool` | 新版 Azure AI 图案能否代表该具体产品仍待确认。 |

## 未落库的源素材

| 原因 | 源图标 |
| --- | --- |
| 源图案错误 | `langgenius/chart_generator` |
| 没有对应插件 | `langgenius/neko`、`langgenius/netmind`、`rag/docs_extractor`、`rag_data_source/servicenow` |
| 备用款式或旧产品命名 | `langgenius/dalle`、`langgenius/mistralai_pure`、`rag/aws_s3_classic`、`rag_data_source/aws_s3_classic` |
| 已选用其他分类的对应图标，避免重复引入 | `rag/aws_s3`、`rag/firecrawl`、`rag/google_cloud`、`rag/google_drive`、`rag/jina`、`rag/notion`、`rag_data_source/outlook`、`rag_data_source/slack`、`rag_data_source/supabase` |

原始 ZIP、预览图、导出 manifest 与迁移临时文件未加入插件资源。

## 依赖升级与兼容性

200 个插件共有 614 项运行依赖、38 项开发依赖和 1 项构建依赖声明，去重后为 119 个包。
这些声明的最低版本与复核时 PyPI 最新稳定版一致，运行和开发依赖的锁定版本也一致。
传递依赖由 `uv lock --upgrade` 在上游约束内更新。
Python 运行版本保持为 3.12，现存 Teams `requirements.txt` 从锁文件重新导出。

| 主要依赖 | 版本 |
| --- | --- |
| [dify-plugin](https://pypi.org/project/dify-plugin/) | `0.10.2` |
| [openai](https://pypi.org/project/openai/) | `3.8.0` |
| [anthropic](https://pypi.org/project/anthropic/) | `1.4.0` |
| [cohere](https://pypi.org/project/cohere/) | `7.1.1` |
| [google-cloud-aiplatform](https://pypi.org/project/google-cloud-aiplatform/) | `2.1.0` |
| [vanna](https://pypi.org/project/vanna/) | `2.0.2` |
| [atlassian-python-api](https://pypi.org/project/atlassian-python-api/) | `5.0.4` |
| [smartsheet-python-sdk](https://pypi.org/project/smartsheet-python-sdk/) | `4.4.0` |
| [monday-api-python-sdk](https://pypi.org/project/monday-api-python-sdk/) | `1.6.8` |

兼容性适配覆盖 Cohere 流事件类型、AiHubMix Anthropic 元数据、Vanna 客户端入口、Monday 写入结果、Baserow 行接口、Smartsheet 请求序列化，以及 Confluence / Jira / Dropbox 的现行 SDK 接口。
SageMaker 推理直接使用已依赖的 Boto3 Runtime，移除不再使用的 SageMaker 高层 SDK，保留流式与非流式响应、角色凭据和响应关闭行为。
Gemini Video 仅保留代码实际导入的四项依赖，删除 16 项无用直接依赖以消除版本约束冲突。
Moonshot 使用新版 Dify SDK 内置的视频序列化，避免重复插入视频内容。
Discord 触发器的公开验签密钥使用新版 SDK 支持的字符串配置类型。

## 验证

- 391 个源文件哈希与设计包 manifest 一致。
- 321 个落地 SVG 与选定源文件逐字节一致，XML、40×40 viewBox、内部引用及 Chromium 非空渲染检查通过。
- 520 处 provider / model / tool 图标引用检查通过，共用插件没有旧暗色文件残留。
- 200 / 200 锁文件通过 `uv lock --check --offline --python 3.12`。
- 200 / 200 插件在独立 Python 3.12 环境中完成冻结依赖安装及 `PluginRegistration` 启动检查。
- 模型与工具离线回归检查覆盖新版 SDK 请求、流事件、输出结构和错误传播；JSON 工具及 OpenAI 兼容扩展另外使用 Dify CLI 执行集成检查。
- 真实厂商服务调用及 Dify SaaS 部署没有执行，需要凭据的用例保持跳过。
- 200 / 200 插件使用 Dify CLI v0.6.10.4 从干净源码副本打包，包内图标字节及旧资源清理检查全部通过。
- 43 组现有回归检查共 1359 项通过，另有针对模型 SDK 迁移的 154 项、工具 SDK 迁移的 11 项及两个数据源检查通过。
- `git diff --check` 通过。
