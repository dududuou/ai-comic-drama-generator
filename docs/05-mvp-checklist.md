# 04. Cursor 使用建议

## 1. 适合 Cursor 的开发方式

Cursor 适合做这类项目，因为它可以：

- 读代码结构
- 自动补全模块
- 生成 Python 的 API / 服务代码
- 批量协作实现多个模块

## 2. 推荐开发顺序

### 第一步：先看文档

阅读顺序：

1. README.md
2. docs/01-project-overview.md
3. docs/02-open-source-stack.md
4. docs/03-implementation-roadmap.md

### 第二步：先写骨架

让 Cursor 先生成以下文件：

- `app/main.py`
- `app/config.py`
- `app/database.py`
- `app/models.py`
- `app/schemas.py`

### 第三步：写核心服务

然后让 Cursor 按模块生成：

- `app/services/script_parser.py`
- `app/services/shot_generator.py`
- `app/services/character_manager.py`
- `app/services/image_generator.py`
- `app/services/tts_service.py`
- `app/services/video_composer.py`

### 第四步：接真实 API

把以下模块替换成真实能力：

- Claude / GPT 处理脚本解析
- Replicate / SDXL 生成图像
- Azure TTS / Piper TTS 生成语音
- FFmpeg / MoviePy 合成视频

## 3. 实用提示词示例

你可以直接让 Cursor 按这种方式开发：

```text
请基于这个 FastAPI 骨架，补全脚本解析模块，并且使用 Pydantic 定义剧本结构。
```

```text
请实现一个 shot_generator.py，输出 scene -> shots 的结构化列表。
```

```text
请实现一个 image_generator.py，用 SDXL 或 Replicate 生成镜头图像，并返回文件路径。
```

```text
请实现一个 tts_service.py，支持 Azure TTS 和 Piper TTS，并返回 MP3 文件路径。
```

```text
请实现一个 video_composer.py，使用 FFmpeg 或 MoviePy 将图片和音频合成成最终视频。
```

## 4. 推荐的协作方式

- 先让 Cursor 生成结构化代码
- 再手工检查逻辑是否合理
- 最后补真实调用
- 做一个最小 demo 先跑通，再扩充功能

## 5. 结论

Cursor 非常适合这种 AI 生成器项目，因为它可以按模块开发并且迭代很快。
