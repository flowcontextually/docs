# How-To: Build In-Terminal Pipelines

The true power of the `cx` shell lies in its ability to compose commands into powerful, in-terminal pipelines. This follows the UNIX philosophy: have small, sharp tools that do one thing well, and chain them together to solve complex problems.

This guide will walk you through the syntax and concepts for building pipelines, from simple data viewing to complex filtering and formatting.

---

## The Core Operators

Your entire workflow is built around three core operators:

| Operator       | Syntax  | Purpose                                                                               |
| :------------- | :------ | :------------------------------------------------------------------------------------ |
| **Assignment** | `=`     | Stores the output of a command or pipeline in a variable.                             |
| **Pipelining** | `\|`    | Sends the output of the command on the left to the input of the command on the right. |
| **Grouping**   | `(...)` | Controls the order of operations, just like in mathematics.                           |

---

## A Step-by-Step Example

Let's build a pipeline to find the most popular repositories from a GitHub organization.

### Step 1: Get the Raw Data

First, we need to get the list of repositories. We'll connect to GitHub and use the `read` command, storing the result in a variable named `repos`.

```

cx> connect user:github --as gh
cx> repos = gh.read("https://api.github.com/users/google/repos")

```

### Step 2: Unpack and View the Data

The `repos` variable contains a wrapper object. To see the actual list of repositories, we need to unpack the data and view it as a table. This is our first simple pipeline.

```

cx> repos --query "content" | --output table

```

- `repos --query "content"`: This command takes the `repos` variable, and the JMESPath query `content` extracts the value of the "content" key (which is our list of repository objects).
- `|`: The pipe operator sends this list of objects to the next command.
- `--output table`: This is a universal formatter that takes the incoming list and renders it as a table.

### Step 3: Filter and Select Data

The full table is too much information. Let's refine the pipeline to only show popular repositories and select specific columns.

```

cx> repos --query "content[?stargazers_count > 20000].[name, language, stargazers_count]" --output table --columns Name,Language,Stars

```

Let's break down this powerful one-liner:

1.  **`repos`**: Starts the pipeline with our variable.
2.  **`--query "..."`**: The JMESPath expression runs first.
    - `content`: Unpacks the repository list from the `content` key.
    - `[?stargazers_count > 20000]`: Filters this list, keeping only repositories with more than 20,000 stars.
    - `.[name, language, stargazers_count]`: From the filtered repos, it creates a new list containing only the values for these three keys. The result is a _list of lists_.
3.  **`--output table`**: Specifies that the final result should be a table.
4.  **`--columns Name,Language,Stars`**: Provides the headers for the table, which is required when the input is a list of lists.

### Step 4: Grouping for Precedence

What if you wanted to run a script on the data _before_ formatting it? You use parentheses to group the data processing part of the pipeline.

```

cx> (repos --query "content" | script run summarize-repos) --output table

```

This ensures the `script run` command completes, and then the `--output table` flag is applied to the _final_ result of that pipeline.

By combining variables, pipes, and formatters, you can perform complex data analysis and exploration tasks in a single, readable line without ever leaving your terminal.
