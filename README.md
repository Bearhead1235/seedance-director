<p align="center">
  <a href="README.md">English</a> | <a href="README_zh.md">中文</a>
</p>

# seedance-director

Your AI Director for Seedance 2.0 — From creative ideas to professional storyboards and production-ready prompts.

## Features

- **Film methodology meets AI** — Integrates classic storytelling frameworks (Three-Act Structure, Kishōtenketsu, Beat Sheet) into prompt generation
- **Full Seedance 2.0 coverage** — Supports all 10 platform capabilities including text-to-video, motion reference, one-take, video extension, and more
- **Adaptive 5-phase workflow** — Beginner-friendly guided flow; experienced users can skip ahead
- **Single & multi-segment** — Handles short clips (up to 15s) and multi-segment projects (60s+) with automatic segmentation
- **Three-layer output** — Professional storyboard + Seedance prompts + platform operation guide
- **Bilingual shot notation** — Camera terminology annotated in both Chinese and English
- **5 scene templates** — Ready-made templates for e-commerce, xianxia, short drama, educational, and music video genres

## Quick Start

```bash
claude skill add --url https://github.com/crowscc/seedance-director
```

Then start a conversation with Claude and describe your video idea. The skill activates automatically when you mention keywords like "Seedance", "video prompt", "storyboard", or "AI video".

## How It Works

```
Your Idea → Understand → Deep Dive → Storyboard → Prompt → Action Guide
   📝          Phase 1      Phase 2     Phase 3     Phase 4    Phase 5
```

**Phase 1 — Understand**: Scans your input across 5 dimensions (theme, duration, style, assets, audio) and determines how much guidance you need.

**Phase 2 — Deep Dive** (adaptive): Only asks about missing dimensions. Offers narrative structure options (hook-twist, contrast, suspense, etc.) via simple multiple-choice questions. Skipped entirely if your input is already detailed.

**Phase 3 — Storyboard**: Generates a professional shot-by-shot breakdown with timestamps, shot sizes, camera movements, and scene descriptions.

**Phase 4 — Prompt**: Converts each shot into a production-ready Seedance 2.0 prompt, selecting the optimal capability for each segment.

**Phase 5 — Action Guide**: Step-by-step platform instructions — which buttons to click, what parameters to set, and how to chain segments together.

## Usage Example

```
You:    I want to make a 30-second coffee brand ad. Cinematic style,
        warm tones. I have 3 product photos.

Director: Got it — cinematic coffee ad, 30s, warm tones, product photos ready.
          A couple of quick questions:

          Narrative structure?
          A. Hook → Reveal — Open with steam close-up, reveal the full scene
          B. Process — Bean to cup journey
          C. Lifestyle — Character enjoying coffee in a cozy setting

You:    B

Director: [Generates full storyboard]

          ## Storyboard: Coffee Origins

          **Structure**: Process (Bean → Brew → Enjoy)
          **Duration**: 30s (2 segments) | **Ratio**: 16:9 | **Style**: Cinematic warm

          | Shot | Time  | Size        | Movement    | Description              |
          |------|-------|-------------|-------------|--------------------------|
          | 001  | 0-4s  | Macro       | Slow Push   | Coffee beans in sunlight |
          | 002  | 4-8s  | Close-Up    | Tilt Down   | Beans pouring into grinder|
          | ...  | ...   | ...         | ...         | ...                      |

          ## Segment 1 Prompt (0-15s)
          @Photo1 中的咖啡豆在晨光中缓缓滚动，镜头从微距缓推至近景...

          ## Segment 2 Prompt (延长15s)
          将@视频1延长15秒。接续画面：热咖啡倒入杯中...

          ## Operation Guide
          1. Open Seedance 2.0 → Image-to-Video
          2. Upload product photo 1, paste Segment 1 prompt...
```

## Supported Capabilities

| # | Capability | Description |
|---|-----------|-------------|
| 1 | **Text-to-Video** | Generate video from text descriptions alone |
| 2 | **Consistency Control** | Lock character/scene appearance via reference images |
| 3 | **Motion Reference** | Replicate camera movements from a reference video |
| 4 | **Effect Reference** | Replicate visual effects from a reference video |
| 5 | **Story Completion** | AI fills in the animation between key frames |
| 6 | **Video Extension** | Extend an existing video with new content |
| 7 | **Voice & Audio** | Control dialogue, voice timbre, and sound effects |
| 8 | **One-Take** | Chain multiple images into a seamless long take |
| 9 | **Video Editing** | Modify specific elements within an existing video |
| 10 | **Music Sync** | Synchronize visuals to music beats |

## Multi-Segment Strategy

| Total Duration | Segments | Strategy |
|---------------|----------|----------|
| up to 15s | 1 | Single generation with timestamp-based storyboard |
| 16-30s | 2 | Generate first segment + 1 video extension |
| 31-45s | 3 | Generate first segment + 2 extensions |
| 46-60s | 4 | Generate first segment + 3 extensions |
| 60s+ | By scene | Independent scenes + editing software assembly |

Each segment includes an **anchor frame** — the ending frame of one segment matches the opening description of the next, ensuring visual continuity.

## Project Structure

```
seedance-director/
├── .claude/
│   └── skills/
│       └── seedance-director/
│           └── SKILL.md              # Core file: system prompt + interaction logic
├── templates/
│   ├── single-video.md               # Single-segment (up to 15s) storyboard template
│   ├── multi-segment.md              # Multi-segment (15s+) storyboard template
│   └── scene-templates.md            # Genre templates (ad/xianxia/drama/edu/MV)
├── examples/
│   ├── single-examples.md            # 6 single-segment examples
│   └── multi-examples.md             # 4 multi-segment examples
├── references/
│   └── vocabulary.md                 # Camera & visual style bilingual vocabulary
├── docs/
│   ├── PROJECT.md                    # Project goals and design decisions
│   └── architecture.md               # Architecture design
├── README.md                         # English documentation
├── README_zh.md                      # Chinese documentation
└── LICENSE                           # MIT License
```

## Acknowledgments

This project was inspired by and builds upon ideas from:

- [songguoxs/seedance-prompt-skill](https://github.com/songguoxs/seedance-prompt-skill) — Inspired the capability templates, vocabulary library, and multi-segment video strategy
- [elementsix/elementsix-skills](https://github.com/elementsix/elementsix-skills) — Inspired the interactive guidance workflow and modular architecture

## License

MIT
