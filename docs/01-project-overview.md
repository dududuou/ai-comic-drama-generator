# AI 漫剧生成项目的总体路线

## 1. 项目定位

本项目不追求一次性直接生成一部超长电影，而是构建一条从剧本到视频的自动化流程：

剧本 -> 分镜 -> 角色 -> 画面 -> 配音 -> 音乐 -> 合成 -> MP4

这是一条最适合做 MVP 的路线。

## 2. shuohao-skills 的启发

`shuohao-skills` 是一个真实存在的项目，GitHub 地址：

https://github.com/eternityspring/shuohao-skills

它的价值在于：

- 角色卡生成
- 小说/剧情拆解
- 分镜和剧情结构化
- 质量门控制

它不是直接生成完整视频，但其思路对我们非常有用：

- 把内容拆成结构化场景
- 为每个角色建立角色卡
- 为每个镜头建立提示词和画面要求
- 对输出增加结构化质量约束

这正是我们需要的。

## 3. 现实项目路线

### 最小可行版（推荐）

- 使用 FastAPI 做后端
- 使用 Claude / GPT 做剧本解析和分镜
- 使用 SDXL / Flux / Stable Diffusion 做镜头图像
- 使用 Azure TTS 或 Piper TTS 做配音
- 使用 FFmpeg + MoviePy 合成最终视频
- 跑通一条短篇 2-5 分钟 demo

### 关键目标

- 用户输入一段剧本
- 系统自动分镜
- 自动生成角色/画面
- 自动配音
- 自动输出视频

## 4. 必要的开源项目

### 必须掌握的

- FastAPI
- SQLAlchemy
- Celery
- Redis
- FFmpeg
- MoviePy
- Pillow
- OpenAI / Anthropic SDK
- Stable Diffusion / SDXL

### 可选增强

- Piper TTS
- Coqui TTS
- Replicate
- Azure TTS
- React 前端

## 5. 你需要如何使用 Cursor

建议开发顺序：

1. 阅读 README.md
2. 阅读 docs/02-open-source-stack.md
3. 阅读 docs/03-implementation-roadmap.md
4. 让 Cursor 生成项目结构
5. 先写 `app/services/script_parser.py`
6. 再写 `shot_generator.py`
7. 再写 `image_generator.py`
8. 再写 `tts_service.py`
9. 再写 `video_composer.py`
10. 最后接前端或接口测试

## 6. 关键原则

不要一开始就追求全自动长篇大片。先跑通：

- 1份剧本
- 5 个镜头
- 1 个角色
- 1 个短片 demo

这是最真实、最稳的路径。

## 7. 结论

你需要的不是一个单独的 `shuohao-skill`，而是：

- 结构化剧本输出
- 分镜生成
- 角色一致性
- 图像生成
- 配音与合成

这些能力组合起来，才是“AI 漫剧生成器”。
