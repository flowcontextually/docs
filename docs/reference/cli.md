# Reference: CLI

This document is the official reference for the Contextually Shell (`cx`) command-line interface. It details all available commands, subcommands, and their options.

---

## Top-Level Command: `cx`

The main entrypoint for the application. Running `cx` with no subcommand starts the interactive REPL (Read-Eval-Print Loop), which is the primary way to use the shell.

**Usage:**

```bash
cx [OPTIONS] [COMMAND]
```

### Global Options

These options can be used with the top-level `cx` command or any of its subcommands.

| Option            | Description                   |
| ----------------- | ----------------------------- |
| `--help`          | Show help messages.           |
| `--verbose`, `-v` | Enable verbose DEBUG logging. |

---

## `cx init`

Initializes the `cx` shell environment in your home directory (`~/.cx`).

This is the recommended first step for any new user. It creates the necessary configuration directories and populates them with a sample "Petstore API" project to get you started, enabling the "5-Minute Tutorial".

**Usage:**

```bash
cx init
```

---

## `cx compile`

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
cx compile https://petstore.swagger.io/v2/swagger.json \
  --name petstore \
  --version v2.0.0 \
  --namespace user
```

---

## `cx extract`

A command group for all actions related to data extraction and I/O with external systems.

### `cx extract run`

Executes a declarative `.connector.yaml` script. This is the primary non-interactive way to run workflows.

**Usage:**

```bash
# Execute a script
cx extract run --script /path/to/your/workflow.connector.yaml

# Pipe data from a previous step into the script
cat data.json | cx extract run --script /path/to/your/workflow.connector.yaml
```

**Options:**

| Option                | Description                                                    |
| --------------------- | -------------------------------------------------------------- |
| `--script, -s <PATH>` | **Required.** Path to the `.connector.yaml` script to execute. |
| `--debug-action`      | Print rendered action payloads to stderr before sending.       |

---

## `cx transform`

A command group for all actions related to in-memory data transformation.

### `cx transform run`

Reads JSON data from stdin, transforms it according to a `.transformer.yaml` script, and prints the result to stdout. This command is designed to be used in a pipeline.

**Usage:**

```bash
cat input.json | cx transform run --script /path/to/your/transform.transformer.yaml > output.json
```

**Options:**

| Option                | Description                                                      |
| --------------------- | ---------------------------------------------------------------- |
| `--script, -s <PATH>` | **Required.** Path to the `.transformer.yaml` script to execute. |

### Example Pipeline

This demonstrates the core "Compose, Don't Command" philosophy of Contextually.

```bash
# A full ETL and delivery pipeline
cx extract run --script get-data.yaml | \
cx transform run --script create-report.yaml | \
cx extract run --script send-email.yaml
```
