# Local Development & Running

This guide describes how to run and develop your agents locally using `agents-cli`. Local development is fast, supports hot-reloading, and doesn't require cloud infrastructure for initial prototyping.

---

## 1. Installation

There are two main packages to install:

### The CLI Tool
The `agents-cli` manages your project lifecycle.
```bash
uvx google-agents-cli setup
```

### The SDK Library
The `google-adk` library provides the core classes like `Agent` and `Gemini`.
```bash
pip install google-adk
```

> **Note**: If you are using a scaffolded project, `agents-cli install` will handle the SDK installation for you automatically.

---

## 2. Local Setup

Once you have scaffolded a project with `agents-cli create`, ensure all local dependencies are installed.

```bash
agents-cli install
```

This command uses `uv` to create a virtual environment and install the necessary ADK and project-specific packages.

---

## 3. Running your Agent

There are two primary ways to interact with your agent locally:

### Direct CLI Run
For quick smoke tests or automated scripts, use the `run` command:

```bash
agents-cli run "Summarize the latest project updates"
```

### Interactive Web Playground (GUI)
The playground provides a rich, chat-like interface to test your agent, inspect tool calls, and view trace information in real-time.

```bash
agents-cli playground
```

- **URL**: `http://localhost:8080`
- **Features**: Hot-reload (changes to `app/agent.py` are reflected instantly), conversation history, and detailed execution logs.

### Example: Build & Run a Hello World Agent

Follow these steps to quickly see your agent in the local GUI:

1. **Scaffold a new project**:
   ```bash
   agents-cli create hello-world --prototype --yes
   cd hello-world && agents-cli install
   ```

2. **Define the agent logic**:
   Open `app/agent.py` and replace the content with a simple greeting agent using the core ADK imports:
   ```python
   from google.adk.agents import Agent
   from google.adk.models import Gemini
   from google.adk.apps import App

   root_agent = Agent(
       name="hello_agent",
       model=Gemini(model="gemini-flash-latest"),
       instruction="You are a helpful assistant. Always start by saying 'Hello! I am your new agent.'",
   )

   app = App(root_agent=root_agent, name="app")
   ```

3. **Launch the Playground GUI**:
   ```bash
   agents-cli playground
   ```

4. **Interact**:
   Open [http://localhost:8080](http://localhost:8080) in your browser. You will see a chat interface. Type a message like "Who are you?", and your agent will respond using the logic you just defined.

---

## 4. Troubleshooting

### `404 NOT_FOUND` (Model Name Issues)
If you get a 404 error saying a model like `gemini-1.5-flash` is not found:

1. **Use Alias Names**: Try using `gemini-flash-latest` which is an alias that points to the most stable version of Flash across both Vertex AI and AI Studio.
2. **Check Location**: If using Vertex AI, ensure your `GOOGLE_CLOUD_LOCATION` is set correctly (e.g., `us-central1`).
3. **Verify API Key**: If using AI Studio, ensure your `GEMINI_API_KEY` is exported and valid.

### `ModuleNotFoundError: No module named 'adk'`
If you encounter this error when running your agent, it means the Python environment doesn't have the Agent Development Kit (ADK) installed.

**Solutions:**
1. **Always use `agents-cli` commands**: Instead of `python app/agent.py`, use:
   ```bash
   agents-cli run "prompt"
   # or
   agents-cli playground
   ```
   These commands automatically detect and use the virtual environment managed by `uv`.

2. **Run via `uv`**: If you must use a standard command, prefix it with `uv run`:
   ```bash
   uv run python app/agent.py
   ```

3. **Re-install dependencies**: Ensure `agents-cli install` completed successfully.
   ```bash
   agents-cli install
   ```

---

## 5. Local Evaluation

Validating your agent's quality is best done locally before any deployment.

```bash
# Run all evaluation sets defined in tests/eval/
agents-cli eval run
```

Evals use a local orchestrator but may call remote LLMs (like Gemini Flash) for the agent logic and judging.

---

## 6. Testing & Quality Control

Maintain code quality using built-in linting and unit testing tools.

### Linting & Formatting
Automatically fix formatting and check for common Python errors:

```bash
agents-cli lint
```

### Unit & Integration Tests
Run your standard test suite:

```bash
uv run pytest tests/unit
uv run pytest tests/integration
```

---

## 7. Summary of Local Commands

| Task | Command |
|------|---------|
| **Install Env** | `agents-cli install` |
| **Quick Test** | `agents-cli run "prompt"` |
| **Web UI** | `agents-cli playground` |
| **Run Evals** | `agents-cli eval run` |
| **Lint Code** | `agents-cli lint` |
| **Unit Tests** | `uv run pytest` |
