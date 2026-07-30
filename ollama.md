Ollama is a free, open-source tool that allows you to run large language models (LLMs) locally on your Mac, Windows, or Linux machine.

It bundles model weights, configurations, and system prompts into a unified package called a "Modelfile." This setup completely eliminates the complex environment configurations typically required to run open-source AI models locally.

---

## 1. Running & Managing Models

Commands for downloading, launching, and managing local LLMs.

* **`ollama run <model>`**: Downloads the model (if not already local) and launches an interactive CLI chat session (e.g., `ollama run llama3.1`).
* **`ollama run <model> "<prompt>"`**: Executes a single prompt in one line without entering interactive mode.
* **`ollama pull <model>`**: Downloads or updates a model without immediately starting a chat session.
* **`ollama list`** (or `ollama ls`): Displays all locally installed models, their tags, size, and unique IDs.
* **`ollama ps`**: Shows currently loaded/active models in memory (VRAM/RAM) along with processor breakdown (GPU vs CPU).
* **`ollama rm <model>`**: Deletes a model from local storage to free up disk space.
* **`ollama show <model>`**: Displays details about a model (license, system prompt, parameters, and architecture).
* `ollama show --modelfile <model>`: Prints the exact `Modelfile` used to build the model.



---

## 2. Building & Customizing Models

Commands for creating tailored models using a **Modelfile** (similar to a Dockerfile).

* **`ollama create <name> -f ./Modelfile`**: Builds a new custom model from a local `Modelfile`.
* **`ollama copy <source> <target>`**: Creates a duplicate/alias of an existing model.
* **`ollama push <namespace>/<model>`**: Uploads a custom model to the public/private Ollama registry.

---

## 3. Serving & System Commands

Commands for controlling the background service and daemon process.

* **`ollama serve`**: Starts the HTTP REST API server on `http://localhost:11434` (usually runs automatically as a background service).
* **`ollama stop <model>`**: Unloads a currently running model from memory (RAM/VRAM) to free up system resources.
* **`ollama --version`**: Shows the installed version of the Ollama CLI and engine.

---

## 4. Interactive CLI Shortcuts

When you are inside an active interactive chat (`ollama run <model>`), use these shortcuts:

| Shortcut | Description |
| --- | --- |
| `/?` or `/help` | Shows available interactive commands. |
| `/set system "<prompt>"` | Overrides the default system prompt for the current session. |
| `/set parameter <param> <val>` | Adjusts parameters on the fly (e.g., `/set parameter temperature 0.2`). |
| `/show info` | Displays parameters and details for the currently active session. |
| `/? multiline` | Toggles multiline input mode for pasting long prompts. |
| `/clear` | Clears the current conversation context. |
| `/bye` (or `Ctrl+D`) | Exits the interactive session. |

---

## 5. Useful Environment Variables

You can pass these environment variables before starting the Ollama service (`ollama serve` or system service) to customize behavior:

* **`OLLAMA_HOST=0.0.0.0:11434`**: Binds Ollama to listen on all network interfaces instead of just `localhost` (useful for LAN access).
* **`OLLAMA_MODELS=/path/to/folder`**: Changes the default directory where downloaded models are stored.
* **`OLLAMA_KEEP_ALIVE=24h`**: Keeps the model loaded in GPU memory for a specified duration (default is `5m`). Setting it to `-1` keeps it loaded indefinitely.
* **`OLLAMA_NUM_PARALLEL=4`**: Sets how many simultaneous requests Ollama can process at once.

---

> **Quick Modelfile Example**
> Create a file named `Modelfile`:
> ```dockerfile
> FROM llama3.1
> PARAMETER temperature 0.3
> SYSTEM "You are a concise, ultra-direct coding assistant."
> 
> ```
> 
> 
> Then run:
> ```bash
> ollama create my-coder -f ./Modelfile
> ollama run my-coder
> 
> ```