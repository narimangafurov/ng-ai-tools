# NG AI Tools for Final Cut Pro — FAQ

> **Test (beta) version.** It works on my machine, but there will be bugs. Please report problems and ideas (see "Reporting a bug" below).

## What is it
A Final Cut Pro extension with three modes: **Transition** (AI transition between two clips), **Extend** (continue a scene), **Scene** (replace background or lighting). Generation is done by Higgsfield using **your own** API key; the extension only prepares frames and brings the finished ProRes back into FCP.

## Requirements
- A Mac with **Apple Silicon** (M1 or newer), macOS 14 or later. Intel Macs are not supported.
- Final Cut Pro 11 or later.
- A Higgsfield account with an API key and a funded balance.

## Installation
1. Download `NG-AI-Tools-1.0.0-arm64.pkg` and double-click it. The package is not signed with an Apple Developer ID, so macOS may say it "can't verify the developer". That is expected.
2. Open **System Settings → Privacy & Security**, scroll down to the message about the package and click **"Open Anyway"**, then confirm with your password. Run the package again and go through the installer (an administrator password is required).
3. After installing, open **NG AI Tools** from Applications once. If macOS blocks it again, use the same path: Privacy & Security → Open Anyway. If it says the app is "damaged", run in Terminal:
   ```
   xattr -cr "/Applications/NG AI Tools.app"
   ```
4. Quit Final Cut Pro completely (⌘Q) and reopen it. Then **Window → Extensions → NG AI Tools**.
5. The panel shows "Getting started": paste your key (see below) and press **"Check and save"**.

The installer also starts a small helper (it launches at login and listens on 127.0.0.1 only) and creates `~/Movies/AI_Transitions` for results. Nothing else is installed: ffmpeg is bundled inside the app; no Python or Homebrew needed.

## Getting a Higgsfield API key
1. Go to **console.higgsfield.ai** and sign in.
2. Open **API keys**, create a key and copy it in full.
3. Paste it into the NG AI Tools panel (or gear icon → "Higgsfield API key") and save.
4. Add funds under Billing. You can enable auto top-up — watch your spending.

The key is stored only in your Mac's Keychain and is sent only to Higgsfield. It is not part of the installer.

## How to use it
1. In Final Cut, select a clip on the timeline and press **⌘C**. In the panel click the **paste icon** on the slot you need (or press ⌘V to fill a free slot). You can also paste a screenshot, or drag a file in.
2. **Transition:** fill A (takes its last frame) and B (takes its first frame), pick a preset or write a prompt, choose the model and length, press Generate. The transition is made to the length you set on the slider (the model can generate a bit more or less, the result is fitted).
3. **Extend:** paste one clip. "From a frame" is cheap and uses only the last frame; "From the video" is smoother but costs more.
4. **Scene:** paste a clip, describe the new background or light (or tap a preset), pick a model. One model (Genjutsu Light) needs a reference image.
5. When it's done, play the result in the panel and **drag it onto the timeline**. Past results are in "Recent".

Tips: use a cheap model for tests and a better one for the final. Several tabs can run at once. The price next to the button updates with length and resolution.

## Limits to know about
- Results are short, about 3–15 s, and AI-generated: they can miss. Plan to retry.
- Models marked with a yellow ⚠︎ are experimental or have no price estimate.
- Some models accept only 16:9, 9:16 or 1:1 frames. For wider footage (e.g. anamorphic 2.67:1) the edges are cropped from the center, or you can tick **"Black bars for wide frames"**.
- Scene takes only part of a clip per run (6 s for most models, 10–15 s for some) because the input video is billed too.
- Not available: Google Veo / Gemini, Topaz upscaling.
- Mac with Apple Silicon only; tested on one machine.

## Is it supported? Will it be updated?
This is a personal project shared **as is, free of charge, with no promise of support or updates**. It may stop working if Final Cut Pro, macOS or Higgsfield change something. If I find time I will keep improving it, but please don't rely on it for deadlines. You use it at your own risk and your own cost.

## Reporting a bug or an idea
Please include: the mode and model, the text of the error in the panel (or a screenshot), your Mac chip, macOS and Final Cut Pro versions, and the last lines of `~/Library/Logs/ai_transition_helper.log` if the helper misbehaves. Never send your API key. Ideas for models, presets and features are welcome.

## Pricing
You pay Higgsfield per second of video; the extension itself is free of charge. Rough guide as of 2026-10-29 (prices change; the panel shows the exact price before each run):

| Model | Approx. per second |
|---|---|
| Wan 3.0 | $0.02–0.05 |
| Kling O3 / 2.5 Turbo | $0.04–0.07 |
| Kling 2.6 Pro | about $0.12 |
| Seedance 2.0 | $0.10–0.14 |
| Seedance 2.5 | $0.14–0.21 |

A 3–5 second transition on the cheaper models usually costs tens of cents; Extend "from video" and Scene cost more (input and output video are both billed). In Settings you can set a price threshold above which a confirmation is shown.

## Troubleshooting
- **The extension is missing from Window → Extensions.** Quit FCP (⌘Q) and reopen it. Make sure the app is in `/Applications` and you opened it once. Otherwise reinstall the package.
- **Red dot / "Helper is not running".** Log out and back in, or reinstall. Check in Terminal: `curl http://127.0.0.1:8787/health`. Log: `~/Library/Logs/ai_transition_helper.log`.
- **"Key rejected".** Copy the full key without spaces, create a new one at console.higgsfield.ai, check your balance.
- **Job failed / model unavailable.** Read the error in the panel and try another model; some models may not be available for your account.
- **Result can't be dragged to the timeline.** The results folder must be in Movies, Desktop, Documents, Downloads or an external drive (gear → Output folder).
- **HEIC images are not accepted.** Use PNG or JPG (macOS screenshots are PNG).

## Uninstall
Gear → **"Remove the helper and login item…"**, then drag "NG AI Tools" from Applications to the Trash. You can delete the key in Keychain Access (item `higgsfield-api`). Finished videos stay in `~/Movies/AI_Transitions`.

## Security and privacy
Frames and clips you submit are sent to Higgsfield over HTTPS. The helper listens on localhost only. The package is not Apple-signed (no paid Developer ID), hence the first-run warning. The bundled ffmpeg is built unmodified from official sources, LGPL (see `ffmpeg-LICENSE.txt` inside the app).
