# Cookbook: Creating an On-Demand Data API

One of the most powerful concepts in the `cx` shell is treating your saved `.flow.yaml` files not just as scripts, but as a personal library of reusable, parameterizable functions. By creating a flow that takes arguments, you are effectively building a custom, on-demand data API for your own operational needs.

**Goal:** Create a reusable flow to look up a GitHub user's profile and then use formatters to view the data in different ways.

---

### 1. The "API Endpoint" - `get-user.flow.yaml`

First, we create our reusable workflow. This flow requires a `username` to be passed in via `script_input`.

**Action:** Save the following content in `~/.cx/flows/get-user.flow.yaml`.

```yaml
# ~/.cx/flows/get-user.flow.yaml
name: "Get GitHub User"
description: "Fetches the public profile for a given GitHub username."
steps:
  - id: get_user
    name: "Get User from GitHub"
    connection_source: "user:github" # Assumes a 'github' connection config exists
    run:
      action: "run_declarative_action"
      template_key: "getUser"
      context:
        # This username is provided at runtime by the 'flow run' command
        username: "{{ script_input.username }}"
```

---

### 2. "Calling" the API Endpoint

Now, from within the `cx>` shell, you can "call" this flow like a function using the `flow run` command.

**Prerequisite:** You have an active GitHub connection with the alias `gh`.

```
cx> connect user:github --as gh
```

**Execution:**

```
# Call the flow for the 'microsoft' user
cx> flow run get-user username=microsoft
```

**Result:** The shell executes the flow, injecting `username=microsoft` into the `script_input` context. The final, raw JSON output of the `getUser` action is printed to the screen.

---

### 3. Composing with Formatters

This is where the true power emerges. Because `flow run` is a standard data-producing command, you can pipe its output to the universal formatters to reshape the data on the fly, just like you would with a real API.

**Scenario 1: View a summary as a table**

Let's say you only care about a few key details. You can run the same flow and pipe the result to a table view with selected columns.

```
cx> flow run get-user username=microsoft --output table --query "to_array(@.'Get User from GitHub')" --columns key,value
```

- **`--query "..."`**: The JMESPath query unpacks the result from the step name (`Get User from GitHub`) and transforms the resulting dictionary into a list of key-value pairs.
- **`--output table`**: Renders that list as a clean, two-column table.

**Scenario 2: Extract a single piece of information**

What if you only need the number of public repositories?

```
cx> flow run get-user username=microsoft --query "'Get User from GitHub'.public_repos"
```

**Result:** The shell will print a single number: `7046` (or whatever the current count is).

By creating a library of these small, parameterized flows, you build up a powerful set of custom tools tailored to your exact needs. You no longer need to remember complex API calls; you just need to remember the name of the flow you created.
