# Getting Started with Contextually

Welcome! This tutorial is the best place to start your journey with Contextually. In the next five minutes, you will:

1.  Install the `cx` command-line shell.
2.  Initialize a sample project on your local machine.
3.  Use the interactive shell to connect to the public GitHub API.
4.  Run your first dynamic, blueprint-driven, and validated API command.

Let's begin!

---

## 1. Installation

Download and install the latest pre-compiled binary for your operating system from our [**GitHub Releases page**](https://github.com/flowcontextually/cx-shell/releases).

=== "Linux"

    ```bash
    # This script downloads the latest v0.1.1 Linux binary and moves it to your path.
    curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.1.1/cx-v0.1.1-linux-x86_64.tar.gz | tar -xz
    sudo mv cx /usr/local/bin/
    cx --version
    ```

=== "macOS (Intel)"

    ```bash
    # This script downloads the latest v0.1.1 macOS (Intel) binary and moves it to your path.
    curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.1.1/cx-v0.1.1-macos-x86_64.tar.gz | tar -xz
    sudo mv cx /usr/local/bin/
    cx --version
    ```

=== "macOS (Apple Silicon)"

    !!! warning "Apple Silicon (M1/M2/M3)"
        A native Apple Silicon build is not yet available. You can run the Intel version via Rosetta 2 using the macOS (Intel) instructions.

=== "Windows (PowerShell)"

    ```powershell
    # This script downloads the latest v0.1.1 Windows binary and unzips it.
    $url = "https://github.com/flowcontextually/cx-shell/releases/download/v0.1.1/cx-v0.1.1-windows-amd64.zip"
    $output = "cx.zip"
    Invoke-WebRequest -Uri $url -OutFile $output
    Expand-Archive -Path $output -DestinationPath .

    # You should now have `cx.exe` in the current directory.
    # We recommend moving `cx.exe` to a directory in your system's PATH for system-wide access.
    ./cx.exe --version
    ```

!!! success "Success!"
If the `cx --version` command prints a version number, you have successfully installed the Contextually Shell.

---

## 2. Initialize Your Environment

The `cx init` command is the key to getting started. It creates a hidden `~/.cx` directory in your home folder and populates it with essential configuration and a sample "GitHub API" blueprint.

Run the command in your terminal:

```bash
cx init
```

You should see output confirming that the directories and sample files have been created. This is a one-time setup step.

---

## 3. Start the Interactive Shell

The `cx` shell is where the magic happens. It's an interactive session that remembers your connections and lets you explore APIs dynamically.

To start it, simply type `cx`:

```bash
cx
```

Your terminal prompt will change to `cx>`, indicating you are now inside the interactive shell.

---

## 4. Connect and Run!

Now let's use the sample project that `init` created for us.

**First, connect to the public GitHub API.** We'll use the `connect` command to activate the connection defined in `~/.cx/connections/github.conn.yaml` and give it a short, memorable alias for this session: `gh`.

```
cx> connect user:github --as gh
```

You will see a spinner while `cx` tests the connection, followed by a `✅ Connection successful` message.

**Finally, run your first blueprint-driven command.** Type `gh.` and wait a moment for the autocompletion menu to appear. You'll see the `getUser` action, which was discovered from the blueprint. Let's get the public profile for Linus Torvalds.

```
cx> gh.getUser(username="torvalds")
```

**Congratulations!** You should see a JSON object printed to your screen with the public profile details for Linus Torvalds.

You have just successfully:

- Loaded the `github` blueprint that defines how to talk to the GitHub API.
- Had your input (`username="torvalds"`) validated against the action's Pydantic model.
- Executed a dynamic action against a live, production API.
- Passed a parameter that was correctly used to build the final URL (`.../users/torvalds`).

To exit the shell, simply type `exit` or press `Ctrl+D`.

---

## Next Steps

You've mastered the basics! Now you're ready to explore more powerful features:

- [**How-To: Connect to a Database**](../how-to/connect-database.md)
- [**Tutorial: Compiling Your First Blueprint**](../tutorials/compiling-a-blueprint.md)
- [**Explanation: The Blueprint Ecosystem**](../explanation/blueprint-ecosystem.md)
