# How-To: Manage Background Processes

The `cx` shell includes a powerful process manager for running long, non-interactive tasks in the background. This allows you to kick off a heavy data processing flow and immediately get your interactive prompt back, while still being able to monitor the task's progress.

While background processes will primarily be launched by the `agent` command in the future (`v0.5.0`), you can learn to manage them now with the `cx process` command suite, available in both the CLI and the interactive REPL.

### When to Use a Background Process

A background process is ideal for any workflow that might take more than a few seconds to run, such as:

- Large database queries and data extraction jobs.
- Flows that make hundreds or thousands of API calls.
- Data transformation scripts that operate on large files.

### Listing Active Processes (`process list`)

To see all background processes you have started, use the `list` subcommand.

````

cx> process list

```

This will display a table showing the unique ID of each process, its current `Status` (`running`, `completed`, `failed`), the name of the flow it's executing, and when it was started.

### Viewing Logs (`process logs`)

Every background process streams its output (both standard output and errors) to a dedicated log file in your `~/.cx/logs/` directory. You can easily view this output with the `logs` subcommand.

**To view the full log of a completed or running process:**
```

cx> process logs proc-20250905-01

```

### Tailing Live Logs (`process logs --follow`)

The most powerful feature for monitoring is the ability to "tail" the log file in real-time, just like the `tail -f` command in Linux. This is perfect for watching the progress of a currently running job.

```

cx> process logs --follow proc-20250905-01

```
The shell will display the log output as it's written. You can press `Ctrl+C` at any time to stop following the log and return to the `cx>` prompt, without interrupting the background process itself.
```

---

#### **File 4: `docs/tutorials/first-agent-session.md` (New File)**

This tutorial provides a gentle, hands-on introduction to the new agentic features.

```markdown
# /home/dpwanjala/repositories/flowcontextually/docs/docs/tutorials/first-agent-session.md

# Tutorial: Your First Agentic Translation

Welcome to the next step in your Contextually journey! You've mastered the basics of running commands; now it's time to let the `cx` shell's built-in AI do the work for you.

This tutorial will guide you through using the new **Translate feature**. You will learn how to turn your plain-English goals into precise, executable commands without needing to remember the exact syntax.

**What you will learn:**

- How to use the `//` directive to trigger a translation.
- How the seamless, on-demand setup for AI providers works.
- How to translate a complex goal involving a pipeline.

### 1. Your First "Whisper" to `cx`

The "Translate" feature is triggered by starting your line with `//`, like a comment. This tells the shell, "Don't run this; translate it for me."

Let's try a simple one.
```

cx> // show me all the applications I have installed

```

Press **Enter**.

### 2. The One-Time Onboarding Magic

If this is the first time you've used an agentic feature, `cx` will now guide you through a one-time setup for your AI provider connection. The default is OpenAI.

1.  The shell will explain that it needs an AI provider connection.
2.  It will launch the interactive `connection create` wizard, pre-filled with the OpenAI blueprint.
3.  Follow the prompts. You'll be asked for:
    *   A friendly name (e.g., `My OpenAI Account`).
    *   A unique ID (you can accept the default).
    *   Your OpenAI API Key (starting with `sk-...`).
4.  Confirm to save the connection.

After you save, `cx` will automatically activate the connection for your current session and seamlessly resume your original translation request.

### 3. Review and Execute

After a brief "Translating..." message, the line in your prompt will be replaced with the correct command.

```

cx> app list

```

The shell has correctly translated your intent into the `app list` command. It's now waiting for your final confirmation. Press **Enter** again to run it.

### 4. Translating a More Complex Goal

The real power of the Translate feature shines when you have a more complex goal. Let's assume you have a session variable named `repos` containing a list of repository data.

Try translating this:
```

cx> // from my repos variable, show a table of javascript repos with more than 1000 stars

```

Press **Enter**. The shell will translate this complex request, including the pipeline, query, and formatters, into the correct one-liner:
```

cx> repos --query "content[?language=='JavaScript' && stargazers_count > `1000`]" --output table

```

**Congratulations!** You are now leveraging the `cx` Intent Engine to write powerful commands with ease. You can use this for any task, from simple lookups to building complex, multi-stage pipelines.
```
````
