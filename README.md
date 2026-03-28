

# 🛡️ Codex Endless Research Loop

> **Turn OpenAI Codex into a 24/7 autonomous auditor.**

This repository provides a **Recursive Research Loop** for the **OpenAI Codex CLI**. By appending the `loop.md` logic to your task definition, you trigger a long-lived, autonomous audit cycle that documents and researches your codebase indefinitely.

## 🚀 How to Use

To start the endless loop, follow these two steps:

### 1. Define Your Core Task
Prepare a base prompt that clearly defines the scope of work. For the loop to function effectively, your prompt **must** include:
* **Role Definition**: (e.g., "You are a Senior Security Auditor.")
* **Task Definition**: (e.g., "Analyze the repository for SQL injection vulnerabilities.")
* **Task Content**: The specific files or modules to be examined.
* **Acceptance Criteria**: **(Crucial)** Define exactly what the output should look like (e.g., "Generate a Markdown table with the filename, line number, and risk level").

### 2. Inject the Loop Logic
Append the contents of `loop.md` to the end of your base prompt. 

When you input this combined prompt into the Codex CLI, the agent will:
1.  Complete the current task based on your criteria.
2.  Save the results.
3.  **Self-Trigger**: Automatically scan for the next task or deeper context, running without interruption until the queue is exhausted.

---

## 📂 Core Files

| File | Role | Description |
| :--- | :--- | :--- |
| **`loop.sh`** | **The Runner** | A bash wrapper that manages CLI execution, handles `CODEX_INTERNAL_ORIGINATOR_OVERRIDE`, and ensures the process restarts if interrupted. |
| **`loop.md`** | **The Logic** | The recursive "brain." It contains instructions for task-switching and self-perpetuation. |

---

## 🛠️ Execution Example

Once your prompt is ready, run the shell script to begin the autonomous cycle:

```bash
chmod +x loop.sh
./loop.sh
```

The script appends `loop.md` to your project context, instructing the agent: 
> *"Finish the current audit task, save the result, and immediately move to the next. Do not stop."*

---

## ⚙️ Monitoring & Safety

* **Read-Only Safety**: The loop is configured with `-s read-only`. It identifies issues and writes reports but **never** modifies your source code.
* **Live Logs**: Since the agent works in the background, you can monitor its reasoning in real-time:
    ```bash
    # Watch high-level progress
    tail -f events.log

    # Watch raw Codex reasoning
    tail -f run.log
    ```

---

## ⚠️ API & Token Usage
Because this loop is designed to run **24/7**, it will consume tokens continuously. Monitor your usage dashboard regularly, even when using the `Codex Desktop` origin override.
