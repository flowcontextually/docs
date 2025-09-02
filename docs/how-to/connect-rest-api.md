# How-To: Connect to a REST API (OAuth2)

Connecting to a REST API is a core function of Contextually. While many public APIs can be used without authentication (like the GitHub example in our "Getting Started" guide), most powerful APIs require it.

This guide will walk you through a common and secure pattern for connecting to an API that uses **OAuth2**, using the **Spotify** blueprint as our example.

### Prerequisites

- You have `cx` installed (`v0.1.1` or later).
- You have followed the API provider's instructions to obtain your credentials. For Spotify, this means visiting their [Developer Dashboard](https://developer.spotify.com/dashboard) to get a **Client ID** and **Client Secret**.

---

### Step 1: Obtain a Refresh Token (One-Time Setup)

OAuth2 requires a one-time, browser-based user authorization step to grant `cx` permission to act on your behalf. This process generates a long-lived **Refresh Token**, which is the key to all future automated interactions.

This step is typically performed using a small helper script or a utility provided by the API developer. A detailed guide is beyond the scope of this document, but the process generally involves:

1.  Running a local script.
2.  Being redirected to your browser to log in to the service (e.g., Spotify).
3.  Authorizing the application.
4.  Being redirected back to the script, which captures the `refresh_token`.

!!! tip "What to Expect"
At the end of this one-time process, you will have three critical secrets: `Client ID`, `Client Secret`, and `Refresh Token`. Keep them safe.

---

### Step 2: Find the Blueprint

Next, identify the correct blueprint for the API. We will use the official Spotify blueprint from the public registry. You can find its ID in the [Blueprint Registry README](https://github.com/flowcontextually/blueprints).

For this guide, the ID is: `community/spotify@v0.1.0`

You can inspect this blueprint's `supported_auth_methods` section in its source to see the exact secret names it expects (e.g., `client_id`, `client_secret`, `refresh_token`).

---

### Step 3: Create the Connection Non-Interactively

For connections with multiple, sensitive secret keys, the non-interactive `cx connection create` command is the most secure and efficient method. It allows you to provide the secrets directly without them being saved in your shell history.

Construct the command in your text editor, replacing the placeholder values with your actual credentials. Then, paste and run the complete command in your terminal.

```bash
cx connection create \
  --name "My Spotify Account" \
  --id "my-spotify" \
  --blueprint "community/spotify@v0.1.0" \
  --secret "client_id=YOUR_SPOTIFY_CLIENT_ID" \
  --secret "client_secret=YOUR_SPOTIFY_CLIENT_SECRET" \
  --secret "refresh_token=YOUR_LONG_LIVED_REFRESH_TOKEN"
```

!!! success "Connection Saved Securely!"
The command will create `~/.cx/connections/my-spotify.conn.yaml` for the non-sensitive data and `~/.cx/secrets/my-spotify.secret.env` for your secret tokens.

---

### Step 4: Test and Use Your Connection

Now you can use your new connection in the interactive shell. `cx` will handle all future token refreshes automatically in the background.

1.  **Start the shell:**

    ```bash
    cx
    ```

2.  **Connect using your new alias:**

    ```
    cx> connect user:my-spotify --as spotify
    ```

    You should see a `✅ Connection successful` message.

3.  **Run an action!**
    Type `spotify.` and press Tab to see all the actions available from the Spotify blueprint.

    ```
    cx> spotify.getListOfCurrentUserPlaylists()
    ```

You are now connected to a live, OAuth2-protected API. The `cx` shell will manage the short-lived access tokens for you, allowing you to focus on your workflow.
