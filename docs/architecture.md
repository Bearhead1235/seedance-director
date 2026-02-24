# seedance-director 架构设计

> 设计人：team-lead | 完成时间：2026-02-24

## 文件结构

```
seedance-director/
├── .claude/
│   └── skills/
│       └── seedance-director/
│           └── SKILL.md              # 核心文件：系统提示词 + 交互逻辑
├── templates/
│   ├── single-video.md               # 单段视频（≤15s）分镜模板
│   ├── multi-segment.md              # 多段拼接（>15s）分镜模板
│   └── scene-templates.md            # 场景化模板（广告/短剧/仙侠/科普/MV）
├── examples/
│   ├── single-examples.md            # 单段视频示例（6个）
│   └── multi-examples.md             # 多段拼接示例（4个）
├── references/
│   └── vocabulary.md                 # 镜头语言+视觉风格词汇库
├── README.md                         # 英文
├── README_zh.md                      # 中文
├── LICENSE                           # MIT
└── .gitignore
```

## 交互流程（自适应五阶段）

```
Phase 1: 理解创意 → Phase 2: 深度挖掘(按需) → Phase 3: 生成分镜 → Phase 4: 生成提示词 → Phase 5: 操作指引
```

- 自适应跳步：分析用户输入信息密度，已明确的维度不重复追问
- 专业术语双语标注：引导用通俗语，输出标注专业术语
- 叙事结构前置：分镜之前先确认叙事结构
- 分镜→提示词双层输出

## 超长视频分段策略

| 总时长 | 段数 | 策略 |
|--------|------|------|
| ≤15s | 1段 | 单次生成，时间戳分镜法 |
| 16-30s | 2段 | 第1段正常生成，第2段视频延长 |
| 31-45s | 3段 | 第1段正常 + 2次延长 |
| 46-60s | 4段 | 第1段正常 + 3次延长 |
| >60s | 按场景拆分 | 独立场景分别生成 + 剪辑软件拼接 |

每段之间设置衔接锚点：上一段结尾画面 = 下一段起始画面描述。
