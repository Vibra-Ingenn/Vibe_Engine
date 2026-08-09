# Vibra-Ingenn (Vibe Engine)

**A deterministic local execution runtime for AI agents. The AI writes a JSON recipe once, then a native Go engine runs it — locally, in 0–200ms, on any Windows machine.**

Website: **https://adeptuscamini.com**
Download: **https://adeptuscamini.com/download.html**

Vibra-Ingenn is the interop layer for multi-LLM agent workflows. It works as an MCP server for Claude Desktop, Cursor, Cline, Windsurf, and VS Code — and as a local REST/CLI runtime that can call out to any OpenAI-compatible LLM (Ollama, LM Studio, OpenRouter, OpenAI, Anthropic, Gemini, Groq, Together, Fireworks, vLLM, Jan, LocalAI).

> *This repository is documentation only. The Vibra-Ingenn source code is proprietary. Download the installer from adeptuscamini.com.*

---

## Why Vibra-Ingenn

Traditional AI agents (ReAct, LangChain, AutoGPT) burn tokens by re-sending the full conversation history at every step. By step 10 you're at 25,000+ accumulated tokens per task.

Vibra-Ingenn replaces the loop with a **single-pass workflow compiler**:

1. The AI writes a JSON recipe once (~500 tokens).
2. The Go runtime executes 1–50 steps locally in milliseconds.
3. The recipe is a durable file on disk — save it, re-run it, share it, commit it to git.

Result: **~98% fewer tokens per task**, **~180ms execution vs. 30–90s for cloud agents**, and small local models (Llama 3.2 3B, Qwen 2.5 3B, DeepSeek-R1 distills) work reliably because they only need to produce one JSON recipe — not survive a 10-step conversation.

---

## Install (Windows)

1. Download the installer: **https://adeptuscamini.com/download.html**
2. Run `Setup_Vibra_Ingenn.exe`
3. The installer auto-registers Vibra with Claude Desktop's MCP config
4. Launch from Start Menu or run `vibra.exe`

---

## Connect to your AI client

### Claude Desktop

Vibra auto-registers during install. If you need to add it manually, edit `%APPDATA%\Claude\claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "vibra-ingenn": {
      "command": "%LOCALAPPDATA%\\Vibra-Ingenn\\vibra.exe",
      "args": ["mcp"]
    }
  }
}
```

Restart Claude Desktop. The `vibra-ingenn` tool appears in Settings → Developer → MCP Servers.

### Cursor / Windsurf / VS Code # Zoo Code)

Add to your MCP settings:

```json
{
  "mcpServers": {
    "vibra-ingenn": {
      "command": "%LOCALAPPDATA%\\Vibra-Ingenn\\vibra.exe",
      "args": ["mcp"]
    }
  }
}
```

### Ollama / LM Studio (local LLM as caller)

Vibra auto-discovers Ollama (port 11434), LM Studio (1234), vLLM (8000), Jan (1337), and LocalAI (8080) on startup. No configuration required.

For a purpose-tuned local model, create the Vibra-Llama variant:

```bash
ollama create vibra-llama -f recipes/ai/Modelfile.vibra
ollama run vibra-llama "Create a file on my desktop"
```

---

## Quick start — one recipe, one command

Ask any connected AI: *"Use Vibra-Ingenn to write a file named report.txt to my desktop containing 'Hello from Vibe Engine'."*

Or run a recipe directly from the CLI:

```powershell
.\vibra run .\recipes\web\bitcoin_live_report.json
```

Vibra parses the JSON, executes the task graph, and returns results — no LLM re-calls between steps.

---

## What Vibra-Ingenn can do

| Action Type   | What it does |
|:--------------|:---|
| `file_op`     | Read, write, append, list, delete, exists — with type & date filters |
| `http_request`| GET/POST/PUT/PATCH/DELETE with headers, body, and optional HTML stripping |
| `run_script`  | Execute shell scripts (safe mode + whitelist gated) |
| `ai_call`     | Call any OpenAI-compatible LLM as a workflow step |
| `notify`      | stdout or webhook (Slack/Discord-compatible) |
| `transform`   | json_extract, html→text, uppercase/lowercase/trim, template rendering |
| `gui_op`      | Native Windows mouse/keyboard automation (user32.dll) |

Plus: conditional DAG branching (`on_success` / `on_failure`), per-task `run_if` guards, automatic retries, encrypted secrets vault, cron scheduling (`schedule:0 9 * * *`), webhook triggers, and cryptographic SHA-256 audit receipts.

---

## Pricing

- **PRO** — $8.99/month ($104.99/year) — Local AI execution, workspace inspector, REST API, hardware ID lock, security updates and product upgrades. **** Only Early Adopters who pay for a year will lock into this price **** 
- **free plans** are customer accounts that no longer wish to continue receiving service and will become local only. So only Ollama LMStudio will still work and ALL Cloud LLM's will no longer be available, plus security updates and product upgrades will no longer be available 

Free 7-day trial. Buy at **https://adeptuscamini.com**.

---

## License

Business Source License 1.1. Source code is proprietary. Redistribution of the installer is permitted for personal, non-commercial evaluation.

---

## Support & Contact

- Website: https://adeptuscamini.com
- Download: https://adeptuscamini.com/download.html
- Docs & guides: bundled inside the installer under `docs/`
- Support please email us at support@adeptuscamini.com 


*Vibra-Ingenn is developed and maintained by its legal Owner. © 2024–2026. All rights reserved.*
