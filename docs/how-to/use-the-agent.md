# How-To: Use the `cx` Agent

The `cx` shell's integrated agentic capabilities can dramatically accelerate your workflows, from generating complex commands to (in a future release) solving multi-step problems. This guide will show you how to get started.

### Prerequisites

To use the agentic features, you need a connection to a Large Language Model (LLM) provider. The agent is provider-agnostic, but the default configuration is set up for OpenAI.

If you haven't already, you will need an API key from an LLM provider. The first time you use an agentic feature, `cx` will guide you through a seamless, one-time setup to configure your connection.

### Your First Translation with `//`

The fastest way to use the agent is with the "Translate" feature. This allows you to write a goal in plain English, and `cx` will translate it into a precise command for you.

1.  **Start the `cx` shell.**

2.  **Write your goal as a comment.** Start the line with `//` and describe what you want to do.

    ```
    cx> // list all my saved SQL queries
    ```

3.  **Press Enter.**

    - If this is your first time, `cx` will trigger a **one-time setup wizard** to configure your OpenAI connection. Simply follow the prompts to enter your API key.
    - The shell will show a "Translating..." status.
    - The line you just typed will be **replaced in-place** with the correct `cx` command.

    ```
    cx> query list
    ```

    The command is now in your prompt, with your cursor at the end, ready for you to review or execute.

4.  **Press Enter again** to run the command.

This workflow is perfect for learning the shell's syntax or for quickly generating complex commands with pipelines and formatters. For example:

```
// from my repos variable, show a table of python repos, with just the name and star count
```

### v0.5.0 Preview: Solving Complex Problems with `agent`

The next major release will activate the full reasoning engine, allowing you to tackle complex problems with the `agent` command. The experience will be a collaborative, turn-by-turn dialogue.

**Example of a Future Interaction:**

```
cx> agent "The checkout-api is slow. Find out what's going on."

[agent] Thinking: I will start by checking for a recent spike in error logs.
[agent] Plan:
        [ ] 1. Check for recent error log spikes in Loki.
        [ ] 2. Check for high CPU/Memory usage in Prometheus.

[agent] Next Step: I will query Loki for error logs from the 'checkout-api'.
> loki.query('{pod=~"checkout-api-.*"} | logfmt | level="error"')

Execute? [Y/n] >
```

This powerful, deliberate reasoning mode will enable `cx` to act as a true co-pilot for your most critical operational tasks.
