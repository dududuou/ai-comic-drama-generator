# AI Comic Drama Generator

## 项目目标

本项目目标是：

- 输入一份剧本
- 自动解析成结构化剧情
- 生成镜头分镜
- 生成每个镜头的画面
- 生成角色配音
- 自动合成成完整 AI 漫剧短视频
- 导出 MP4

## 适用场景

- 小说/剧本转短视频
- AI 漫剧脚本生成
- AI 角色动画演示
- 自动视频配音与剪辑

## 核心技术链路

```text
剧本输入
  ↓
剧本解析（LLM）
  ↓
分镜生成（LLM）
  ↓
角色卡（Role Cards）
  ↓
图像生成（SDXL / Flux / Stable Diffusion）
  ↓
TTS 配音（Azure TTS / Piper TTS / OpenAI TTS）
  ↓
BGM + 音效
  ↓
视频合成（FFmpeg + MoviePy）
  ↓
输出 MP4
```

## 参考项目

- shuohao-skills：用于小说/短剧的分镜、角色设定和内容分解思路
  - https://github.com/eternityspring/shuohao-skills
- Stable Diffusion WebUI：本地画面生成
  - https://github.com/AUTOMATIC1111/stable-diffusion-webui
- Piper TTS：本地离线语音
  - https://github.com/rhasspy/piper
- FFmpeg：视频合成与转码
  - https://ffmpeg.org/
- MoviePy：Python 视频处理
  - https://github.com/Zulko/moviepy
- FastAPI：API 服务
  - https://github.com/tiangolo/fastapi
- Celery：异步任务队列
  - https://github.com/celery/celery

## 推荐技术栈（MVP）

- 后端：FastAPI + SQLAlchemy + Celery + Redis
- 数据库：SQLite（开发） / PostgreSQL（生产）
- AI：Claude / GPT / OpenAI SDK
- 图像生成：SDXL via Replicate / 本地 SD WebUI
- 配音：Azure TTS / Piper TTS
- 视频合成：FFmpeg + MoviePy
- 前端：React（可选）

## 代码骨架

```text
ai-comic-drama-generator/
├── app/
│   ├── main.py
│   ├── config.py
│   ├── database.py
│   ├── models.py
│   ├── schemas.py
│   ├── routes/
│   │   ├── script.py
│   │   ├── generation.py
│   │   └── task.py
│   └── services/
│       ├── script_parser.py
│       ├── shot_generator.py
│       ├── character_manager.py
│       ├── image_generator.py
│       ├── tts_service.py
│       ├── music_service.py
│       └── video_composer.py
├── data/
├── docs/
│   ├─��� 01-project-overview.md
│   ├── 02-open-source-stack.md
│   ├── 03-implementation-roadmap.md
│   ├── 04-cursor-usage.md
│   └── 05-mvp-checklist.md
├── .env.example
├── requirements.txt
├── run.py
├── README.md
└── .gitignore
```

## MVP 实施路线

### Phase 1：剧本解析（1-3 天）
- 剧本上传
- 解析 scenes
- 提取角色、地点、对话

### Phase 2：分镜生成（3-5 天）
- 为每个 scene 生成 shot
- 输出镜头描述和 prompt

### Phase 3：图像生成（3-7 天）
- 为每个 shot 生成图片
- 保存到 `data/images/`

### Phase 4：语音生成（2-4 天）
- 为每段对白生成 TTS
- 保存到 `data/audio/`

### Phase 5：视频合成（2-4 天）
- 合成 MP4
- 输出到 `data/output/`

### Phase 6：优化（1-2 周）
- 角色一致性
- 镜头风格统一
- 自动字幕
- BGM 和音效

## 使用方式（Cursor / 本地）

```bash
git clone https://github.com/dududuou/ai-comic-drama-generator.git
cd ai-comic-drama-generator
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python run.py
```

访问：

```text
http://localhost:8000/docs
```

## 测试接口

### 1. 上传剧本

```bash
curl -X POST http://localhost:8000/api/scripts/upload \
  -H "Content-Type: application/json" \
  -d '{
    "title": "星河回响",
    "content": "林岚站在城市天台，顾沉缓缓出现。雨夜的灯光映在两个人的脸上。林岚说：你到底来不来？顾沉说：我来了。"
  }'
```

### 2. 生成分镜

```bash
curl -X POST http://localhost:8000/api/generate/shots/1
```

### 3. 生成整部漫剧

```bash
curl -X POST http://localhost:8000/api/generate/comic/1
```

### 4. 查询任务状态

```bash
curl http://localhost:8000/api/tasks/1
```

## 关键原则

- 先做能跑，不要一开始追求完美
- 先跑通最小闭环：剧本 -> scenes -> shots -> video
- 不要追求“一次生成整个长篇电影”
- 先做 2~5 分钟短片 MVP，再扩展功能

## 参考价值

`shuohao-skills` 最重要的价值在于：

- 角色设定拆解
- 剧情结构化
- 分镜与质量门思路

但它不是一个“直接把剧本做成视频”的系统。所以本项目在设计上需要把：

- LLM 结构化
- 图像生成
- 配音
- 视频合成

全部组合起来，才能构成真正的 AI 漫剧生成器。

## 结论

你现在的目标不是“一个模型直接输出整个漫剧”，而是构建一条可运行、可扩展的自动化链路。只要这条链路跑通，AI 漫剧生成器就已经成功落地。
