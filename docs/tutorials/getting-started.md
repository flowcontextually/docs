# Getting Started with Contextually

Welcome! This tutorial is the best place to start your journey with Contextually. In the next five minutes, you will:

1.  Install the `cx` command-line shell (v0.2.1).
2.  Initialize a sample project on your local machine.
3.  Use the interactive shell to connect to the public GitHub API.
4.  Run an API command and store its result in a **session variable**.
5.  **Inspect** the data you've fetched.
6.  Use your variable to run a second command.
7.  Format the final output as a **table**.
8.  **Save your session** for later use.

<!-- TODO: Add animated GIF of this entire workflow -->

Let's begin!

---

## 1. Installation

Download and install the latest `v0.2.1` pre-compiled binary for your operating system from our [**GitHub Releases page**](https://github.com/flowcontextually/cx-shell/releases).

=== "Linux"

    ```bash
    # This script downloads the latest v0.2.1 Linux binary and moves it to your path.
    curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.2.1/cx-v0.2.1-linux-x86_64.tar.gz | tar -xz
    sudo mv cx /usr/local/bin/
    cx --version
    ```

=== "macOS (Intel & Apple Silicon)"

    ```bash
    # This script downloads the latest v0.2.1 macOS binary and moves it to your path.
    curl -sL https://github.com/flowcontextually/cx-shell/releases/download/v0.2.1/cx-v0.2.1-macos-x86_64.tar.gz | tar -xz
    sudo mv cx /usr/local/bin/
    cx --version
    ```

=== "Windows (PowerShell)"

    ```powershell
    # This script downloads the latest v0.2.1 Windows binary and unzips it.
    $url = "https://github.com/flowcontextually/cx-shell/releases/download/v0.2.1/cx-v0.2.1-windows-amd64.zip"
    $output = "cx.zip"
    Invoke-WebRequest -Uri $url -OutFile $output
    Expand-Archive -Path $output -DestinationPath .
    # We recommend moving `cx.exe` to a directory in your system's PATH for system-wide access.
    ./cx.exe --version
    ```

!!! success "Success!"
If the `cx --version` command prints a version number, you have successfully installed the Contextually Shell.

---

## 2. Initialize Your Environment

The `cx init` command creates a hidden `~/.cx` directory and populates it with essential configuration and a sample "GitHub API" project.

```bash
cx init
```

---

## 3. Start the Interactive Shell

The `cx` shell is a stateful session where you can explore APIs and build workflows.

```bash
cx
```

Your terminal prompt will change to `cx>`, indicating you are in the interactive shell.

---

## 4. Connect to the API

Use the `connect` command to activate the sample GitHub connection and give it the short alias `gh` for this session.

```
cx> connect user:github --as gh
```

You will see a `✅ Connection successful` message.

---

## 5. Fetch Data and Store it in a Variable

Now, let's run an API command. We'll get the public profile for the Microsoft organization and save the result to a new session variable named `msft`.

```
cx> msft = gh.getUser(username="microsoft")
```

The shell will show a confirmation: `✓ Variable 'msft' set.`. The data is now stored in your session's memory.

---

## 6. Inspect Your Data

How do you know what's in the `msft` variable? Use the `inspect` command.

```
cx> inspect msft
```

You will see a formatted panel showing you that `msft` is a dictionary, its length, and all of its available keys (`login`, `id`, `repos_url`, etc.). This is how you discover the data available to you.

---

## 7. Use Your Variable to Run Another Command

Now, let's use the `repos_url` we discovered in the `msft` variable to fetch the list of Microsoft's public repositories. We will use Jinja2 templating to inject the value.

```
cx> repos = gh.read("{{ msft['Get User from GitHub'].repos_url }}")
```

This runs the `read` command and stores the massive list of repositories in a new variable called `repos`.

---

## 8. Format Your Final Output

Printing the raw `repos` variable would be overwhelming. Let's use the universal output formatter to create a clean, readable table of the most popular repositories.

```
cx> repos --output table --query "content[?stargazers_count > 40000].[name, language, stargazers_count]"
```

**Congratulations!** You should see a beautifully formatted table in your terminal showing only Microsoft's most popular projects.

You have just successfully:

- Connected to a live API.
- Stored results in session variables.
- Inspected variable structure.
- Used variables to run subsequent commands.
- Used a powerful JMESPath query to filter and reshape data on the fly.
- Rendered the final result as a clean, formatted table.

---

## 9. Save Your Session

Finally, let's save this entire workspace—including your active `gh` connection and your `msft` and `repos` variables—so you can come back to it later.

```
cx> session save my-first-session
```

To exit the shell, type `exit`. The next time you start `cx`, you can simply run `session load my-first-session` to restore your entire workspace instantly.

---

## Next Steps

You've mastered the core workflow! Now you're ready to explore more powerful features:

- [**How-To: Manage Your Workspace**](../how-to/managing-your-workspace.md)
- [**How-To: Build In-Terminal Pipelines**](../how-to/building-pipelines.md)
- [**Reference: The Interactive Shell Language**](../reference/repl.md)
