# Reference: CLI

This document is the official reference for the Contextually Shell (`cx`) command-line interface. It details all available commands, subcommands, and their options.

---

## Top-Level Command: `cx`

The main entrypoint for the application. Running `cx` with no subcommand starts the interactive REPL (Read-Eval-Print Loop), which is the primary, stateful way to use the shell.

**Usage:**

```bash
cx [OPTIONS] [COMMAND]
```

### Global Options

These options can be used with the top-level `cx` command or any of its subcommands.

| Option            | Description                                           |
| ----------------- | ----------------------------------------------------- |
| `--help`          | Show help messages for any command.                   |
| `--verbose`, `-v` | Enable verbose DEBUG logging for detailed tracebacks. |

---

## Top-Level Commands

These commands are available directly under `cx`.

### `cx init`

Initializes the `cx` shell environment in your home directory (`~/.cx`).

This is the **recommended first step** for any new user. It creates the necessary configuration directories and populates them with a sample "GitHub API" project to enable the "Getting Started" tutorial. This command is safe to run multiple times.

**Usage:**

```bash
cx init
```

### `cx compile`

Compiles an API specification (like OpenAPI or Swagger) into a Contextually blueprint package. This is the core command for onboarding new services onto the platform.

**Usage:**

```bash
cx compile [OPTIONS] <SPEC_SOURCE>
```

**Arguments:**

| Argument        | Description                                              |
| --------------- | -------------------------------------------------------- |
| `<SPEC_SOURCE>` | **Required.** The path or URL to the specification file. |

**Options:**

| Option                | Description                                                              | Default             |
| --------------------- | ------------------------------------------------------------------------ | ------------------- |
| `--output, -o <PATH>` | The root directory to write the blueprint package to.                    | `~/.cx/blueprints/` |
| `--name <TEXT>`       | **Required.** The machine-friendly name of the service (e.g., `stripe`). |                     |
| `--version <TEXT>`    | The semantic version for this blueprint package.                         | `v1.0.0`            |
| `--namespace <TEXT>`  | The target namespace (`user`, `community`, `organization`, `system`).    | `user`              |

**Example:**

```bash
cx compile https://api.digitalocean.com/v2/openapi.json \
  --name digitalocean \
  --version v1.0.0 \
  --namespace user
```

---

## Command Group: `cx connection`

A command group for managing your local connection configurations stored in `~/.cx/connections/`.

### `cx connection list`

Lists all locally configured connections in a table, showing their friendly name, ID, and the blueprint they use.

**Usage:**

```bash
cx connection list
```

### `cx connection create`

Creates a new connection configuration file (`.conn.yaml`) and a corresponding secrets file (`.secret.env`). This command has two modes: interactive and non-interactive.

**Usage (Interactive):**

```bash
# Start the fully interactive, guided setup
cx connection create

# Start a guided setup for a specific blueprint
cx connection create --blueprint "system/mssql@v0.1.1"
```

**Usage (Non-Interactive):**

This mode is designed for scripting and automation.

```bash
cx connection create [OPTIONS]
```

**Options for `create`:**

| Option                   | Description                                                                              |
| ------------------------ | ---------------------------------------------------------------------------------------- |
| `--blueprint, -b <TEXT>` | Pre-select the blueprint ID to use for the connection.                                   |
| `--name <TEXT>`          | **(Non-interactive only)** A friendly name for the connection.                           |
| `--id <TEXT>`            | **(Non-interactive only)** A unique ID/alias for the connection.                         |
| `--detail <TEXT>`        | **(Non-interactive only)** A non-sensitive `key=value` pair. Can be used multiple times. |
| `--secret <TEXT>`        | **(Non-interactive only)** A SENSITIVE `key=value` pair. Can be used multiple times.     |

**Example (Non-Interactive):**

```bash
cx connection create \
  --name "My Production DB" \
  --id "prod-db" \
  --blueprint "system/mssql@v0.1.1" \
  --detail "server=db.example.com" \
  --detail "database=production" \
  --secret "username=prod_user" \
  --secret "password=my$ecret"
```

---

## Command Group: `cx extract`

A command group for all actions related to data extraction and I/O with external systems.

### `cx extract run`

Executes a declarative `.connector.yaml` script. This is the primary non-interactive way to run workflows.

**Usage:**

```bash
# Pipe data from a previous step into the script
cat data.json | cx extract run --script /path/to/workflow.connector.yaml
```

**Options:**

| Option                | Description                                                    |
| --------------------- | -------------------------------------------------------------- |
| `--script, -s <PATH>` | **Required.** Path to the `.connector.yaml` script to execute. |

---

## Command Group: `cx transform`

A command group for all actions related to in-memory data transformation.

### `cx transform run`

Reads JSON data from stdin, transforms it according to a `.transformer.yaml` script, and prints the result to stdout. This command is designed to be used in a pipeline.

**Usage:**

```bash
cx extract run --script get-data.yaml | cx transform run --script create-report.yaml
```

**Options:**

| Option                | Description                                                      |
| --------------------- | ---------------------------------------------------------------- |
| `--script, -s <PATH>` | **Required.** Path to the `.transformer.yaml` script to execute. |
