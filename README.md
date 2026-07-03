# llama-deck

Lightweight web admin panel for [llama.cpp](https://github.com/ggerganov/llama.cpp) server management.

## Features

- **Model Browser** — Scan local directory for `.gguf` files, view name/size/modified
- **Model Download** — Search HuggingFace for GGUF models, download with progress tracking
- **Server Lifecycle** — Start/stop/restart `llama-server` subprocess from the browser
- **Parameter Config** — Visual editor for llama-server flags (context size, GPU layers, batch size, flash attention, etc.)
- **GPU Monitoring** — Real-time VRAM usage, temperature, utilization via `nvidia-smi`
- **Log Viewer** — Stream llama-server stdout/stderr via Server-Sent Events
- **Health Monitoring** — Poll `/health` endpoint, show status badge

## Install

```bash
pip install llama-deck
```

## Quick Start

```bash
# Start the admin panel
llama-deck --host 0.0.0.0 --port 7860

# With custom config
llama-deck --config /path/to/config.json
```

Then open `http://localhost:7860` in your browser.

## Configuration

Config is stored at `~/.config/llama-deck/config.json`:

```json
{
  "llama_server_path": "/opt/llama.cpp/build/bin/llama-server",
  "models_dir": "/mnt/data/models",
  "default_args": {
    "host": "0.0.0.0",
    "port": 8080,
    "n_gpu_layers": 99,
    "ctx_size": 8192,
    "flash_attn": true,
    "batch_size": 2048,
    "ubatch_size": 512,
    "threads": 0,
    "parallel": 1,
    "cont_batching": true,
    "metrics": true
  }
}
```

## Docker

```bash
# Build
docker build -t llama-deck .

# Run (with GPU access for nvidia-smi monitoring)
docker run --gpus all -p 7860:7860 \
  -v /mnt/data/models:/mnt/data/models \
  -v ~/.config/llama-deck:/root/.config/llama-deck \
  llama-deck
```

## Tech Stack

- **Backend**: Python asyncio (zero-framework, vendored HTTP server)
- **Frontend**: Single-file vanilla HTML/CSS/JS
- **Dependencies**: `httpx`, `huggingface-hub` only
- **No**: Flask, FastAPI, React, npm, database

## License

MIT
