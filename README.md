# 🛡️ Codex Endless Research Loop

> **Turn OpenAI Codex into a 24/7 autonomous auditor.**

This repository contains a specialized **"Recursive Research Loop"** designed for the **OpenAI Codex CLI**. By feeding the contents of `loop.md` into your base prompt, you trigger a long-lived, autonomous, and **read-only** audit cycle that documents your codebase indefinitely.

## 🚀 The "Endless" Concept

Most AI interactions are one-off tasks. This setup changes the paradigm:
1. **The Engine (`loop.sh`)**: A bash wrapper that maintains the execution environment, handles authentication overrides, and ensures the process restarts if interrupted.
2. **The Logic (`loop.md`)**: The "brain" of the operation. When appended to your prompt, it instructs Codex to:
    - Scan the repository for the next unresolved "story" or "audit task."
    - Execute a deep-dive research session (optionally with web search).
    - Write a detailed report.
    - **Self-Trigger**: Mark the task as complete and immediately move to the next, creating a 24-hour productivity cycle.

---

## 📂 Core Files

| File | Role | Description |
| :--- | :--- | :--- |
| **`loop.sh`** | **The Runner** | Manages the CLI execution, sets `CODEX_INTERNAL_ORIGINATOR_OVERRIDE` for Desktop limits, and enforces `-s read-only` safety. |
| **`loop.md`** | **The Logic** | The recursive prompt. It contains the audit standards, file-reading rules, and the "Self-Perpetuating" instructions. |

---

## 🛠️ Setup & Prerequisites

- **Codex CLI**: Installed and authenticated (`codex auth login`).
- **Permissions**: The loop runs in **Read-Only** mode. It identifies problems but *never* modifies your source code.
- **Dependencies**: `jq` (for parsing task states) and `bash`.

---

## 📖 Usage: How to Start the Loop

The magic happens when you combine your project context with the `loop.md` logic.

### 1. Basic Execution
Run the shell script to start the autonomous cycle:
```bash
chmod +x loop.sh
./loop.sh
```

### 2. How it works internally
The script takes your base project prompt and appends the contents of `loop.md`. This tells the agent:
> *"Finish the current audit task, save the result to `audit/*.md`, and then look for the next task in the queue. Do not stop until the queue is empty."*

### 3. Monitoring the 24/7 Work
Since the agent works in the background, you can tail the logs to see what it's thinking:
```bash
# Watch high-level progress
tail -f events.log

# Watch the raw Codex reasoning and output
tail -f run.log
```

---

## ⚙️ Customization

### Tailoring the "Brain" (`loop.md`)
To change *how* the agent audits your code, edit `loop.md`:
- **Define Quality Gates**: Tell it to look for memory leaks, API inconsistencies, or lack of documentation.
- **Set Output Format**: Force the agent to write reports in a specific structure.

### Tuning the "Engine" (`loop.sh`)
- **Reasoning Effort**: Adjust the `REASONING_EFFORT` variable to balance between speed and deep architectural thinking.
- **Search**: Toggle `--search` to allow the agent to look up modern libraries or documentation online.

---

## ⚠️ Important Note on Usage
Because this loop is designed to run **24/7**, keep an eye on your API usage. This script utilizes the `Codex Desktop` origin override to take advantage of specific rate limits, but it will still consume tokens continuously as it "researches" your code.
