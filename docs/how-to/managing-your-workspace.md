# How-To: Manage Your Workspace

The `cx` interactive shell is more than just a command line—it's a persistent workspace. As you work, you create valuable assets: active connections, variables holding important data, and reusable workflows. This guide will show you how to manage these assets effectively using the built-in workspace commands.

---

## Understanding Your Session

Your "session" is the live, in-memory state of your `cx>` prompt. It includes:

- **Active Connections:** Aliases you've created with `connect`.
- **Session Variables:** Data you've saved with the `=` operator.

### Checking Session Status

At any time, you can get a high-level overview of your current session with the `session status` command.

```
cx> session status
```

This will display a clean panel showing how many connections and variables are currently active.

### Listing Active Connections

To see the details of your active connections, use the `connections` command.

```
cx> connections
```

This is useful for remembering which alias points to which data source.

### Listing Session Variables

To see a detailed summary of all the data you have stored in memory, use the `var list` (or `vars list`) command.

```
cx> var list
```

This will display a table showing each variable's name, its data type (like `dict` or `list`), its size, and a preview of its content.

---

## Persisting Your Workspace with Sessions

Session variables are temporary; they disappear when you `exit` the shell. To save your entire workspace for later, you need to use the `session` commands.

### Saving a Session

The `session save` command takes a snapshot of your current state (all active connections and variables) and saves it to a file in your `~/.cx/sessions/` directory.

```
# After setting up connections and variables...
cx> session save my-project-work
```

This creates a file named `my-project-work.cxsession`.

### Listing Saved Sessions

Forgot what you've saved? Use `session list`.

```
cx> session list
```

This scans your `~/.cx/sessions/` directory and shows a table of all saved sessions, including when they were last modified and their size.

### Loading a Session

To restore a saved workspace, start a fresh `cx` shell and use `session load`.

```
cx> session load my-project-work
```

The shell will instantly restore all the connections and variables from that session, allowing you to pick up exactly where you left off.

### Deleting a Session

Once a project is finished, you can clean up saved sessions with `session rm`.

```
cx> session rm my-project-work
```

The shell will ask for confirmation before deleting the file.

---

## Managing Your Reusable Assets

Beyond the live session, `cx` helps you build a personal library of reusable `flows`, `queries`, and `scripts`. These are stored as files in your `~/.cx/` directory.

### Listing Your Assets

You can discover the assets you've created with the `list` subcommand for each asset type.

```
# See all your reusable YAML workflows
cx> flow list

# See all your saved SQL files
cx> query list

# See all your saved Python scripts
cx> script list
```

### Editing Your Assets with `open`

The `open` command is a powerful bridge to your text editor. It lets you quickly open the source file for any asset without needing to find it in your file explorer.

```
# Open a flow file in your default editor (e.g., VS Code)
cx> open flow my-data-flow

# Open a saved SQL query
cx> open query get-daily-users
```

By mastering these commands, you transform the `cx` shell from a simple tool into a complete, persistent, and organized Integrated Development Environment (IDE) for your data and operations tasks.
