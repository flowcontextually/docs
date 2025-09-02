# Getting Started with Contextually

Welcome! This tutorial is the best place to start your journey with Contextually. In the next five minutes, you will:

1.  Install the `cx` command-line shell.
2.  Initialize a sample project on your local machine.
3.  Use the interactive shell to connect to a live sample API.
4.  Run your first dynamic, blueprint-driven command.

Let's begin!

---

## 1. Installation

Download and install the latest pre-compiled binary for your operating system from our [**GitHub Releases page**](https://github.com/flowcontextually/cx-shell/releases).

=== "Linux"

    ```bash
    # This script downloads the latest Linux binary, extracts it, and moves it to your path.
    curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.1.0/cx-v0.1.0-linux-x86_64.tar.gz | tar -xz
    sudo mv cx /usr/local/bin/
    cx --version
    ```

=== "macOS (Intel)"

    ```bash
    # This script downloads the latest macOS (Intel) binary, extracts it, and moves it to your path.
    curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.1.0/cx-v0.1.0-macos-x86_64.tar.gz | tar -xz
    sudo mv cx /usr/local/bin/
    cx --version
    ```

=== "macOS (Apple Silicon)"

    !!! warning "Apple Silicon (M1/M2/M3)"
        A native Apple Silicon build is not yet available. You can run the Intel version via Rosetta 2 using the macOS (Intel) instructions.

=== "Windows (PowerShell)"

    ```powershell
    # This script downloads the latest Windows binary and unzips it.
    $url = "https://github.com/flowcontextually/cx-shell/releases/download/v0.1.0/cx-v0.1.0-windows-amd64.zip"
    $output = "cx.zip"
    Invoke-WebRequest -Uri $url -OutFile $output
    Expand-Archive -Path $output -DestinationPath .

    # You should now have `cx.exe` in the current directory.
    # For system-wide access, move `cx.exe` to a directory in your system's PATH.
    ./cx.exe --version
    ```

!!! success "Success!"
If the `cx --version` command prints a version number, you have successfully installed the Contextually Shell.

---

## 2. Initialize Your Environment

Now, we'll use the `cx init` command to set up your local environment. This command creates a hidden `~/.cx` directory in your user's home folder and populates it with some essential configuration and a sample "Petstore API" project.

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

**First, connect to the Petstore API.** We'll use the `connect` command to activate the connection defined in `~/.cx/connections/petstore.conn.yaml` and give it a short, memorable alias for this session: `api`.

```
cx> connect user:petstore --as api
```

!!! success
You should see a `✅ Connection successful` message.

**Finally, run your first blueprint-driven command.** Type `api.` and wait a moment for the autocompletion menu to appear with a list of available actions. Let's run `getPetById` and ask for the pet with an ID of 1.

```
cx> api.getPetById(petId=1)
```

**Congratulations!** You should see a JSON object printed to your screen with the details of the pet.

You have just successfully:

- Loaded a blueprint that defines how to talk to the Petstore API.
- Executed a dynamic action against that API.
- Passed a parameter that was correctly used to build the final URL (`.../pet/1`).

To exit the shell, simply type `exit` or press `Ctrl+D`.

---

## Next Steps

You've mastered the basics! Now you're ready to explore the core concepts in more depth:

- [Explanation: The Blueprint Ecosystem](../explanation/blueprint-ecosystem.md)
- [Tutorial: Compiling Your First Blueprint](../tutorials/compiling-a-blueprint.md)
