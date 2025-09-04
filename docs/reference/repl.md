# Reference: Interactive Shell (cx>)

The `cx` interactive shell (REPL) is a powerful, stateful environment for exploring APIs, managing workspace assets, and building data workflows. This document is the definitive reference for its language and syntax, used at the `cx>` prompt.

!!! tip "Looking for Setup or Scripting Commands?"
For a guide to the non-interactive CLI commands like `cx init`, `cx compile`, and running automated scripts from your OS terminal, please see the [**CLI Commands Reference**](cli.md).

---

## Core Syntax & Operators

| Syntax                  | Example                                    | Description                                                 |
| :---------------------- | :----------------------------------------- | :---------------------------------------------------------- |
| `<alias>.<action>(...)` | `gh.getUser(name="google")`                | Execute a blueprint-defined action on an active connection. |
| `<var> = <command>`     | `repos = gh.read(...)`                     | Assign the result of a command to a session variable.       |
| `<cmd> \| <cmd>`        | `flow run ... \| script run ...`           | Pipe the output of one command to the input of the next.    |
| `(...)`                 | `(repos \| script run ...) --output table` | Group commands to control the order of operations.          |

---

## Universal Output Formatters

These flags can be added to the end of any data-producing command or pipeline to control how the final result is displayed.

| Flag               | Example                                | Description                                                                 |
| :----------------- | :------------------------------------- | :-------------------------------------------------------------------------- |
| `--output table`   | `... --output table`                   | Renders the output as a formatted table (if the data is a list of objects). |
| `--columns <cols>` | `... --output table --columns name,id` | Selects specific columns for table view.                                    |
| `--query <expr>`   | `... --query "[?stars > 100]"`         | Filters or reshapes data using a JMESPath expression before rendering.      |

---

## Workspace Management Commands

These commands allow you to manage your entire workspace without leaving the shell.

!!! tip "Command Aliases"
Many commands have short aliases for convenience (e.g., `var list` can also be `vars list`). The reference shows the primary form.

???+ note "`session`: Manage persistent workspace sessions."

    | Command | Description |
    | :--- | :--- |
    | `session list` | Lists all saved session files from `~/.cx/sessions`. |
    | `session save <name>` | Saves the current session (active connections and variables) to a file. |
    | `session load <name>` | Replaces the current session with a saved one. |
    | `session rm <name>` | Deletes a saved session file. |
    | `session status` | Displays a summary of the current in-memory session. |

???+ note "`var` / `vars`: Manage in-memory session variables."

    | Command | Description |
    | :--- | :--- |
    | `var list` | Lists all variables in the current session with a summary table. |
    | `var rm <name>` | Deletes a variable from the current session. |

???+ note "`flow`: Manage reusable `.flow.yaml` workflows."

    | Command | Description |
    | :--- | :--- |
    | `flow list` | Lists all available flows from `~/.cx/flows/`. |
    | `flow run <name> [key=value ...]`| Executes a flow, passing arguments to its `script_input`. |

???+ note "`query`: Manage reusable `.sql` queries."

    | Command | Description |
    | :--- | :--- |
    | `query list` | Lists all available queries from `~/.cx/queries/`. |
    | `query run --on <alias> <name> [key=value ...]` | Executes a query on an active connection. |

???+ note "`script`: Manage reusable `.py` scripts."

    | Command | Description |
    | :--- | :--- |
    | `script list` | Lists all available scripts from `~/.cx/scripts/`. |
    | `script run <name> [key=value ...]` | Executes a Python script. |

???+ note "`connection`: Manage connection configuration files."

    | Command | Description |
    | :--- | :--- |
    | `connection list` | Lists all saved connection configuration files. |
    | `connection create --blueprint "<id>"` | Starts an interactive wizard to create a new connection file. |

???+ note "`open`: Open workspace assets in external applications."

    | Command | Description |
    | :--- | :--- |
    | `open <type> [name]` | Opens an asset (e.g., `open flow my-flow`). Types: `flow`, `query`, `script`, `connection`, `config`. |
    | `open {{ url_variable }}` | Opens a URL from a variable in a web browser. |
    | `... --in <handler>` | (Optional) Opens with a specific handler (e.g., `--in vscode`). |

???+ note "`inspect`: Introspect session variables."

    | Command | Description |
    | :--- | :--- |
    | `inspect <variable>` | Displays a detailed summary panel of a session variable. |

???+ note "`app`: Discover and manage installable applications."

    | Command | Description |
    | :--- | :--- |
    | `app list` | Lists all applications currently installed in your workspace. |
    | `app install <id\|url>` | Installs a new application from the public registry or a private URL. |
    | `app uninstall <id>` | Uninstalls an application and removes its assets. |
    | `app sync` | Refreshes and installs any missing blueprint dependencies for all your applications. |
