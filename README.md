# AI Comic Drama Generator

## 项目概述

本项目是一个完整的 AI 漫剧生成系统，可将剧本自动转换为短视频。

**核心目标**：剧本输入 → 自动生成短视频输出

**技术特点**：
- 结构化剧本解析
- 智能分镜生成
- AI 画面生成
- 自动配音合成
- 视频自动剪辑

## 完整技术链路图

```
┌─────────────────┐
│   剧本输入      │
│  (TXT/JSON)    │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│  剧本解析 (LLM - Claude/GPT)             │
│  输出: scenes 列表                      │
│  包含: 地点、时间、角色、对话、情绪     │
└────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│  分镜生成 (LLM - Claude/GPT)             │
│  输出: shots 列表                       │
│  包含: 镜头描述、持续时间、提示词       │
└────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│  角色卡生成 (本地)                       │
│  统一视觉风格和语音特性                  │
└────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│  画面生成 (SDXL/Flux - Replicate API)   │
│  为每个 shot 生成对应图片               │
│  保存: data/images/                     │
└────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│  配音生成 (Azure TTS/Piper/OpenAI TTS)  │
│  为每段对话生成语音                      │
│  保存: data/audio/                      │
└────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────┐
│  视频合成 (FFmpeg/MoviePy)              │
│  合并图片、音频、BGM、字幕              │
│  输出: data/output/final_episode.mp4    │
└─────────────────────────────────────────┘
```

## 需要的外部项目和 API

### 后端框架
- **FastAPI** - 异步 Web 框架
  - GitHub: https://github.com/tiangolo/fastapi
  - 用途: API 服务器
  - 版本: 0.104.1+

- **SQLAlchemy** - ORM 数据库
  - GitHub: https://github.com/sqlalchemy/sqlalchemy
  - 用途: 数据模型管理
  - 版本: 2.0.23+

- **Alembic** - 数据库迁移
  - GitHub: https://github.com/sqlalchemy/alembic
  - 用途: 数据库版本管理
  - 版本: 1.12.1+

### 异步任务队列
- **Celery** - 分布式任务队列
  - GitHub: https://github.com/celery/celery
  - 用途: 后台异步任务处理
  - 版本: 5.3.4+
  - **依赖**: Redis

- **Redis** - 缓存和消息队列
  - 官网: https://redis.io/
  - 用途: Celery 消息代理、缓存
  - 版本: 6.0+（或 Docker 镜像）

### AI 和 LLM

#### 脚本解析 & 分镜生成
- **OpenAI Python SDK** - GPT API
  - GitHub: https://github.com/openai/openai-python
  - 用途: 剧本解析、分镜生成
  - 版本: 1.3.8+
  - 替代品: Anthropic SDK (Claude)

- **Anthropic Python SDK** - Claude API
  - GitHub: https://github.com/anthropics/anthropic-sdk-python
  - 用途: 高质量的剧本解析和分镜
  - 版本: 0.7.8+

#### 画面生成
- **Replicate Python SDK** - Replicate API 客户端
  - GitHub: https://github.com/replicate/replicate-python
  - 用途: 调用 SDXL/Flux 模型生成画面
  - 版本: 0.20.0+
  - 支持的模型:
    - SDXL (stability-ai/sdxl)
    - Flux (black-forest-labs/flux-schnell)
    - 其他 Stable Diffusion 变体

- **本地 Stable Diffusion** (可选)
  - GitHub: https://github.com/AUTOMATIC1111/stable-diffusion-webui
  - 用途: 本地离线画面生成
  - 方式: WebUI API 调用

### 语音合成 (TTS)

#### Azure 语音服务
- **Azure Cognitive Services Speech SDK**
  - 官方: https://github.com/Azure-Samples/cognitive-services-speech-sdk
  - pip: `azure-cognitiveservices-speech`
  - 版本: 1.39.0+
  - 用途: 高质量中文 TTS
  - 支持的音色: zh-CN-XiaoxiaoNeural 等

#### 本地 TTS (离线)
- **Piper TTS**
  - GitHub: https://github.com/rhasspy/piper
  - 用途: 本地离线语音合成
  - 版本: 1.2.0+
  - 优点: 完全离线、无成本、快速

- **Coqui TTS**
  - GitHub: https://github.com/coqui-ai/TTS
  - 用途: 本地开源 TTS
  - 版本: 最新版

#### OpenAI TTS (可选)
- **OpenAI Python SDK** (同上)
  - 用途: 高质量语音合成
  - 模型: tts-1 / tts-1-hd

### 视频处理

#### FFmpeg (核心)
- **FFmpeg** - 视频处理命令行工具
  - 官方: https://ffmpeg.org/
  - 用途: 视频合成、转码、编码
  - 必须安装到系统 PATH
  - 版本: 4.0+
  - 安装方式:
    ```bash
    # macOS
    brew install ffmpeg
    # Ubuntu/Debian
    sudo apt-get install ffmpeg
    # Windows: 下载并添加到 PATH
    ```

- **ffmpeg-python** - Python FFmpeg 包装
  - GitHub: https://github.com/kkroening/ffmpeg-python
  - 用途: Python 中调用 FFmpeg
  - 版本: 0.2.1+

#### MoviePy (Python 视频编辑)
- **MoviePy**
  - GitHub: https://github.com/Zulko/moviepy
  - 用途: Python 视频编辑、合成
  - 版本: 1.0.3+
  - 依赖: FFmpeg、ImageIO

#### 图像处理
- **Pillow** - Python 图像库
  - GitHub: https://github.com/python-pillow/Pillow
  - 用途: 图像处理、生成
  - 版本: 10.1.0+

- **ImageIO** - 图像和视频 I/O
  - GitHub: https://github.com/imageio/imageio
  - 用途: 读写多种格式的图像和视频
  - 版本: 2.33.1+

- **imageio-ffmpeg** - ImageIO 的 FFmpeg 支持
  - 版本: 1.4.1+

### 数据处理

- **Pandas** - 数据处理
  - 版本: 2.1.0+
  - 用途: 数据分析和处理

- **NumPy** - 数值计算
  - 版本: 1.26.0+
  - 用途: 数组处理

### 配置和工具

- **python-dotenv** - 环境变量管理
  - GitHub: https://github.com/theskumar/python-dotenv
  - 用途: 从 .env 文件加载配置
  - 版本: 1.0.0+

- **Pydantic** - 数据验证
  - GitHub: https://github.com/pydantic/pydantic
  - 用途: API 请求/响应数据模型
  - 版本: 2.5.0+

- **Requests** - HTTP 请求
  - GitHub: https://github.com/psf/requests
  - 用途: API 调用
  - 版本: 2.31.0+

### 前端 (可选)

- **React** - 前端框架
  - GitHub: https://github.com/facebook/react
  - 用途: 控制台 UI
  - 版本: 18+

- **Vite** - 前端构建工具
  - GitHub: https://github.com/vitejs/vite
  - 用途: React 项目构建
  - 版本: 5.0+

- **Ant Design** / **Element Plus** - UI 组件库
  - 用途: 快速搭建界面

### 测试和开发

- **Pytest** - 单元测试框架
  - 版本: 7.4.3+

- **pytest-asyncio** - Pytest 异步支持
  - 版本: 0.21.1+

## 依赖关系总结

### 必需的核心依赖
```
FastAPI → Uvicorn, Pydantic, SQLAlchemy
LLM 处理 → OpenAI SDK / Anthropic SDK
画面生成 → Replicate SDK (需要 API 调用)
配音 → Azure TTS SDK / Piper TTS
视频合成 → FFmpeg (系统工具) + MoviePy
数据库 → SQLAlchemy + SQLite/PostgreSQL
异步任务 → Celery + Redis
```

### 可选但推荐的依赖
```
本地 SD → Stable Diffusion WebUI
本地 TTS → Piper TTS / Coqui TTS
前端 → React + Vite + Ant Design
```

## 完整的 requirements.txt

```txt
# 核心框架
fastapi==0.104.1
uvicorn==0.24.0
pydantic==2.5.0
pydantic-settings==2.1.0

# 数据库
sqlalchemy==2.0.23
aiosqlite==0.19.0
alembic==1.12.1

# 异步任务
celery==5.3.4
redis==5.0.1

# LLM API
openai==1.3.8
anthropic==0.7.8
replicate==0.20.0

# 图像处理
pillow==10.1.0
imageio==2.33.1
imageio-ffmpeg==1.4.1
ffmpeg-python==0.2.1

# 视频处理
moviepy==1.0.3

# 语音处理
azure-cognitiveservices-speech==1.39.0
piper-tts==1.2.0

# 数据处理
pandas==2.1.3
numpy==1.26.2

# HTTP 和工具
requests==2.31.0
python-dotenv==1.0.0
python-multipart==0.0.6
httpx==0.25.2
aiofiles==23.2.1
PyYAML==6.0.1

# 测试
pytest==7.4.3
pytest-asyncio==0.21.1

# 开发工具 (可选)
black==23.12.0
flake8==6.1.0
```

## 项目目录结构

```text
ai-comic-drama-generator/
│
├── app/                                    # 主应用代码
│   ├── __init__.py
│   ├── main.py                            # FastAPI 主入口
│   ├── config.py                          # 配置文件
│   ├── database.py                        # 数据库初始化
│   ├── models.py                          # SQLAlchemy 模型
│   ├── schemas.py                         # Pydantic 数据模型
│   ├── utils.py                           # 工具函数
│   │
│   ├── routes/                            # API 路由
│   │   ├── __init__.py
│   │   ├── script.py                      # 剧本上传/查询
│   │   ├── generation.py                  # 分镜/漫剧生成
│   │   └── task.py                        # 任务状态查询
│   │
│   └── services/                          # 业务逻辑层
│       ├── __init__.py
│       ├── script_parser.py               # 剧本解析 (LLM)
│       ├── shot_generator.py              # 分镜生成 (LLM)
│       ├── character_manager.py           # 角色卡统一
│       ├── image_generator.py             # 画面生成 (SDXL)
│       ├── tts_service.py                 # 配音生成 (TTS)
│       ├── music_service.py               # BGM 和音效
│       └── video_composer.py              # 视频合成 (FFmpeg)
│
├── frontend/                              # 前端代码 (可选)
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Upload.jsx
│   │   │   ├── Editor.jsx
│   │   │   └── Preview.jsx
│   │   ├── components/
│   │   ├── services/
│   │   └── App.jsx
│   ├── package.json
│   └── .env.example
│
├── data/                                  # 数据目录 (生成)
│   ├── scripts/                           # 上传的剧本
│   ├── shots/                             # 分镜数据
│   ├── images/                            # 生成的图片
│   ├── audio/                             # 生成的音频
│   ├── music/                             # BGM 文件
│   ├── output/                            # 输出视频
│   └── app.db                             # SQLite 数据库
│
├── docs/                                  # 文档
│   ├── 01-project-overview.md
│   ├── 02-open-source-stack.md
│   ├── 03-implementation-roadmap.md
│   ├── 04-cursor-usage.md
│   └── 05-mvp-checklist.md
│
├── .env.example                           # 环境变量示例
├── requirements.txt                       # Python 依赖
├── run.py                                 # 启动脚本
├── README.md                              # 项目说明 (本文件)
└── .gitignore
```

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/dududuou/ai-comic-drama-generator.git
cd ai-comic-drama-generator
```

### 2. 创建虚拟环境

```bash
python -m venv venv
source venv/bin/activate  # macOS/Linux
# 或
venv\Scripts\activate     # Windows
```

### 3. 安装依赖

```bash
pip install -r requirements.txt
```

### 4. 配置环境变量

```bash
cp .env.example .env
```

编辑 `.env`：

```env
APP_ENV=dev
APP_PORT=8000

# LLM API Keys
OPENAI_API_KEY=your_openai_key
ANTHROPIC_API_KEY=your_anthropic_key

# 画面生成
REPLICATE_API_TOKEN=your_replicate_token

# 语音合成
AZURE_TTS_KEY=your_azure_key
AZURE_TTS_REGION=eastus

# 数据库
DATABASE_URL=sqlite:///./data/app.db
REDIS_URL=redis://localhost:6379
```

### 5. 安装 FFmpeg (重要)

```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt-get install ffmpeg

# Windows: 从 https://ffmpeg.org/download.html 下载
```

### 6. 启动 Redis (可选但推荐)

```bash
redis-server
```

### 7. 启动应用

```bash
python run.py
```

访问：`http://localhost:8000/docs`

## API 使用示例

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

### 3. 生成完整漫剧

```bash
curl -X POST http://localhost:8000/api/generate/comic/1
```

### 4. 查询任务状态

```bash
curl http://localhost:8000/api/tasks/1
```

## 参考项目和灵感来源

- **shuohao-skills** - 剧本/分镜/角色设定思路
  - https://github.com/eternityspring/shuohao-skills

- **Stable Diffusion WebUI** - 本地画面生成参考
  - https://github.com/AUTOMATIC1111/stable-diffusion-webui

## 核心开发步骤

### Phase 1: 基础框架 (1-3 天)
- [ ] 配置 FastAPI 和数据库
- [ ] 实现 `script_parser.py` - 剧本解析
- [ ] 实现 `script.py` 路由 - 上传接口
- [ ] 测试剧本上传和解析

### Phase 2: 分镜生成 (3-5 天)
- [ ] 实现 `shot_generator.py` - 分镜生成
- [ ] 实现 `generation.py` 路由 - 分镜接口
- [ ] 测试分镜返回数据结构

### Phase 3: 角色和画面 (3-7 天)
- [ ] 实现 `character_manager.py` - 角色卡
- [ ] 实现 `image_generator.py` - 画面生成
- [ ] 接入 Replicate SDXL API
- [ ] 测试图片生成

### Phase 4: 配音和合成 (2-4 天)
- [ ] 实现 `tts_service.py` - TTS 配音
- [ ] 实现 `video_composer.py` - 视频合成
- [ ] 接入 Azure TTS / Piper TTS
- [ ] 接入 FFmpeg 视频合成

### Phase 5: 任务管理和前端 (2-4 天)
- [ ] 实现 `task.py` 路由 - 任务状态
- [ ] 实现异步任务系统 (Celery)
- [ ] 可选: 实现 React 前端控制台

### Phase 6: 优化和扩展 (1-2 周)
- [ ] 角色一致性优化
- [ ] 字幕自动生成
- [ ] BGM 选择和集成
- [ ] 多集管理支持

## 最小可行版 (MVP) 目标

✅ 用户可以上传剧本  
✅ 系统自动解析为 scenes  
✅ 系统自动生成 shots  
✅ 系统自动生成图片  
✅ 系统自动生成配音  
✅ 系统自动合成视频  
✅ 最终输出 MP4 可播放  

## 关键原则

1. **先跑通，再优化** - 完成最小闭环是首要目标
2. **模块化设计** - 每个服务独立，便于迭代和替换
3. **循序渐进** - 逐步接入真实 API，而不是一开始就完全依赖
4. **短片优先** - 先做 2-5 分钟 MVP，再扩展到长篇
5. **API 设计清晰** - 便于前后端协作和第三方集成

## 常见问题排查

### 问题 1: FFmpeg 找不到
```bash
# 确保 FFmpeg 已安装
ffmpeg -version
```

### 问题 2: 数据库初始化失败
```bash
# 删除旧数据库并重新初始化
rm data/app.db
python run.py
```

### 问题 3: API 调用失败
- 检查 .env 中的 API Key 是否正确
- 检查网络连接
- 检查 API 配额是否充足

## 贡献和反馈

欢迎提交 Issue 和 Pull Request。

## 许可证

MIT License
