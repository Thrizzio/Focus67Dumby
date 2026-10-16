# Focus67dumby (FocusDumbyDumb)

## What this is
A tiny, privacy-first floating sticky-note accountability widget for developers and freelancers that uses ultra-lightweight local open-weight AI (`gemma3:270m` / `gemma2:2b` via Ollama) to distinguish productive coding tutorials and tasks from YouTube rabbit holes—100% offline with zero cloud cost.

## Stack
- **Language:** Python 3.11+ (pinned in `.python-version`)
- **Desktop UI:** `customtkinter` (frameless, always-on-top floating sticky-note overlay)
- **Local AI Runtime:** Ollama REST API (`http://localhost:11434`, configurable via `.env`)
- **OS Integration:** `pygetwindow` (Windows active window detection) + `plyer` / `winotify` (notifications) + automatic headless/CLI simulation fallback for Linux/Docker rebuilds
- **Testing & Packaging:** `pytest`, `Docker`, `requirements.txt` + `requirements.lock`

## Commands
- Build: `docker build -t focus67dumby .`
- Test: `pytest tests/ -v`
- Lint: `python -m py_compile src/*.py`
- Run locally (Desktop GUI): `python -m src.main`
- Run locally (Headless/Linux CLI Mode): `python -m src.main --headless`

## Conventions
- `src/main.py`: Entry point; auto-detects GUI availability (`DISPLAY` / OS) or `--headless` flag so Linux Docker containers never crash on X11/Tkinter imports.
- `src/ollama_client.py`: All HTTP calls to Ollama (`/api/tags` and `/api/generate`). Uses strict JSON mode (`"format": "json"`).
- `src/window_watcher.py`: Polls foreground window title every 10s. Caches evaluated window titles in memory so identical titles never re-trigger LLM calls.
- `src/ui_sticky.py`: CustomTkinter floating sticky-note interface. All background thread updates MUST pass through `queue.Queue()` or `root.after()`.
- `src/notifier.py`: Cross-platform notification dispatcher (Windows Toast + UI visual pulse + stdout log in headless mode).
- `tests/test_classifier.py`: Deterministic unit tests (including mock/offline fallback tests so `pytest` passes 100% inside clean Linux CI/Docker machines without requiring a live GPU).

## Treat as high-stakes (explain before implementing)
1. **Judge Rebuildability (Clean Linux Docker Check):** Judges rebuild and test this repo on a clean Linux machine without a GUI display or pre-pulled Ollama model. `Dockerfile` and `pytest` MUST succeed out-of-the-box without throwing `TclError: no display name` or failing on missing local models, and no absolute paths from this machine are allowed.
2. **UI Thread Safety:** Never run `requests.post()` or blocking loops on the main Tkinter thread.
3. **Sub-1B Model Prompting (`gemma3:270m`):** Keep prompts under 60 words with strict


# Focus67dumby

## What this is
A tiny, privacy-first floating sticky-note accountability widget powered by local open-weight models (`gemma3:270m` / `gemma2:2b` via Ollama) that distinguishes productive coding tutorials from YouTube rabbit holes.

## Stack
Python 3.11, CustomTkinter (Desktop GUI),Headless CLI fallback (Linux/Docker), Ollama REST API, Pytest, Docker.

## Commands
- Build: `docker build -t focus67dumby .`
- Test: `pytest`
- Run GUI: `python -m src.main`
- Run Headless Demo (Linux/Docker): `python -m src.main --headless --demo`

## Conventions & Architecture
Always load and follow the **`focus67dumby-master`** skill before writing or modifying any files.

## Treat as high-stakes (explain before implementing)
1. **Linux Docker Rebuildability:** Never import `tkinter`/`customtkinter` unconditionally at module top-level or include Windows-only packages without `; sys_platform == 'win32'` environment markers in `requirements.txt` / `requirements.lock`.
2. **UI Thread Safety:** Never block the Tkinter mainloop with synchronous HTTP calls.