# Concepts: Thinking in Pipelines

The design of the `cx` shell is deeply rooted in the **UNIX philosophy**, a set of design principles that have powered command-line interfaces for decades. The most important of these principles is **composition**:

> Write programs that do one thing and do it well. Write programs to work together. Write programs to handle text streams, because that is a universal interface.

The `cx` shell adapts this for the modern data world. Instead of text streams, we use structured data (JSON), but the core idea is the same. To master `cx`, you must learn to "think in pipelines."

---

## The Anatomy of a `cx` Pipeline

A pipeline is a chain of one or more commands linked by the pipe operator (`|`). The data flows from left to right.

`[SOURCE]` -> `|` -> `[FILTER / TRANSFORM]` -> `|` -> `[ACTION / SINK]`

### 1. The Source: Where Data Begins

Every pipeline starts with a command that **produces data**. This can be:

- A **dot-notation command** fetching data from a live API (`gh.getUsers()`).
- A **`query run` command** fetching data from a database.
- A **`flow run` command** executing a predefined workflow.
- A **session variable** that holds data from a previous operation (`my_repos`).

### 2. The Filter / Transform: Shaping the Data

This is the middle of the pipeline. These commands take data from their `stdin` (the command to the left) and transform it in some way.

- **`transform run --script ...`**: The most powerful transformer. It executes a multi-step `.transformer.yaml` workflow on the incoming data.
- **`script run ...`**: Executes a custom Python script on the data.
- **`--query "..."`**: The universal JMESPath formatter can also be thought of as a powerful, in-line filter and shaping tool.

### 3. The Sink: Where Data Ends

The final command in a pipeline either displays the data or sends it to another system.

- **`--output table`**: A formatter that acts as a "sink," displaying the final data in a human-readable table.
- **(Future) `email send ...`**: A hypothetical action that would take the incoming data and use it to populate and send an email.
- **(Future) `webhook post ...`**: An action that would send the final data to a webhook URL.

If no sink is specified, the default behavior is to print the final, transformed JSON data to the console.

---

## Prototyping in the Shell

The power of the interactive shell is that it allows you to build these pipelines **iteratively**.

1.  **Start with the source:** `cx> repos = gh.read("...")`
2.  **Inspect the result:** `cx> inspect repos`
3.  **Add a filter:** `cx> repos --query "content[?language=='Python']"`
4.  **Add formatting:** `cx> repos --query "content[?language=='Python']" --output table`
5.  **Chain a transformation:** `cx> repos | transform run --script my-script.yaml`
6.  **Assign the final result:** `cx> python_repos = repos | transform ...`

Once you have perfected your one-liner, you can encapsulate that entire pipeline inside a `.flow.yaml` file. This process of interactive prototyping followed by persistent automation is the core workflow of the Contextually platform.
