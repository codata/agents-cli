# Local Development & Running

This guide describes how to run and develop your agents locally using `agents-cli`. Local development is fast, supports hot-reloading, and doesn't require cloud infrastructure for initial prototyping.

---

## 1. Local Setup

Once you have scaffolded a project with `agents-cli create`, ensure all local dependencies are installed.

```bash
agents-cli install
```

This command uses `uv` to create a virtual environment and install the necessary ADK and project-specific packages.

---

## 2. Running your Agent

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
   Open `app/agent.py` and replace the content with a simple greeting agent:
   ```python
   from adk import Agent, Gemini

   root_agent = Agent(
       name="hello_agent",
       model=Gemini(model="gemini-1.5-flash"),
       instruction="You are a helpful assistant. Always start by saying 'Hello! I am your new agent.'",
   )
   ```

3. **Launch the Playground GUI**:
   ```bash
   agents-cli playground
   ```

4. **Interact**:
   Open [http://localhost:8080](http://localhost:8080) in your browser. You will see a chat interface. Type a message like "Who are you?", and your agent will respond using the logic you just defined.

---

## 3. Local Evaluation

Validating your agent's quality is best done locally before any deployment.

```bash
# Run all evaluation sets defined in tests/eval/
agents-cli eval run
```

Evals use a local orchestrator but may call remote LLMs (like Gemini Flash) for the agent logic and judging.

---

## 4. Testing & Quality Control

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

## 5. Summary of Local Commands

| Task | Command |
|------|---------|
| **Install Env** | `agents-cli install` |
| **Quick Test** | `agents-cli run "prompt"` |
| **Web UI** | `agents-cli playground` |
| **Run Evals** | `agents-cli eval run` |
| **Lint Code** | `agents-cli lint` |
| **Unit Tests** | `uv run pytest` |
