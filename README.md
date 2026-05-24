# sound-director-skill

将用户的一句话故事创意转化为专业的声音设计方案。纯提示词驱动，输出涵盖声音世界观、配音声线、环境音景、拟音清单、声音转场、配乐方向、动态混音策略及执行建议，专为AI漫剧和有声影像优化。

## 推荐模型与 API

本技能为纯提示词驱动，不绑定特定模型。以下为经过实测的推荐方案，按"易落地性"排序：

### 🥇 首选：DeepSeek-V3（性价比最高）

| 项目 | 说明 |
|------|------|
| API 地址 | `https://api.deepseek.com/v1` |
| 模型名 | `deepseek-chat` |
| 价格 | 输入 ¥1/百万token，输出 ¥2/百万token |
| 优势 | 国内直连无需代理，中文声音描述细腻精准 |
| 注册 | [https://platform.deepseek.com](https://platform.deepseek.com) |

```python
from openai import OpenAI

client = OpenAI(
    api_key="your-deepseek-api-key",
    base_url="https://api.deepseek.com/v1"
)

response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": SYSTEM_PROMPT},
        {"role": "user", "content": "你的故事创意"}
    ],
    temperature=0.8,
    max_tokens=8000
)
```

### 🥈 备选：硅基流动 SiliconFlow（国内最便捷）

| 项目 | 说明 |
|------|------|
| API 地址 | `https://api.siliconflow.cn/v1` |
| 模型名 | `deepseek-ai/DeepSeek-V3` |
| 价格 | 新用户赠送额度，约 ¥0.5/百万token |
| 优势 | 一个 API Key 可切换多模型，国内直连 |
| 注册 | [https://siliconflow.cn](https://siliconflow.cn) |

### 🥉 品质首选：GPT-4o（结构化输出最可靠）

| 项目 | 说明 |
|------|------|
| API 地址 | `https://api.openai.com/v1` |
| 模型名 | `gpt-4o` |
| 价格 | 输入 $2.5/百万token，输出 $10/百万token |
| 优势 | 音效分类和格式最稳定，适合预算充足的生产环境 |
| 注意 | 国内需代理访问 |

### 其他可用模型

- **智谱 GLM-4-Plus**：`https://open.bigmodel.cn/api/paas/v4`，中文理解力强
- **Qwen2.5-72B**：通过硅基流动或阿里云 DashScope 调用
- **Claude 3.5 Sonnet**：创意质量极高，但国内访问不便

## 快速开始

### 方式一：直接复制提示词（最快）

打开 [skill.yaml](./skill.yaml)，找到 `system_prompt` 字段，将完整内容复制到任意 LLM 平台的 System Prompt 设置中，将一句话创意作为用户消息发送即可。

### 方式二：在 Dify 中导入

1. 创建 Chatflow 应用，添加 LLM 节点
2. System Prompt 粘贴 `system_prompt` 完整内容
3. 模型参数：Temperature `0.8`，Max Tokens `8000`
4. 添加开始节点，定义输入变量 `user_idea`
5. LLM 节点用户消息引用 `{{user_idea}}`
6. 发布应用

### 方式三：在 Coze 中导入

1. 创建新 Bot，在"人设与回复逻辑"中粘贴 `system_prompt`
2. 设置 Temperature 为 0.8
3. 将一句话创意直接发送给 Bot

### 方式四：Python 脚本调用

```python
import yaml
from openai import OpenAI

with open("skill.yaml", "r", encoding="utf-8") as f:
    skill = yaml.safe_load(f)

client = OpenAI(
    api_key="your-api-key",
    base_url="https://api.deepseek.com/v1"
)

result = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": skill["system_prompt"]},
        {"role": "user", "content": "一个落魄钢琴手在地下停车场听见一段神秘旋律，追踪后发现弹奏者是30年前的自己。"}
    ],
    temperature=skill["runtime"]["temperature"],
    max_tokens=skill["runtime"]["max_tokens"]
)

print(result.choices[0].message.content)
```

## 输入示例

```
一个落魄钢琴手在地下停车场听见一段神秘旋律，追踪后发现弹奏者是30年前的自己。
```

## 输出示例

完整输出示例请查看 [examples/output_01.md](./examples/output_01.md)，包含以下结构化段落：

- **【声音世界观总纲】** — 声音美学、空间感、动态范围、声音母题
- **【配音与角色声线指导】** — 音域/音色/咬字/气息/表演方向/声音弧光
- **【环境音景设计】** — 混响特征/三层架构/情绪功能/叙事变化
- **【关键音效与拟音清单】** — 12-18个音效点，含物理特性和AI漫剧适配
- **【声音转场设计】** — 转场类型/衔接逻辑/静音使用
- **【配乐风格与节奏设计】** — 风格/参考/节奏策略/主题旋律
- **【动态范围与混音策略】** — 频段分配/对白处理/空间混音/响度标准
- **【执行与资源建议】** — 资源评估/配音建议/AI漫剧适配/三档预算方案

## 与其他 Skill 配合使用

本技能包与以下 Skill 天然互补，形成完整的影视前期设计链：

1. **[screenwriter-skill](https://github.com/small-bluesky/screenwriter-skill)**：一句话→完整剧本
2. **[art-director-skill](https://github.com/small-bluesky/art-director-skill)**：一句话→美术设计视觉方案
3. **[storyboard-artist-skill](https://github.com/small-bluesky/storyboard-artist-skill)**：一句话→分镜脚本
4. **sound-director-skill**（本技能）：一句话→声音设计方案

推荐工作流：先用 **screenwriter** 生成剧本 → **art-director** 确定视觉风格 → **storyboard-artist** 设计镜头 → **sound-director** 设计声音，四者交叉对照形成完整的"剧本+美术+分镜+声音"前期包。

## 高级用法

### 调整运行时参数

- **temperature**：`0.7-0.9` 适合创意发散。声音设计需要创意但也需要技术精确，如需更稳定的格式，可降至 `0.6`
- **max_tokens**：本技能输出量较大（8个段落），建议 ≥ `8000`
- **stream**：默认关闭。流式输出适合实时展示生成过程

### 与 RAG 结合

在 LLM 调用前增加知识库检索节点，注入以下参考资料：
- 经典电影的声音设计分析（如《现代启示录》的声音层叠、《社交网络》的电子配乐）
- 音效素材库的分类目录和检索关键词
- 特定类型（悬疑/科幻/古装）的声音设计惯例

### 接入 AI 语音合成

`extensions.ai_voice` 预留了 AI 语音合成接口。未来工作流可扩展为：
1. LLM 生成配音指导文本
2. 提取每个角色的声线参数
3. 自动调用 Azure TTS / ElevenLabs / ChatTTS 生成语音样本

### 接入 AI 音效生成

`extensions.ai_sfx` 预留了 AI 音效生成接口。未来工作流可扩展为：
1. LLM 生成音效描述
2. 提取物理材质和声学参数
3. 自动调用 AudioCraft / Stable Diffusion Audio 生成音效素材

## 常见问题

### 为什么新增了「声音转场设计」和「动态范围与混音策略」？

初版缺少这两个维度。声音转场是声音导演的核心工作——好的声音转场比画面转场更能引导观众的情绪。动态范围与混音策略则为后期制作提供了明确的技术标准，避免"前期设计很棒但混音时全乱了"的问题。

### 这个 Skill 适合 AI 漫剧还是传统影视？

两者都适合，但特别为 AI 漫剧做了优化。AI 漫剧（静态画面+声音）中，声音承担的信息量远大于传统影视，因此每个音效都标注了"AI漫剧适配"——说明哪些可以从素材库获取、哪些需要定制、哪些可以用 AI 生成。

### 推荐用 DeepSeek 还是 GPT-4o？

- **日常使用、个人项目、预算有限**：DeepSeek-V3，性价比极高
- **商业项目、需要最稳定的音效分类和格式**：GPT-4o，结构化输出最可靠
- **国内快速上手**：硅基流动，注册即用

## 项目结构

```
sound-director-skill/
├── README.md
├── skill.yaml
├── examples/
│   ├── input_01.txt
│   └── output_01.md
├── assets/
│   └── workflow-diagram.png
├── LICENSE
└── .gitignore
```

## 贡献指南

欢迎通过 Issue 或 PR 参与贡献：

- 提示词优化，提升特定类型（悬疑/科幻/古装/爱情等）的声音设计质量
- 提供更多示例输入输出
- 适配更多 Agent 平台的导入方案
- 补充 AI 语音合成和 AI 音效生成的实测效果

请确保所有提交使用 **UTF-8 without BOM** 编码，换行符为 **LF**。

## 许可证

本项目基于 MIT 许可证开源，详见 [LICENSE](./LICENSE) 文件。