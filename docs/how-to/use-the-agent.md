# How-To: Use the `cx` Agent

The `cx` shell's integrated agentic capabilities can dramatically accelerate your workflows, from generating complex commands in an instant to solving multi-step problems autonomously. This guide will show you how to leverage both modes of the `cx` Intent Engine.

### Prerequisites

To use the agentic features, you need a connection to a Large Language Model (LLM) provider. The agent is provider-agnostic, but the default configuration is set up for OpenAI.

If you haven't already, you will need an API key from an LLM provider. The first time you use an agentic feature, `cx` will guide you through a seamless, **one-time setup wizard** to configure your connection.

---

### Fast Path: Instant Translation with `//`

The fastest way to use the agent is with the "Translate" feature. This allows you to write a goal in plain English, and `cx` will translate it into a precise, executable command for you. It's perfect for learning the shell's syntax or for commands you don't use often.

1.  **Start the `cx` shell.**

2.  **Write your goal as a comment.** Start the line with `//` and describe what you want to do.

    ```
    cx> // list all my saved SQL queries
    ```

3.  **Press Enter.**

    - If it's your first time, the one-time setup wizard for your LLM provider will launch.
    - The shell will show a "Translating..." status.
    - The line you just typed will be **replaced in-place** with the correct `cx` command.

    ```
    cx> query list
    ```

    The command is now in your prompt, ready for you to review, edit, or execute.

4.  **Press Enter again** to run the command.

---

### Deliberate Path: Solving Problems with `agent`

For more complex, multi-step tasks, you can engage the full reasoning engine with the `agent` command. This initiates a collaborative, turn-by-turn session where the agent formulates a plan and executes it with your approval.

**Example: Onboarding a new API Blueprint**

Let's ask the agent to perform a common, multi-step task: compiling a new blueprint for the Spotify API.

1.  **Invoke the agent with a high-level goal.**

    ```
    cx> agent "Onboard the Spotify API. The OpenAPI spec is at https://.../openapi.yaml"
    ```

2.  **The agent presents its plan.** The Planner agent analyzes your goal and creates a high-level strategy using `cx` meta-commands.

    ```
                                     Agent Plan
    ╭──────────┬──────────────────────────────────────────────────────────────────╮
    │  Status  │ Step                                                             │
    ├──────────┼──────────────────────────────────────────────────────────────────┤
    │ Pending  │ 1. Use the `compile` command to generate a blueprint from the    │
    │          │    provided URL.                                                 │
    │ Pending  │ 2. Use the `connection create` command to set up a new           │
    │          │    connection for the newly created blueprint.                   │
    ╰──────────┴──────────────────────────────────────────────────────────────────╯
    ```

3.  **The agent executes the plan step-by-step.** The Tool Specialist agent now takes over, generating the precise command for the first step and presenting it for your approval.

    ```
    Executing Step 1: Use the `compile` command...

    ╭────────────────────────────────── Agent Plan ───────────────────────────────────╮
    │ Reasoning: The active step is to compile the blueprint. I will use the URL      │
    │ from the original goal...                                                       │
    │                                                                                 │
    │ Next Command:                                                                   │
    │ > compile --spec-url https://.../openapi.yaml --name spotify --version 1.0.0    │
    │                                                                                 │
    │ 🦾 Dry Run Preview:                                                             │
    │    ✓ Command is syntactically valid.                                            │
    ╰─────────────────────────────────────────────────────────────────────────────────╯
    Execute? [Yes/no/edit]:
    ```

4.  **Confirm and Collaborate.** You press `yes`, the command runs, and the agent proceeds to the next step, continuing this loop until the mission is accomplished.

This powerful, deliberate reasoning mode enables `cx` to act as a true **intelligent assistant** for your most critical operational tasks.
