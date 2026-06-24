# Quick Subtitler Script

Pipeline for generating English subtitles from long-form (1hr+) videos, without hitting the file-length limits most free online AI-subtitler tools impose. Audio extraction, transcription, and a rough translation pass are automated in a notebook; final cleanup and burned-in on-screen text are done manually in a subtitle editor. **The notebook is configured for Korean, please adjust accordingly to your needs**

## What's automated vs. manual

| Step | Automated? | Tool |
|---|---|---|
| Extract audio | ✅ Notebook | ffmpeg |
| Transcribe (Korean) | ✅ Notebook | faster-whisper |
| Rough translate (KO→EN) | ✅ Notebook | deep-translator (Google) |
| Fix translation, timing, tone | ❌ Manual | Aegisub / Subtitle Edit |
| Add burned-in on-screen text | ❌ Manual | Aegisub / Subtitle Edit |
| Final QC | ❌ Manual | — |

The last three steps can't be automated: burned-in graphics aren't in the audio track at all, and machine translation reliably flattens jokes/tone, especially in variety-show speech, so a human pass is required either way for high-quality subs.

## Usage

### Option A — Google Colab (no local GPU needed)
1. Open `transcribe_translate.ipynb` in Colab.
2. Upload your video to Google Drive, mount Drive in the notebook, set `VIDEO_PATH`.
3. Run all cells.
4. Download the resulting `*_en.srt` from Drive.

### Option B — Local (if you have a GPU)
```bash
git clone <this repo>
pip install -r requirements.txt
jupyter notebook notebook/transcribe_translate.ipynb
```
Set `VIDEO_PATH` to your local file and run all cells.

### After the notebook
1. Open the generated `*_en.srt` alongside the video in **Aegisub** or **Subtitle Edit**.
2. Fix mistranslations, tone, and timing.
3. Manually add subtitle lines for any burned-in on-screen text (reaction captions, name tags, etc.) — the model never sees these since they're graphics, not audio.
4. Export your final `.srt`/`.ass`.

## Notes
- Don't commit video/audio files or model weights to this repo — see `.gitignore`. GitHub isn't built for hosting large media; keep those local or on Drive.
- For better translation quality than Google Translate, swap in DeepL or Papago in the notebook's translation cell (requires an API key).
- Test the full pipeline on a short 5–10 minute clip before running it on a full episode.
