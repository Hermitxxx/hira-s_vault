---
name: vagents
description: Generate a "high voltage" master agent specification (AGENTS.md) that a coding agent uses to build a broadcast-grade motion-graphic YouTube explainer video in Remotion / HyperFrames with lucide-react icons. The spec is frame-accurate at 60 fps, synced to the voiceover, uses an original brand palette, and guarantees there is never an empty or dead screen. Use this whenever the user asks for a master spec, agents spec, AGENTS.md, motion design brief, video agent prompt, or Remotion/HyperFrames build plan for a tech video. This includes requests that come with a script, .srt, voiceover, screenshots or reference videos, and requests like "make the spec for my next video" or "vagents this script", even if they don't say "AGENTS.md".
---

# vagents: Master Agent Specification Generator

You write the **master production spec** that a coding agent (Antigravity, Claude Code, Codex, etc.) follows to build a finished motion-graphic explainer video. The spec is the only thing that agent sees, so it has to be complete, frame-accurate and unambiguous. If the spec is vague, the agent will guess, and the video will show those guesses.

The gold-standard example is the `WinUtil_v2` spec. It produced a video the user loved, and it had exactly **one flaw** that this skill exists to prevent forever (see "The Dead-Air Law" below).

---

## Step 1: Gather inputs (read everything first)

Read every file the user attached before you write anything. Typical inputs are:

| Input | What you extract |
|---|---|
| `raw_script.txt` / pasted script | Every sentence, the punchlines, the action verbs ("right-click", "toggle", "scroll down") |
| `subtitles.srt` | Exact cue timestamps. These are the **master clock** |
| `voiceover.wav` | Probe the duration if you can (`ffprobe`). Otherwise tell the build agent to probe it |
| Screenshots (`*.png`) | **Open and look at each one.** Write down the visible UI elements and estimate normalized `x/y/w/h %` regions |
| Reference videos | Sample frames (`ffmpeg -ss T -frames:v 1`) every 5–10s. Note the layout, camera moves, HUD style, pacing and palette, so you can **emulate the motion and avoid copying the colors** |
| Previous spec / previous output video | Inherit everything that worked. Find any defects (sample frames) and add them to the Banned Defects list |

If there is no `.srt`, estimate timestamps from the script at ~2.5 words/sec, mark every timestamp as `PROVISIONAL`, and make the build agent re-lock them with `ffprobe` + `silencedetect` before coding.

Ask the user only for what you truly can't infer (for example, the official product name or launch command if the script is unclear). Use sensible defaults for everything else and state them at the top of the spec:
- Canvas: 1920×1080, 16:9, **60 fps**
- Audience: US YouTube tech audience
- Stack: Remotion + HyperFrames + `lucide-react`, with procedural SFX via Node/FFmpeg
- Host: Antigravity inside Termux on Android (lean deps, `--concurrency=1`), unless the user names another environment

Verify technical facts (commands, menu paths, repo URLs, setting names) against official sources with web search when you can. A wrong command shown on screen ruins a tech video's credibility.

---

## Step 2: Build the beat map (the backbone)

Turn the script into a table where **each row is one spoken beat** (a sentence or clause, roughly 2–5s):

| # | Start | End | Frame | Spoken words (short excerpt) | Visual event | Hero on stage | SFX | Bottom pill |

Rules for the beat map:
1. Group beats into **scenes of ~12–30s**. Every scene cut lands on a sentence-end breath gap, never mid-word.
2. Every beat gets **at least one new visual event** (an entry, morph, zoom, click, counter or reveal). A beat with nothing new is a defect.
3. When narration describes an action ("right-click Start", "type this command", "click Enable"), the visual for that action **starts on or slightly before (up to 6 frames) the first word of the instruction**, never after the sentence ends.
4. The **Hero on stage** column can never be blank. This column is how you run the Dead-Air audit.

---

## Step 3: The Dead-Air Law (lesson learned from `WinUtil_v2`)

**What went wrong:** In `WinUtil_v2`, the spec said at 00:29.6 *"Trust card slides up and shrinks into a compact top badge, clearing center stage."* The next visual (taskbar + Start menu) was scheduled for 00:34.2. The viewer got **~4.5 seconds of an empty stage** while the narrator kept talking about a PowerShell command. That's where viewers feel bored and disconnected, and where retention drops.

**Why it happened:** The spec had an **exit with no paired entry**. "Clearing the stage" was written as if it were an event, but it actually removes the event. The beat in between ("just one clean PowerShell command") had no visual assigned.

**The law, which applies to every spec you write:**
1. **No vacant stage longer than 0.5s (30 frames at 60 fps).** The center hero zone must always hold a focal subject, apart from a planned transition of 0.35–0.45s at most.
2. **Every exit is a handoff.** Any time an element leaves, shrinks, docks to a badge or "clears the stage", the same keyframe must say what enters in its place, starting on the same frame (or overlapping by 6–12 frames). The words "clearing center stage" are banned unless they are followed by "...as [X] springs in".
3. **Every spoken idea gets a picture.** If narration mentions a concept that is not yet on screen (for example "one PowerShell command"), you must schedule a **bridge visual** for it. Bridge visual examples:
   - a ghost preview of the upcoming object (a dark terminal card with a blinking cursor and a `PowerShell` label),
   - a kinetic keyword card (`1 Command` with an icon),
   - a camera push or parallax drift onto the next hero building itself in,
   - an icon morph (shield → terminal icon).
4. **Max 1.8s without a new visual event**, and nothing is ever fully frozen (ambient drift, cursor blink and aura breathing are required).
5. **The "Hold & Breathe" holds (0.6–1.2s after a reveal) still need a hero on screen.** Holding means *a settled hero with ambient motion*, never an empty stage.

The spec you write must include:
- a **Visual Coverage Table** (Section 7.0 in the template) listing every second-range of the video and its on-stage hero, with **zero gaps**, and
- a **Dead-Air QA check** in the pre-export checklist: render stills every **0.5s across every transition window and every "dock/shrink/exit" moment**, then confirm the center zone (roughly 640–1280 x, 250–830 y) is never empty.

Before you deliver, scan your own spec for the words *clear, clears, clearing, exits, dissolves, slides out, shrinks into, swipes out, fades out*. Each one needs a matching "as [X] enters" in the same keyframe. Fix any that don't have one.

---

## Step 4: Design an original brand system

Each video gets its own **named palette concept** (e.g. "Cobalt Precision"). It must be:
- **original**: never clone a reference video's palette. Name the reference palettes you're avoiding in the Banned Defects list.
- **light-stage friendly** by default (a premium light studio stage with a dark obsidian accent for terminals and HUD pills), unless the topic suggests otherwise (gaming topics can go to a dark stage).
- complete: base/grid/3 aura blooms (primary, secondary, warning), card surfaces, dark surfaces, 2 brand accents + gradient, a cyan/secondary accent, semantic success/danger/warning with tinted backgrounds, 4 ink levels, 3 multi-layer shadows, sans + mono font stacks, layout constants (`fps: 60`, safe margins, HUD safe bands).

Output it as a single immutable `src/theme.ts` `THEME` object with exact hex/rgba values and comments.

---

## Step 5: Write the spec using this exact template

Use this section structure. Keep the WinUtil_v2 level of precision: every keyframe gets a timestamp, a frame number (`f = round(s*60)`), the quoted narration it syncs to, exact copy text, Lucide icon names, easing, and SFX.

```
# AGENTS.md: `<ProjectName>` Master Production Specification

> Persona Directive (senior motion designer + generative video prompt specialist)
> Goal (duration mm:ss, 1920×1080, 60 fps, audience)
> Brief (subject, official URL, emulate reference kinetic polish, original palette, zero layout defects, zero dead air)
> Technical Specs (Remotion, HyperFrames, lucide-react; deterministic frame-driven only, no CSS transitions / rAF;
>   timestamps + 60fps frame keys; locked theme.ts; Disney easing: anticipation, spring overshoot with fps: 60,
>   6–10f staggers, follow-through, Easing.bezier(0.22, 1, 0.36, 1); 6–10 words max per headline)

## 1. MANDATORY SKILL DISCOVERY & EXECUTION PROTOCOL (HARD GATE)
   Required skill domains (Remotion, HyperFrames, Motion Design, Icons/SVG/UI, Audio/FFmpeg);
   run find-skills if missing; document loaded skills; never guess APIs.

## 2. PROJECT IDENTITY, ENVIRONMENT & ROOT RESOURCES
   Project name, audience, host environment constraints, root files (voiceover.wav, screenshots,
   raw_script.txt, subtitles.srt), official tool reference (repo/URL/exact commands verified),
   setup rule: copy assets into public/ and use staticFile().

## 3. FORENSIC DESIGN RULES
   A. Reference quality benchmarks (full-stage centered hero, depth/atmosphere grid + aura,
      HUD: top chapter pill at top:36px + bottom 2–5 word takeaway pill at bottom:40px,
      living 60fps canvas, real-UI camera zoom + spotlight storytelling)
   B. Zero-tolerance banned defects: text collisions, audio desync/cutoff, 30fps hardcoding,
      repetitive layouts (list the varied layout per scene), cloned reference colors,
      ignored assets, **DEAD AIR / EMPTY STAGE (see Section 6.C)**, plus any defect
      found in the previous version's output.

## 4. LOCKED ORIGINAL BRAND IDENTITY (src/theme.ts)
   Brand concept name + one-line mood, full THEME code block.

## 5. HERO ASSET MASTERY (per real screenshot)
   Visual anatomy, framing (glass bezel, radius, shadow), normalized region table
   (x%, y%, w%, h%) for every control the narration mentions, the <StageCamera> component
   API (focusX, focusY, zoom, rotateX, rotateY, spotlightRect), spotlight mask, highlight ring
   colors, SVG cursor with spring travel + click ripple, glass callout cards placed in the
   negative space OPPOSITE the zoomed region.

## 6. AUDIO, PACING & CONTINUOUS COVERAGE
   A. Audio-first sync (ffprobe duration D, durationInFrames = ceil((D + 2.0) * 60),
      silencedetect to lock cuts inside breath gaps; transition 21–27f; hold & breathe 36–72f;
      dramatic ambient-bed drops at named high-impact lines)
   B. Procedural SFX engine scripts/generate-sfx.mjs (each file: duration, synthesis recipe,
      usage, volume; 48kHz stereo, fades); master loudness rule (VO 1.0, -14 LUFS, SFX never over VO)
   C. Continuous Visual Coverage (Dead-Air Law): the 5 rules from this skill, written out
      for this video, including the max-vacancy and max-static numbers.

## 7. MASTER SCENE-BY-SCENE BLUEPRINT
   7.0 Visual Coverage Table: | Time range | Frames | On-stage hero | Entering next | (zero gaps)
   Then for each scene:
   ### Scene N: <Title> | mm:ss.s – mm:ss.s (Frames a – b)
   - Chapter Pill: `NN · TITLE` (dot color + hex)
   - Visual Brief: one or two sentences, including which layout archetype is used
   - Keyframe Choreography: bullets of
     `mm:ss.s` (`f=…`, *"quoted narration"*): what enters/moves/morphs, exact on-screen copy,
     Lucide icons, spring/easing, rotate/scale values, SFX, bottom pill text.
     Every exit bullet states its paired entry.
   - On-screen text cap.

## 8. ARCHITECTURE, LAYOUT SAFETY & AUTOMATED QA
   File structure (theme.ts, timeline.ts, StageBackground, HudChrome, StageCamera,
   AnimatedCursor, scenes/SceneN*.tsx, scripts/generate-sfx.mjs), SafeStage container,
   flex/grid with gap >= 28px and minWidth: 0, chart labels in flex headers.
   Mandatory pre-export verification:
   1. Audio length & sync audit
   2. npx tsc --noEmit (zero errors)
   3. Anti-overlap still audit (>= 10 named frames)
   4. DEAD-AIR AUDIT: stills every 0.5s through every transition / exit / dock moment
      (list the exact frame ranges), center zone never empty, no event gap > 1.8s
   5. Device-safe 60fps render command (concurrency, jpeg-quality, h264 crf 18, aac 192k, yuv420p)
```

### Layout archetypes to rotate (don't repeat the same one in back-to-back scenes)
- 3-card hero dashboard (problem/stress hook)
- Centered trust/hero card + staggered pills
- Dark terminal / command window, typed character by character with syntax colors
- OS mockup (taskbar, Start, context menu, settings panel) with cursor
- Full-stage 3D-tilted real screenshot with camera zoom + spotlight + opposite-side callouts
- Before/after bar chart or counter with labels in a flex header
- Split comparison cards (Do / Don't, Laptop / Desktop)
- Centered CTA end card (Like + Subscribe cursor clicks, tagline, source footer)

### Copy rules for on-screen text
- Headlines ≤ 6–10 words, chips/cards ≤ 5 words, bottom pill 2–5 words.
- Bottom pill is a punchy takeaway (`• 1-Click Safe Preset`), never a subtitle sentence.
- Exact commands/URLs are shown verbatim and verified.

---

## Step 6: Self-check before delivering

Go through these and fix anything that fails:
- [ ] Every timestamp has a matching frame number, and `f = round(s × 60)` is correct.
- [ ] Scene ranges are contiguous with no gaps or overlaps, and the last scene ends ≈ 2s after the final word.
- [ ] **Coverage table has zero empty ranges. Every exit verb has a paired entry. Every narration beat has a visual event.**
- [ ] Every action the narrator instructs has a visual that starts on (or just before) the words.
- [ ] Every screenshot control the narration mentions has a region in Section 5 and is used in a scene.
- [ ] No palette hex values are copied from the references. Every color in the scenes comes from `THEME`.
- [ ] No hardcoded `30` fps, and every `spring()` gets `fps`.
- [ ] Layout archetypes vary between neighboring scenes.
- [ ] Text caps are respected and no sentence-length subtitles appear in pills.
- [ ] The QA section lists the exact dead-air audit frame ranges for this video.

---

## Step 7: Deliver

- Save the spec as `<ProjectName>_master_agent_spec.md` (Markdown, ready to drop into the project root as `AGENTS.md`) and send the file to the user.
- In the reply, write 2–4 lines: the palette concept name, the scene count and runtime, and anything marked `PROVISIONAL` or needing verification (e.g. screenshot regions estimated from the image).
- Don't paste the whole spec into chat. The file is the deliverable.
