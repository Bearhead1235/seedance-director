<p align="center">
  <a href="README.md">English</a> | <a href="README_zh.md">中文</a>
</p>

# seedance-director

> 用即梦做视频，提示词不知道怎么写？交给你的 AI 导演。

一个 [Agent Skill](https://agentskills.io)，在 Claude Code 里帮你走完从创意到即梦提示词的全流程。

## 安装

```bash
npx skills add https://github.com/crowscc/seedance-director
```

安装后直接聊你的视频创意。提到「即梦」「Seedance」「分镜」「运镜」「视频延长」等关键词时自动激活。

## v2 改了什么

| | v1 | v2 |
|---|---|---|
| 素材 | 假设用户自备 | 帮你生成角色三视图、场景概念图、关键帧 |
| 一致性 | 靠一张参考图碰运气 | 三视图 + 场景图锚定每一段 |
| 长视频 | 全用视频延长串联（越延越歪） | 3 段以上改为独立生成 + 首帧衔接 |
| 交互 | 纯文本问答 | `AskUserQuestion` 结构化选择，选项根据创意动态生成 |
| 架构 | 635 行大文件 | 262 行核心 + 按需加载的参考文件 |

## 工作流演示

```
你：我想做一个 30 秒的咖啡广告，电影感，暖色调，有 3 张产品图。

导演：[AskUserQuestion]
      哪种叙事结构最适合你的创意？
      > 过程展示 — 咖啡豆到杯中的旅程，契合你的产品故事 ✓
      > 对比型 — Before/After，适合口感反差
      > 自定义 — 描述你想要的结构

你：过程展示

导演：[AskUserQuestion]
      检测到你缺少场景参考图，要帮你生成吗？
      > 帮我生成场景概念图
      > 不需要，我自己准备
      > 跳过，纯文本生成

你：帮我生成

导演：
  1. 场景概念图提示词（粘贴到图片生成器）：
     "日出时分的咖啡庄园，金色光线，薄雾升腾..."

  2. 分镜脚本（2 段，16:9，电影暖调）：
     001 | 0-4s  | 微距特写    | 缓推 Dolly In | 咖啡豆纹理，金色逆光
     002 | 4-8s  | 近景 CU     | 俯拍 Tilt    | 豆子倒入研磨机
     003 | 8-12s | 中景 MS     | 固定 Static  | 手冲，蒸汽升腾
     ...

  3. 第 1 段即梦提示词（直接粘贴）：
     宽高比16:9，时长15秒，电影质感...
     @图片1 中的咖啡豆在晨光中缓缓滚动...

  4. 操作指引：
     上传产品图 → 设置 15s、16:9 → 生成
     → 截取末帧 → 作为第 2 段的首帧参考 → 生成第 2 段
```

## 多段策略

| 时长 | 策略 | 原因 |
|------|------|------|
| ≤15s | 一次生成 | 平台单次上限 |
| 16-30s | 段1 + 视频延长 | 延长的最佳区间，衔接自然 |
| 31s+ | 每段独立生成 | 每段引用三视图 + 场景图 + 上段末帧，避免错误传递和风格漂移。各段可独立重试，最后在剪映拼接。 |

## 素材制备（Phase 0）

生成视频之前，导演先检查你有什么素材，缺什么帮你补：

| 素材 | 用途 | 生成方式 |
|------|------|---------|
| 角色三视图 | 所有镜头的角色一致性锚定 | 图片模型：正面 + 侧面 + 背面 |
| 场景概念图 | 锁定环境风格、光线、色调 | 图片模型：环境参考图 |
| 关键帧 | 段间衔接精准 | 图片模型生成，或截取上段末帧 |

如果你已经有参考图，Phase 0 自动跳过。

## 项目结构

```
skills/seedance-director/
├── SKILL.md                          # 核心流程（262 行）
├── references/
│   ├── platform-capabilities.md      # 即梦 10 种模式 + 技术参数 + @引用规范
│   ├── narrative-structures.md       # 5 种叙事结构及时间分配
│   ├── scene-strategies.md           # 5 类场景策略及完整提示词示例
│   └── vocabulary.md                 # 镜头语言 + 视觉风格双语词汇表
├── templates/
│   ├── single-video.md               # 5 种分镜模板（A-E）
│   ├── multi-segment.md              # 30s/45s/60s+ 多段模板
│   └── scene-templates.md            # 电商/仙侠/短剧/科普/MV
└── examples/
    ├── single-examples.md            # 6 个完整单段示例
    └── multi-examples.md             # 4 个完整多段示例
```

参考文件按需加载，核心 SKILL.md 保持精简。

## 致谢

灵感来自：

- [songguoxs/seedance-prompt-skill](https://github.com/songguoxs/seedance-prompt-skill)
- [elementsix/elementsix-skills](https://github.com/elementsix/elementsix-skills)

## License

MIT
