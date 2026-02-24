<p align="center">
  <a href="README.md">English</a> | <a href="README_zh.md">中文</a>
</p>

# seedance-director

> AI video director for the Seedance (即梦) platform. Idea in, production-ready prompts out.

An [Agent Skill](https://agentskills.io) that guides you from a rough idea through asset preparation, storyboarding, and prompt generation — all inside Claude Code.

## Install

```bash
npx skills add https://github.com/crowscc/seedance-director
```

Then describe your video idea. Activates on keywords like "Seedance", "即梦", "storyboard", "分镜", "运镜", etc.

## What's Different About v2

| | v1 | v2 |
|---|---|---|
| Assets | You bring your own | Generates character turnaround sheets, scene concepts, key frames |
| Consistency | One reference image, hope for the best | Turnaround sheets + scene anchors across every segment |
| Long videos | Chain via video extension (quality degrades) | Independent generation + first-frame stitching for 3+ segments |
| Interaction | Text-based Q&A | Structured choices via `AskUserQuestion` with dynamic options |
| Architecture | 635-line monolith | 262-line core + progressive-disclosure references |

## How It Works

```
You: I want a 30-second coffee brand ad. Cinematic, warm tones. I have 3 product photos.

Director: [AskUserQuestion]
          Which narrative structure fits best?
          > Process Journey — bean to cup, matches your product story ✓
          > Contrast — before/after, good for taste reveal
          > Custom — describe your own

You: Process Journey

Director: [AskUserQuestion]
          You're missing character/scene references.
          > Generate scene concept art for me
          > I'll provide my own
          > Skip — text-only generation

You: Generate scene concept art

Director:
  1. Scene concept prompt (paste into image generator):
     "Coffee plantation at sunrise, golden hour, mist rising..."

  2. Storyboard (2 segments, 16:9, cinematic warm):
     001 | 0-4s  | Macro CU    | Dolly In | Bean texture, golden backlight
     002 | 4-8s  | Close-Up    | Tilt Down| Beans into grinder
     003 | 8-12s | Medium Shot | Static   | Pour-over, steam rising
     ...

  3. Segment 1 prompt (paste into Seedance):
     宽高比16:9，时长15秒，电影质感...
     @图片1 中的咖啡豆在晨光中缓缓滚动...

  4. Operation guide:
     Upload product photo → Set 15s, 16:9 → Generate
     → Screenshot last frame → Use as first-frame ref for segment 2
```

## Multi-Segment Strategy

| Duration | Strategy | Why |
|----------|----------|-----|
| Up to 15s | Single generation | Within platform limit |
| 16-30s | Segment 1 + video extension | Extension's sweet spot, natural continuity |
| 31s+ | Independent generation per segment | Each segment uses turnaround sheets + scene art + previous segment's last frame as references. Avoids error propagation and style drift. Assemble in CapCut/editor. |

## Asset Preparation (Phase 0)

Before generating any video, the director checks what you have and offers to fill gaps:

| Asset | Purpose | How |
|-------|---------|-----|
| Character turnaround sheet | Consistent character across all shots | Image model: front + side + back views |
| Scene concept art | Lock environment style, lighting, color | Image model: environment reference |
| Key frames | Ensure segment-to-segment continuity | Image model or screenshot from previous segment |

If you already have reference images, Phase 0 is skipped entirely.

## Project Structure

```
skills/seedance-director/
├── SKILL.md                          # Core workflow (262 lines)
├── references/
│   ├── platform-capabilities.md      # 10 Seedance modes + tech specs + @reference rules
│   ├── narrative-structures.md       # 5 narrative structures with timing
│   ├── scene-strategies.md           # 5 scene-type strategies with full prompt examples
│   └── vocabulary.md                 # Camera + visual style bilingual vocabulary
├── templates/
│   ├── single-video.md               # 5 storyboard templates (A-E)
│   ├── multi-segment.md              # Multi-segment templates for 30s/45s/60s+
│   └── scene-templates.md            # E-commerce / Xianxia / Drama / Education / MV
└── examples/
    ├── single-examples.md            # 6 complete single-segment examples
    └── multi-examples.md             # 4 complete multi-segment examples
```

References are loaded on demand — the core SKILL.md stays lean.

## Acknowledgments

Inspired by:

- [songguoxs/seedance-prompt-skill](https://github.com/songguoxs/seedance-prompt-skill)
- [elementsix/elementsix-skills](https://github.com/elementsix/elementsix-skills)

## License

MIT
