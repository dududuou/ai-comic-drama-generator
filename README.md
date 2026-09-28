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

```
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
│   ├── utils.py
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
│   ├── 01-project-overview.md
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

## 时序目标

### Phase 1：MVP 原型（1-2 周）
- 剧本上传
- 解析为 scenes
- 生成 3-5 个镜头
- 生成图片
- 生成 TTS
- 合成简单视频

### Phase 2：可用版本（2-4 周）
- 角色卡统一
- 更稳定风格
- 音乐与字幕
- 任务队列
- 进度状态

### Phase 3：产品级（4-8 周）
- 可视化编辑器
- 多集管理
- 用户系统
- 人工微调

## 使用方式（Cursor）

```bash
git clone https://github.com/dududuou/ai-comic-drama-generator.git
cd ai-comic-drama-generator
pip install -r requirements.txt
cp .env.example .env
python run.py
```

## 学习建议

- 先读 `docs/01-project-overview.md`
- 再读 `docs/02-open-source-stack.md`
- 再读 `docs/03-implementation-roadmap.md`
- 最后直接开始写代码

## 适合 Cursor 的开发方式

建议：

1. 让 Cursor 先阅读 README 和 docs
2. 让它基于骨架开始生成 `script_parser.py` 和 `shot_generator.py`
3. 再逐步接入 `image_generator.py` / `tts_service.py` / `video_composer.py`
4. 最后补前端或 API 调试

## 备注

这个项目的目标不是“一个黑盒模型直接生成整个漫剧”，而是构建一条稳定的自动化链路。只要流程和依赖清晰，Cursor 很适合作为协作开发工具。
