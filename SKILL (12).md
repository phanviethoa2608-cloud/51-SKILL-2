---
name: lemo-opuscar
description: Direct and produce a short film made entirely in code, in one of the styles of the Lemo-Opuscar library (e.g. Impasto Oil Painting 油画厚涂, Watercolor Brush 水彩笔刷, Ukiyo-e 浮世绘, Satellite Briefing 卫星简报, Exploded Teardown 产品拆解). Use when the user asks for a video, film, short, promo, explainer or animation (视频、短片、动画、宣传片) in a named style, including explainers built from real material such as satellite imagery, laser scans, CAD files or archive scans (数据解说、科普、产品拆解), and new films cut from real archive footage and documents (剪辑与纪实, e.g. 用档案影像剪一支新闻片); when they want a film about their own topic and haven't chosen a style (help them choose); when they want to change a style into their own, make their own style from a blank page, or use a style they saved (改风格、从空白开始、我的风格); when they start the skill without saying what they want; or when they ask which film styles there are. Not for plain editing, trimming or converting the user's existing video files.
---


## 0. Trên máy này (Windows · tiếng Việt · ổ D) — đọc trước
- **Thư viện đã cài sẵn** ở `D:\EDIT VIDEO CLAUDE\TOOLS\skill-repos\lemo-opuscar` (bản đầy đủ, có cả demo): đặt `LIB` = đường dẫn đó (biến `LEMO_OPUSCAR_HOME`), **bỏ qua `setup.sh`** (máy không có `sh`/`git`). Gói lõi đã cài: Node (`node_modules`), trình duyệt `chromium-headless-shell`, Python `.venv` (gọi `D:\EDIT VIDEO CLAUDE\TOOLS\skill-repos\lemo-opuscar\.venv\Scripts\python.exe`, không phải `.venv/bin/python`).
- Tầng `voice`/`music` chưa cài: khi cần, `.venv\Scripts\python.exe -m pip install -r requirements-voice.txt` (hoặc `-music`). Giọng tiếng Việt: dùng Edge TTS của `tao-video-giai-thich-noi-bat` thay vì giọng tiếng Anh kokoro.
- Bước `setup.sh demo <slug>` không cần: demo đã có trong `styles/<slug>/demo`.
- Thư mục: làm việc ở `D:\EDIT VIDEO CLAUDE\VIEC-DANG-LAM\<dự-án>`, video xuất ở `D:\EDIT VIDEO CLAUDE\VIDEO-XUAT\<dự-án>`; không ghi vào thư mục skill.
- Chữ tiếng Việt: chỉ dùng phông có đủ dấu (thư viện `D:\EDIT VIDEO CLAUDE\TU-LIEU\FONT`, kiểm bằng `video-minh-hoa-bo-cuc\scripts\font.py scan`); xem khung kiểm để chắc dấu không vỡ.
- Thương hiệu: lấy từ `video-bo-nhan-dien` (TÊN THƯƠNG HIỆU) trừ khi người dùng nói khác.
- **Không chạy `npx skills add ...`** (cài skill vào ổ C, phá cách quản lý một nơi `SKILLS-AI`); công cụ cần thì đã có sẵn bên dưới.
- Dịch vụ trả phí (ElevenLabs, fal, OpenAI…) chỉ dùng khi người dùng đồng ý chi phí; mặc định dùng công cụ miễn phí trên máy (Edge TTS, nhạc/SFX tự tạo).
- Máy Windows: Python `D:\EDIT VIDEO CLAUDE\TOOLS\Python312\python.exe` (trong PATH), Node 24, ffmpeg 9 trong PATH; trình duyệt Playwright dùng chung ở `CACHE\ms-playwright` (biến `PLAYWRIGHT_BROWSERS_PATH` đã đặt). Lệnh `bash`/`sh` không có: làm các bước tương đương bằng PowerShell.
- Nguồn và giấy phép: `NGUON.txt`, `LICENSE` trong thư mục skill.

# Lemo-Opuscar

The styles, guides and tools live in the Lemo-Opuscar library (đã cài sẵn ở D:\EDIT VIDEO CLAUDE\TOOLS\skill-repos\lemo-opuscar). This skill fetches it and hands you over to it.

1. **Get the library.** Run `sh "<skill base directory>/scripts/setup.sh"`. It clones or updates the library (default `~/lemo-opuscar`; a clone the user is standing in is used as it is) and prints `LIB=<path>` on its last line.
2. **Install the core tools early.** Start `sh "<skill base directory>/scripts/setup.sh" deps` right away, in the background if you can: it takes a few minutes the first time. Add `deps voice` or `deps music` later, only if the film needs them (`$LIB/TECHNIQUE.md` §1). Report missing system tools (Node, ffmpeg, Python) in your one round of questions; don't install system software yourself. Packages a style needs (its `STYLE.md` lists them) you install yourself, into the project folder.
3. **Follow the library.** Read `$LIB/AGENTS.md` and do what it says, starting with **Start** (the user's own styles in `~/lemo-my-styles/`; when they started the skill without writing anything, the language question first, then the ways in): finding the style or making their own, the one round of questions, the treatment, production and delivery.

What changes in skill mode:

| | |
|---|---|
| Project folder | `<name>/` in the folder the user started from (lowercase letters, digits, hyphens; avoid `#` and `?` in the path). Wherever the guides say `films/<name>/`, read this folder |
| Running tools | from `$LIB`, with the project's absolute path: `cd "$LIB" && node core/render/still.mjs "<project>" 1.5 3` |
| Library files in pages | absolute URLs: `/core/lib.js`, `/node_modules/three/…` (the project is served at `/@film/`) |
| Library files in scripts and `build.sh` | `$LIB/…`, with `LIB=<path>` at the top of `build.sh`; never `../..` |
| Demo source | not downloaded. `sh "<skill base directory>/scripts/setup.sh" demo <slug>`, only after your `TREATMENT.md` is written, to read its techniques; it is not meant to be rendered. An Edit & Documentary style is the exception: fetch it before the treatment to use the material tools its `STYLE.md` names in `demo/tools/` (`$LIB/AGENTS.md`, "Treatment first") |

Deliver what `$LIB/DIRECTOR.md` §11 lists, in the project folder, and tell the user where it is.
