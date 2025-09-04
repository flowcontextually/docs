# Reference: CLI Commands (`$`)

This document is the official reference for the `cx` command-line interface. These commands are run from your standard operating system terminal (like Bash or PowerShell) and are primarily used for setup, automation, and non-interactive workflows.

!!! tip "Looking for the Interactive Shell?"
For a detailed guide to the powerful interactive REPL (`cx>`), including pipelines, variables, and session management, please see the [**Interactive Shell (REPL) Reference**](repl.md).

---

## Global Options

These options can be used with the top-level `cx` command or any of its subcommands.

| Option            | Description                                           |
| ----------------- | ----------------------------------------------------- |
| `--help`          | Show help messages for any command.                   |
| `--verbose`, `-v` | Enable verbose DEBUG logging for detailed tracebacks. |

---

## Top-Level Commands

| Command                   | Description                                                                                        |
| :------------------------ | :------------------------------------------------------------------------------------------------- |
| `cx`                      | Starts the interactive REPL (`cx>`).                                                               |
| `cx init`                 | Initializes the `~/.cx` workspace directory. This is the recommended first step for all new users. |
| `cx compile <src> [opts]` | Compiles an API specification (like OpenAPI) into a Contextually blueprint package.                |

---

## Command Group: `cx connection`

Manages the connection configuration files stored in `~/.cx/connections/`.

| Command                       | Description                                                                                                                  |
| :---------------------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| `cx connection list`          | Lists all locally saved connection configuration files.                                                                      |
| `cx connection create [opts]` | Creates a new connection file. Run without options for an interactive wizard, or provide flags for non-interactive creation. |

---

### Command Groups: `cx extract` & `cx transform`

These commands are the workhorses for automated scripting. They are designed to be used with standard OS pipes (`|`) to chain workflows together.

| Command            | Example Usage                                      | Description                                                                      |
| :----------------- | :------------------------------------------------- | :------------------------------------------------------------------------------- |
| `cx extract run`   | `... \| cx extract run --script get-data.yaml`     | Executes a declarative `.connector.yaml` script.                                 |
| `cx transform run` | `... \| cx transform run --script shape-data.yaml` | Executes a declarative `.transformer.yaml` script, transforming data from stdin. |
