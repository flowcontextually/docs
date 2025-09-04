# Concepts: The Shell as a Workspace IDE

The `cx` interactive shell is designed around a core philosophy: your terminal can be more than just a command line; it can be a complete, persistent **Integrated Development Environment (IDE)** for your data and operations workflows.

This is achieved by treating your workspace and its contents as a set of first-class **assets** that you can create, manage, and compose.

---

## The Core Assets of Your Workspace

When you work in `cx`, you are interacting with a few key types of assets. Understanding their roles is key to mastering the shell.

### 1. The Session: Your Live Workspace

The "session" is the ephemeral, in-memory state of your current `cx>` prompt. It contains two main components:

- **Active Connections:** These are the live, authenticated links to your data sources (APIs, databases) that you create with `connect user:my-db --as db`. They are temporary shortcuts for your current work.
- **Session Variables:** These are the in-memory containers for your data that you create with the assignment operator (`my_data = ...`). They allow you to hold onto the results of a command and reuse them without having to run the command again.

The `session` command (`session save`, `session load`) allows you to make this ephemeral workspace **persistent**, so you can close your terminal and restore your exact context later.

### 2. The Library: Your Reusable Assets

While the session is your live workbench, the `~/.cx/` directory acts as your personal library of reusable tools. The shell provides dedicated commands to manage these file-based assets, so you rarely need to interact with the filesystem directly.

- **Flows (`.flow.yaml`):** These are the most powerful assets. A flow is a persistent, versionable, multi-step workflow. By using the `flow run` command, you can execute a complex series of actions with a single line. Because they can be parameterized, they act like reusable functions in your personal library.
- **Queries (`.sql`):** For data professionals, storing complex SQL queries as named assets is a huge productivity boost. The `query run` command allows you to execute these saved queries against any active connection, turning your SQL library into a set of on-demand data retrieval tools.
- **Scripts (`.py`):** For tasks that require the full power of Python, you can save `.py` scripts to your workspace. The `script run` command executes them in a sandboxed environment, allowing you to integrate custom logic and analysis directly into your shell pipelines.

### The `open` Command: Your Bridge

The `open` command acts as the seamless bridge between your `cx` IDE and your other desktop tools. Instead of manually navigating your filesystem to find the source file for a flow you want to edit, you can simply run:

```
cx> open flow my-daily-report

```

The shell understands what a "flow" is, finds the correct file, and opens it in your default text editor. This keeps you in the "flow state" of your work without context switching.

By combining the live session with a managed library of reusable assets, the `cx` shell provides a complete, end-to-end environment for building, testing, and executing your most common operational workflows.
