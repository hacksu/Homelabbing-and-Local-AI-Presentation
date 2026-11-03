# Homelabbing and Local AI

A HacKSU workshop deck on the basics of homelabbing, using a real homelab as the running example. Built on the HacKSU presentation template.

```text
.github/workflows/
  pages.yml               builds and deploys to GitHub Pages
static/
  app.js                  slide navigation and particle background
  style.css               HacKSU deck design
  HacKSU.png              logo
  images/                 slide images
templates/
  slides/                 template layouts to copy from
    homelab/              homelab half (Ian)
    ai/                   local AI half (Meheraj)
    closing.html          shared Q&A slide
  base.html               page shell, nav bar, logo
  index.html              slide order
  notes.html              speaker notes and run of show
app.py                    base app, two routes: deck, notes
build.py                  renders the deck to static HTML in dist/
OUTLINE.md                slide-by-slide content outline
pyproject.toml            project metadata and dependencies (uv)
README.md                 documentation
```

A Flask app on port 5174, managed with [uv](https://docs.astral.sh/uv/).

Run the commands below from a Linux shell (WSL on Windows, or macOS/Linux). If uv isn't installed yet:

`curl -LsSf https://astral.sh/uv/install.sh | sh`

Install dependencies:

`uv sync`

To run the app:

`uv run app.py`

`/` is the deck, navigated with arrows, space, or the dots.

`/notes` is the speaker notes with a minute-by-minute run of show.

To add a dependency, use `uv add <package>` rather than editing `pyproject.toml` by hand, so `uv.lock` stays in sync.

To change the order of slides, navigate to `templates/index.html`, and reorder using `{% include %}`

Use relative paths (`static/...`, no leading slash) for images and other files, so the deck also works on GitHub Pages.

## GitHub Pages

Every push to the default branch builds the deck and publishes it to GitHub Pages. After forking, do this once:

1. **Actions** tab: enable workflows (GitHub disables them on forks).
2. **Settings → Pages**: set Source to **GitHub Actions**.
3. **Actions** tab: run **Deploy to GitHub Pages** manually, or push a commit.

The deck is published at `https://<username>.github.io/<repo-name>/` and the notes at `.../notes.html`.

To build locally: `uv run build.py`, then open `dist/index.html`.

Design and plumbing lifted from the polyglot programming deck.

# HacKSU Local AI Demo

Run a small language model on your Mac or Windows PC and turn homelab notes into a quiz. Choose **Ollama** for a terminal demo that matches the homelab presentation, or **LM Studio** for a graphical chat window. You only need one app.

The model generates answers on your computer. Download the app and model while online, then try them offline. The local Ollama commands below require no account or subscription. [Ollama Cloud documentation](https://docs.ollama.com/cloud) · [LM Studio offline operation](https://lmstudio.ai/docs/app/offline)

## Mac

### Option A — Ollama in Terminal

1. Open **Apple menu → About This Mac**. Ollama requires macOS 14 Sonoma or newer. Apple Silicon Macs can use GPU acceleration. Supported Intel Macs use the CPU. [Mac requirements](https://docs.ollama.com/macos)
2. Download Ollama from the [Mac download page](https://ollama.com/download/mac). In Finder, open **Downloads**, double-click the downloaded `.dmg`, and drag **Ollama** into **Applications**.
3. Open **Ollama** from Applications. Allow it to install the command-line tool if prompted. Leave the app running.
4. Press **Command + Space**, type **Terminal**, and press **Return**.
5. Paste the following command, press **Return**, and wait for the model download to finish. The file is about 986 MB. [Model page](https://ollama.com/library/qwen2.5:1.5b)

   ```bash
   ollama pull qwen2.5:1.5b
   ```

6. Start a chat. When you see `>>>`, paste the first prompt from [Demo prompts](#demo-prompts). Type `/bye` to exit.

   ```bash
   ollama run qwen2.5:1.5b
   ```

If Terminal says `command not found`, reopen Ollama, check for the command-line installation prompt, and try again in a fresh Terminal window. [CLI reference](https://docs.ollama.com/cli)

### Option B — LM Studio with a chat window

1. Check **Apple menu → About This Mac**. LM Studio requires Apple Silicon and macOS 14 or newer. It recommends 16 GB of RAM. For an Intel Mac, use Ollama. [LM Studio requirements](https://lmstudio.ai/docs/app/system-requirements)
2. Download the Mac installer from [LM Studio](https://lmstudio.ai/download). Open the download, install the app, and launch it from **Applications**.
3. Open **Discover**, search for `Qwen2.5-1.5B-Instruct`, and choose a **GGUF** variant labeled **Q4_K_M** if available. This variant stores the model weights at lower precision to reduce memory use.
4. Click **Download**, then go to **Chat** and select the downloaded model from the model loader. Once it loads, paste the first prompt below. [LM Studio setup guide](https://lmstudio.ai/docs/app/basics)

## Windows

### Option A — Ollama in PowerShell

1. Press **Windows + R**, type `winver`, and press **Enter**. Ollama requires Windows 10 22H2 or newer. Allow at least 4 GB of disk space for the app plus about 1 GB for the model. [Windows requirements](https://docs.ollama.com/windows) · [Model download size](https://ollama.com/library/qwen2.5:1.5b)
2. Download [Ollama for Windows](https://ollama.com/download/windows). Open **Downloads**, double-click `OllamaSetup.exe`, and follow the installer. Ollama runs directly on Windows, so you can use PowerShell.
3. Open **Ollama** from the Start menu if it hasn’t already started. Leave it running.
4. Open Start, search for **PowerShell**, and open a new window.
5. Paste this command, press **Enter**, and wait for the download to finish.

   ```powershell
   ollama pull qwen2.5:1.5b
   ```

6. Start a chat. When you see `>>>`, paste the first prompt below. Type `/bye` to exit.

   ```powershell
   ollama run qwen2.5:1.5b
   ```

If PowerShell says `ollama` isn’t recognized, close it and open a fresh window after installation. If the command can’t connect, launch Ollama from Start and try again. [CLI reference](https://docs.ollama.com/cli)

### Option B — LM Studio with a chat window

1. Check **Settings → System → About → System type**. LM Studio supports x64 PCs with AVX2 and supported Snapdragon ARM systems. It recommends 16 GB of RAM and 4 GB of dedicated GPU memory. [LM Studio requirements](https://lmstudio.ai/docs/app/system-requirements)
2. Get the Windows installer that matches your system from [LM Studio](https://lmstudio.ai/download). Run it, follow the prompts, and open LM Studio from Start.
3. In **Discover**, search for `Qwen2.5-1.5B-Instruct`. Download a **GGUF** variant labeled **Q4_K_M** if available.
4. Open **Chat**, select the downloaded model from the model loader, and wait for it to load. Paste the first prompt below. [LM Studio setup guide](https://lmstudio.ai/docs/app/basics)

## Demo prompts

Paste this in the Ollama terminal after `>>>` or in the LM Studio chat box.

```text
Using only these notes, write three short study questions and an answer key. DHCP assigns IP addresses. DNS maps names to IP addresses. A UPS provides temporary backup power.
```

Ask the room whether the notes say how long the UPS will last. Then send this in the **same chat**.

```text
Based only on the notes, how long will the UPS power the server?
```

The notes give no runtime. If the model gives a number, ask someone to find it in the source. If the model says it needs more information, ask the room what would help, such as the UPS model and the server’s power draw.

Before the talk, rehearse both prompts offline on the machine you’ll present from. Save a screenshot of the actual output as a fallback and open a fresh chat for the live demo. Attendees can vote on the answer from their seats.