# 02. 必需的开源项目与技术搭配

## 1. 最核心的技术栈

### 后端

- FastAPI
- SQLAlchemy
- Alembic
- Celery
- Redis

### 图像生成

- Stable Diffusion WebUI
- diffusers
- SDXL
- Replicate SDK

### 语音

- Azure TTS
- Piper TTS
- Coqui TTS
- OpenAI TTS

### 视频处理

- FFmpeg
- MoviePy
- Pillow
- ImageIO

### 前端（可选）

- React
- Vite
- Ant Design / Element Plus

## 2. 推荐组合

### 方案 A：轻量版（适合个人）

- FastAPI
- SQLite
- Replicate 生成图片
- Azure TTS
- FFmpeg + MoviePy

优点：快速启动，成本低。 缺点：不适合复杂生产。

### 方案 B：推荐方案（平衡）

- FastAPI
- Celery + Redis
- Claude/GPT 分析剧本
- SDXL 画面生成
- Azure TTS / Piper TTS
- FFmpeg + MoviePy

优点：结构更完整，适合做产品原型。 缺点：需要更多集成工作。

### 方案 C：完整产品版

- 方案 B 全部能力
- 本地 SDWebUI
- 人物角色库
- 多集管理
- 可视化编排器

优点：更强大。 缺点：开发周期更长。

## 3. 推荐直接学习的项目

### 1) shuohao-skills

GitHub：
https://github.com/eternityspring/shuohao-skills

价值：
- 角色设定
- 结构化分解
- 故事拆解
- 分镜质量控制

### 2) Stable Diffusion WebUI

GitHub：
https://github.com/AUTOMATIC1111/stable-diffusion-webui

价值：
- 本地生成画面
- 支持高级控制
- 可以做定制风格

### 3) Piper TTS

GitHub：
https://github.com/rhasspy/piper

价值：
- 本地离线配音
- 适合本地开发和高稳定性

### 4) FFmpeg

链接：https://ffmpeg.org/

价值：
- 视频拼接
- 转码
- 音频同步
- 字幕处理

### 5) MoviePy

GitHub：
https://github.com/Zulko/moviepy

价值：
- Python 里做视频编辑
- 易于嵌入 FastAPI 流程

## 4. 实际落地建议

对本项目来说，最现实的路线不是“纯本地部署”，而是：

- 结构化任务：LLM API
- 画面生成：SDXL / Replicate
- 语音：Azure TTS / Piper
- 合成：FFmpeg + MoviePy

这样最稳，成本也更可控。
