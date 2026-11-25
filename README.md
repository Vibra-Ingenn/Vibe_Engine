<p align="center">
  <img src="https://adeptuscamini.com/vibra-ingenn-wheel.jpeg" alt="Vibra-Ingenn (Vibe Engine) PRO" width="420">
</p>

# Vibra-Ingenn (Vibe Engine) PRO

**Software your AI pilots:** your AI writes a recipe (a short JSON list of steps), and Vibra-Ingenn (Vibe Engine) runs it the same way every time on your own PC. It is not an autonomous agent loop that re-sends the whole chat on every step.

> **This repository is documentation only.** The Vibe Engine runtime is closed source. Install it from [adeptuscamini.com/download.html](https://adeptuscamini.com/download.html).

Current release: **2.0.0**.

---

## What's new in 2.0.0

2.0.0 is a full new version: you can ask in plain words on Home, recipes handle more document types (including real Excel), miswritten steps are repaired or refused with a reason, and scheduled runs actually fire.

**Ask in plain words**

- Home asks “What do you want done?” — your AI writes the recipe, the engine checks and repairs it, then runs it.
- One **AI to use** list for every Janus, Ollama, and LM Studio model plus your online models; switch from the header.
- Janus on this PC counts as local; so does an AI server elsewhere on your home network.
- Separate settings for the AI you chat with and the AI that writes recipes.
- **What your AI did** lists every finished job on Home.

**Recipes do more**

- Run the same steps on every file in a folder (`for_each`).
- Real Excel (`.xlsx`) with trustworthy rows — the same bill is not added twice.
- Drop folders that trigger a recipe when a file lands.
- Read text, CSV, Word, and PDF (including damaged PDFs).
- Remember a value and report whether it is new, the same, or changed.
- Ask the AI several times and keep only what the answers agree on.
- Scheduled recipes actually run on time.

**Mistake handling**

- Self-healing repairs slightly wrong steps in code; clearly broken steps are refused with a reason.
- Self-correcting recipes send the model one named fix when a step fails.
- Repairs and refusals are logged on your PC.
- Local models that support format rules (Ollama, LM Studio) are steered to stay inside the recipe format.

**Dashboard**

- Calmer layout, plain error words, browse buttons, drag-and-drop, open-result-in-folder, add and save recipes from the editor, Inter font and built-in colours, dashboard in its own window again.

**Fixes that matter**

- Recipes survive uninstall and upgrade; scheduled jobs were broken and now run.

---

## Why it is not an agent

The AI writes the recipe once, then steps back. The model only comes back when a step explicitly asks it to. Pure-execution recipes can rerun with no further model calls; AI-in-the-loop recipes call a model only on the steps that say so. See [Our way vs agent loops](https://adeptuscamini.com/vs-agent-loops.html).

---

## Key features

- **Any-model chaining:** Chain local models (Ollama, LM Studio, Janus) with cloud models in one recipe — model-agnostic by design.
- **Privacy boundary:** Local steps can read private files; only what a recipe step sends onward reaches the cloud (see example below).
- **Self-fixing recipes:** When a step fails, the engine can show the AI what went wrong on your PC and let it correct that one step, with a limit before it asks you.
- **Ask a person mid-run:** Logins and CAPTCHAs open for you; you finish, confirm, and the recipe continues.
- **State and branching:** Remember values across runs; branch on success or failure; compare web pages for changes.
- **Recipe calls recipe:** One recipe can invoke another in order, like a small program made of files.
- **MCP for AI clients:** Works as an MCP server in Claude Desktop, Cursor, Cline, VS Code, and similar tools.

---

## Privacy boundary (triple-model audit)

The demo recipe `triple_model_private_audit` keeps source code on your PC: a local AI reads the file and passes on **only function signatures and one-line descriptions**. Cloud models threat-model and suggest fixes from those signatures — not from the full source. Change the file path in the recipe to audit your own code.

What stays local: the file read and the local model’s extraction step. What the cloud sees: only the text stored from that extraction step, sent in later HTTP steps.

---

## A short real recipe

Gets the current Bitcoin and Ethereum prices and adds one line to price_history.csv on your Desktop, so the history grows every time it runs. No AI needed — the engine writes the numbers itself (change the API address to track other prices).

```json
{
  "workflow_id": "competitor-price-tracker",
  "description": "Gets the current Bitcoin and Ethereum prices and adds one line to price_history.csv on your Desktop, so the history grows every time it runs. No AI needed: the engine writes the numbers itself. Change the address to track other prices.",
  "tasks": [
    {
      "id": "fetch_pricing",
      "trigger": "manual",
      "action_type": "http_request",
      "target": "https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum&vs_currencies=usd",
      "payload": {
        "method": "GET"
      },
      "output": {
        "store_as": "prices"
      },
      "next": "append_to_csv"
    },
    {
      "id": "append_to_csv",
      "trigger": "after:fetch_pricing",
      "action_type": "file_op",
      "target": "{{desktop_dir}}\\price_history.csv",
      "payload": {
        "operation": "append",
        "content": "{{datetime}},{{prices.bitcoin.usd}},{{prices.ethereum.usd}}\n"
      },
      "next": "notify_done"
    },
    {
      "id": "notify_done",
      "trigger": "after:append_to_csv",
      "action_type": "notify",
      "target": "stdout",
      "payload": {
        "message": "\u2713 Added today's prices to price_history.csv on your Desktop (Bitcoin {{prices.bitcoin.usd}}, Ethereum {{prices.ethereum.usd}})"
      }
    }
  ]
}
```

More examples: [Recipe library](https://adeptuscamini.com/recipes/).

---

## Install (Windows)

1. Download the installer from **[adeptuscamini.com/download.html](https://adeptuscamini.com/download.html)** (Windows 10 / 11).
2. Run `Setup_Vibra_Ingenn.exe`. If SmartScreen appears, choose **More info**, then **Run anyway**.
3. The dashboard opens in your browser; a new install includes a **7-day trial** with full features.

The installer is standalone — no Go, Python, or other dev tools required. Linux builds are not offered on the download page today.

After install, PRO keeps cloud AI and outbound web access after the trial; local recipes and files on your PC stay yours either way.

---

## Try it in your browser

**[adeptuscamini.com/playground](https://adeptuscamini.com/playground)** — run sample recipes against the public playground (topics such as research briefs and price checks; see the site recipe library for what ships there).

---

## Links

| | |
|---|---|
| Website | [adeptuscamini.com/vibra-ingenn.html](https://adeptuscamini.com/vibra-ingenn.html) |
| Download | [adeptuscamini.com/download.html](https://adeptuscamini.com/download.html) |
| Recipe library | [adeptuscamini.com/recipes/](https://adeptuscamini.com/recipes/) |
| Docs for AI tools | [adeptuscamini.com/llms.txt](https://adeptuscamini.com/llms.txt) |
| vs agent loops | [adeptuscamini.com/vs-agent-loops.html](https://adeptuscamini.com/vs-agent-loops.html) |
| Compare | [adeptuscamini.com/compare.html](https://adeptuscamini.com/compare.html) |

---

## Works with the rest of the toolbox

Vibe Engine can run **[Janus](https://github.com/Vibra-Ingenn/Janus)**, **[Terminus](https://github.com/Vibra-Ingenn/Terminus)**, **[Vox Vena](https://github.com/Vibra-Ingenn/VoxVena)**, **[VoxStilus](https://github.com/Vibra-Ingenn/VoxStilus)** (transcription), and other tools together — one recipe, same steps every time. All from [Adeptus Camini](https://adeptuscamini.com).

---

## Like it?

If the docs help you explain what Vibe Engine does, a star on this repo helps people find it.

---

<p align="center"><i lang="la" title="We refuse to accept the same broken things, we build differently!">Recusamus eadem fracta accipere. Nos aliter aedificamus.</i><br><sub>We refuse to accept the same broken things, we build differently!</sub></p>