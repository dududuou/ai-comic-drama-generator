# 03. 实施路线图

## Phase 1：剧本解析

目标：把一段文本剧本转换成结构化 scene 列表。

### 需要的模块

- `app/services/script_parser.py`
- `app/routes/script.py`
- `app/schemas.py`

### 交付成果

- 能输入剧本标题和正文
- 能解析出场景列表
- 能提取角色、地点、情绪、对话

## Phase 2：分镜生成

目标：根据 scene 生成多个镜头 shot。

### 需要模块

- `app/services/shot_generator.py`
- `app/templates/prompt_templates.py`

### 交付成果

- 每个 shot 包含：
  - shot_id
  - scene_id
  - description
  - duration
  - visual_prompt
  - camera_direction
  - emotion

## Phase 3：角色卡生成

目标：统一角色风格，保证人脸和服装一致。

### 需要模块

- `app/services/character_manager.py`

### 交付成果

- 角色统一设定
- 对应的 prompt 模板
- 角色声线信息

## Phase 4：画面生成

目标：为每个 shot 生成独立图像。

### 需要模块

- `app/services/image_generator.py`

### 交付成果

- 生成多张镜头图片
- 图片保存在 `data/images/`

## Phase 5：配音生成

目标：将每段对话生成对应音频。

### 需要模块

- `app/services/tts_service.py`

### 交付成果

- 每个对白都对应一个 mp3
- 角色音色与情绪尽量稳定

## Phase 6：视频合成

目标：把图片、音频、音乐和字幕合成成视频。

### 需要模块

- `app/services/music_service.py`
- `app/services/video_composer.py`

### 交付成果

- MP4 导出
- 最终视频可播放

## Phase 7：前端与控制台

目标：让用户能看到任务进度和结果链接。

### 可选模块

- React 前端
- 任务状态页
- 生成历史页

## Phase 8：稳定性优化

目标：减少错误，提高实际可用性。 

重点：

- 角色风格一致
- 画面风格统一
- 语音和字幕同步
- 镜头切换逻辑更顺

## 结论

先做 MVP，不要追求长篇作品。 一条“剧本到短视频”的链路跑通后，后续再慢慢扩展。
