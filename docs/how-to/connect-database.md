# How-To: Connect to a Database

One of the most powerful features of Contextually is its ability to connect to and query your SQL databases directly from the `cx` shell. This guide will walk you through the process of setting up a new connection to a Microsoft SQL Server database.

The process for other supported databases (like PostgreSQL or Trino) is nearly identical—you just need to use the correct blueprint ID.

### Prerequisites

- You have `cx` installed.
- You have the connection details for your database: server address, database name, username, and password.

---

### Step 1: Find the Right Blueprint

The first step is to identify the correct blueprint for your database type. You can find a full list in our public [Blueprint Registry](https://github.com/flowcontextually/blueprints).

For this guide, we will use the official system blueprint for Microsoft SQL Server:
`system/mssql@v0.1.1`

---

### Step 2: Run `cx connection create`

The `cx connection create` command is an interactive wizard that will guide you through the setup process. It uses the blueprint you specify to ask for exactly the right information.

Run the following command in your terminal:

```bash
cx connection create --blueprint "system/mssql@v0.1.1"
```

---

### Step 3: Follow the Interactive Prompts

The shell will now prompt you for the information defined in the `mssql` blueprint.

1.  **Friendly Name:** Give your connection a memorable name, like `Production Reporting DB`.
2.  **Unique ID (Alias):** Provide a short, command-line-friendly alias, like `prod-db`.
3.  **Connection Details:** Enter your server address, database name, username, and password when prompted.

```
--- Create a New Connection (Interactive) ---
Using blueprint: system/mssql@v0.1.1

Please provide the following details for 'Username & Password':
Enter a friendly name for this connection: Production Reporting DB
Enter a unique ID (alias) (production-reporting-db): prod-db
(Note: Password input will be hidden for security.)
Server Address (hostname or IP): your-server.database.windows.net
Database Name: YourDatabase
Username: your_username
Password: ***************
```

After you provide the details, `cx` will show you a preview of the files it's about to create and ask for confirmation. Press `y` to save.

!!! success "Connection Saved!"
Your connection details are now securely saved in `~/.cx/connections/prod-db.conn.yaml` and `~/.cx/secrets/prod-db.secret.env`.

---

### Step 4: Test and Use Your New Connection

Now you can use your new connection in the interactive shell.

1.  **Start the shell:**

    ```bash
    cx
    ```

2.  **Connect using your new alias:**

    ```
    cx> connect user:prod-db --as db
    ```

    You should see a `✅ Connection successful` message.

3.  **Run a query!**
    You can now use the canonical `.query()` action to execute any SQL command against your database.

    ```
    cx> db.query("SELECT TOP 5 * FROM YourTable;")
    ```

You are now connected to your live database and can run queries, pipe results, and integrate this data source into larger Contextually workflows.
