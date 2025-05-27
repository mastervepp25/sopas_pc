Sopas PC  |  Private, Local AI Agent

<p align="center">
  <img src="https://aiteksoftware.site/sopas-pc/logo.png" width="280" alt="Sopas PC Logo" />
</p>> Sopas PC is a 100 % local, privacy‑first AI assistant that can browse, code, plan, and talk – entirely on your own machine. No cloud, no telemetry, full data sovereignty.




---

📚 Table of Contents

1. Features


2. Architecture Overview


3. Requirements


4. Installation

Automatic Setup

Manual Setup



5. Running Sopas PC


6. Configuration


7. Voice Interface (Optional)


8. Directory Structure


9. Roadmap


10. Contributing


11. Community & Support


12. License




---

✨ Features

100 % Local Processing – All LLM inference and vector storage stay on‑device.

Autonomous Web Browser – Parse pages, follow links, fill forms, and scrape data using headless Chromium.

Multi‑Language Code Generation – Python, JS/TS, Bash, Rust, Go, and more.

Goal‑Directed Planning – Hierarchical task decomposition with memory and retry logic.

Voice I/O – Whisper‑style STT + TTS via Piper or ElevenLabs.

Extensible Tools API – Plug‑in new skills (e.g., Home Assistant control, file search, or proprietary APIs).

Cross‑Platform – Tested on Ubuntu 22/24, macOS 13+, and Windows 11.



---

🏗️ Architecture Overview

flowchart TD
    subgraph User Machine
        UI["CLI / Web UI / Voice"]
        UI -->|REST / CLI| Core(Core Orchestrator)
        Core --> LLM[[Local LLM (Ollama / llama.cpp)]]
        Core --> Tools((Tools Registry))
        Tools --> Browser(Headless Browser)
        Tools --> FS[Filesystem IO]
        Tools --> Voice[STT / TTS]
    end

Core = a thin task runner that routes prompts to the local LLM, invokes tools, manages state, and streams the response back to whichever interface you launched.


---

🛠️ Requirements

Component	Minimum Version	Notes

Python	3.10	3.12 supported
Node.js	18 LTS	needed for the Web UI build
Git	any	
nix utils	bash, curl, gzip	
GPU (optional)	NVIDIA RTX 2060 +	for fp16 or quantised models
Ollama	0.1.30+	or other local LLM server like llama.cpp
Chrome/Chromium	latest	+ matching ChromeDriver


> Windows users: install [WSL 2] and enable GPU compute for best performance.




---

📦 Installation

Automatic Setup

1. Clone the repo & enter it

git clone https://github.com/mastervepp25/sopas-pc.git
cd sopas-pc


2. Copy the env template

cp .env.example .env


3. Run the one‑liner installer (Linux / macOS)

chmod +x install.sh && ./install.sh

On Windows (PowerShell):

./install.bat



Manual Setup

python3.10 -m venv sopas_env
source sopas_env/bin/activate          # Windows: sopas_env\\Scripts\\activate
pip install -r requirements.txt        # core Python deps
# -- Install Ollama ----------------------------------------------------------
curl https://ollama.ai/install.sh | sh   # or brew install ollama
# ---------------------------------------------------------------------------
# -- Install ChromeDriver ----------------------------------------------------
# ensure the version matches your installed Chrome/Chromium
sudo apt install chromium-driver        # Ubuntu example

See docs/INSTALL.md for distribution‑specific details (Fedora, Arch, etc.).


---

🏃‍♂️ Running Sopas PC

source sopas_env/bin/activate
# start background agents (vector DB, browser pool, etc.)
sudo ./start_services.sh        # Linux / macOS
start ./start_services.cmd      # Windows

# CLI mode
python cli.py                   # interactive shell

# Web mode
python api.py                   # REST + WebSocket server
# browse http://localhost:3000/

Quick Demo

> ask "Write a Bash script that finds the five largest files in /var and prints their sizes."


---

⚙️ Configuration

All runtime settings live in config.ini — edit and restart.

[MAIN]
agent_name           = Sopas
work_dir             = /home/$USER/sopas_workspace

[LLM]
provider_name        = ollama
model               = deepseek-r1:14b
server_address       = 127.0.0.1:11434
ctx_window           = 8192

[VOICE]
enable_voice         = false       ; true to activate speech layer
stt_backend          = whispercpp
...

Environment variables in .env override the INI when present.


---

🎙️ Voice Interface (Optional)

1. Install Piper (sudo apt install piper-tts) or sign‑up for ElevenLabs.


2. Flip enable_voice=true in config.ini.


3. Launch with python voice_cli.py.




---

📁 Directory Structure

├── api.py            # FastAPI server (Web UI & REST)
├── cli.py            # REPL shell
├── core/             # agent orchestrator & planning engine
├── tools/            # built‑in tool plugins
├── voice/            # STT / TTS helpers
├── installs/         # platform‑specific installers
├── docs/             # extended documentation
└── start_services.*  # service supervisor scripts


---

🔭 Roadmap

[ ] Multi‑agent collaboration (Sopas PC "teams")

[ ] GUI prompt designer (Electron‑based)

[ ] Auto‑update mechanism for local models

[ ] Mobile companion app (Flutter)



---

🤝 Contributing

1. Fork → Branch → PR (please write clear commit messages).


2. Run pre‑commit run --all-files before pushing.


3. New tool integrations should ship with unit tests in tests/.




---

💬 Community & Support

Discussions: https://github.com/mastervepp25/sopas-pc/discussions

Issues: please search first, then open a detailed ticket.

Email: sopas‑support@aiteksoftware.site



---

📄 License

Sopas PC is released under the GNU GPL v3. You are free to use, modify, and redistribute under the same terms.


---

> Built with  ❤️ by Aitek PH Software



