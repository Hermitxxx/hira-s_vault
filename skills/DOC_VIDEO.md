You are a senior motion designer and Remotion engineer. Build a broadcast-grade, 30-second vertical documentary video (1080x1920, 30fps) in **Remotion** titled **"Movile Cave"**.

Your previous type of output failed because of three fatal mistakes that you are **strictly forbidden** from repeating:
1. **Disconnected voiceover & loose timing:** NEVER guess timestamps or animate before the voiceover exists.
2. **Abrupt, unmotivated cuts:** NEVER hard-cut between unrelated layouts. Every scene transition must be a physical match-cut driven by camera momentum, splitting frames, rack-focus motion blur, or scale pull-backs.
3. **Cheap visual assets:** NEVER use flat CSS rectangles, basic emojis, stock-looking vector icons, or ungraded mixed-color images.

---

## 1. Reference Study (`./video_1.mp4` — Craft Inspiration Only, Do NOT Copy)
Inspect `./video_1.mp4` in the project directory (extract frames at 2 fps + inspect audio pacing) to absorb its **exact visual physics and craft principles**:
- **Tactile Monochrome Editorial World:** Heavy embossed watercolor/canvas paper texture across the entire frame, strict monochrome high-contrast grading (`#080808` obsidian slate, `#D4D4D4` textured paper, pure ink black, crisp chalk white), subtle radial halftone dot falloff under hero objects, and realistic multi-layered directional + ambient drop shadows.
- **Continuous Match-Cut Transitions:**
  1. Framed monochrome subject physically **splits down the centerline** with snappy easing, pushing left/right halves outward to reveal the central subject emerging from inside with hairline guide lines.
  2. **Directional motion-blur whip-zoom** morphs one circular/central object directly into the next while a clean horizon line rotates smoothly across the frame.
  3. **Inversion + Volumetric Light-Cone Masking:** Background flips to dark textured slate with a central pale circle; a rotating textured light beam sweeps 360°, **revealing hidden typography ONLY inside the illuminated beam** (using SVG `<clipPath>` / CSS mask).
  4. **Zoom-Out Context Reveal:** Continuous smooth camera pull-back transforms the small central circle into a much larger architectural/environmental diagram filling the entire dark canvas, ending with the light beam narrowing to a razor slit and snapping shut on the final beat.

**Originality Rule:** Do NOT copy football, brains, eyeballs, or a soccer pitch. Translate those exact motion-design mechanics into a completely original **Subterranean Geological & Chemosynthetic Biology** visual world for Movile Cave.

---

## 2. Script & Audio-First Timing Pipeline (MANDATORY FIRST STEP)
**Script (Narration):**
> "In southeastern Romania, workers accidentally broke into a cave sealed for five million years. No sunlight had ever touched it, and its air was choked with toxic gas. It should have been completely lifeless. Instead, Movile Cave rewrote biology. Inside the dark, researchers found dozens of blind, ghost-white species found nowhere else on Earth—an entire hidden world thriving without ever needing the sun."

### Step 1 — Lock Real Audio & Word-Level Timestamps BEFORE Writing Animation Code
1. If `./voiceover.mp3` (or `.wav`) already exists in the project directory, use it. Otherwise, generate a calm, deep, authoritative documentary voiceover audio file (or stop and ask me to drop `./voiceover.mp3` in `/public/audio/` if no TTS tool is configured—do NOT silently ship a broken/robotic fallback).
2. Extract **exact word-level timestamps** from the actual voiceover file (using Whisper / `@remotion/install-whisper-cpp` / forced alignment / ElevenLabs alignment) and write them to `src/data/wordTimestamps.json` in this exact schema:
   ```json
   [
     { "word": "In", "startSec": 0.00, "endSec": 0.16 },
     { "word": "southeastern", "startSec": 0.16, "endSec": 0.82 }
   ]