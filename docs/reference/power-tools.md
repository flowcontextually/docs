# Reference: Power User Tools

The `cx` interactive shell integrates two powerful, industry-standard tools for templating and data manipulation: **Jinja2** and **JMESPath**. Mastering these is the key to unlocking the full potential of the shell.

---

## Templating with Jinja2

Anywhere you can provide a string value in a `cx` command—such as a parameter in a dot-notation action or an asset name—you can use Jinja2 templating syntax (`{{ ... }}`).

The shell automatically makes all of your current **session variables** available within the template context.

### Common Use Cases

**1. Using a Value from a Variable:**
This is the most common use case. After storing the result of one command, you can use its values to drive the next one.

```
# Store the Microsoft organization profile in a variable
cx> msft = gh.getUser(username="microsoft")

# Inspect the variable to find the 'repos_url' key
cx> inspect msft

# Use that key in the next command to fetch the repositories
cx> repos = gh.read("{{ msft['Get User from GitHub'].repos_url }}")
```

**2. Dynamic File Paths:**
You can construct file paths dynamically, which is useful in scripts.

```
cx> filename = "daily-report-2025-09-03.xlsx"

# The 'save' command is a future feature, but this demonstrates the concept.
cx> my_data | save --path "./reports/{{ filename }}"
```

> For a complete guide to Jinja2 syntax, please refer to the [**official Jinja2 documentation**](https://jinja.palletsprojects.com/en/3.1.x/templates/).

---

## Filtering and Reshaping with JMESPath

The universal `--query` flag allows you to apply a [**JMESPath**](https://jmespath.org/) expression to the final result of any command or pipeline _before_ it is rendered. This is an incredibly powerful tool for drilling down into complex JSON data without leaving the shell.

The query operates on the **unpacked data payload**. For example, after running `gh.read(...)`, the query runs on the array of repository objects, not the `VfsFileContentResponse` wrapper.

### Common Use Cases

Assume we have a variable `repos` which contains the list of repository objects from the GitHub API.

**1. Filtering a List:**
Get a list of repositories that have more than 20,000 stargazers. The `?` operator is the filter expression.

```
cx> repos --query "[?stargazers_count > `20000`]"
```

**2. Selecting Specific Keys (Projection):**
From the filtered list, create a new list of objects containing only the `name` and `language`.

```
cx> repos --query "[?stargazers_count > `20000`].[name, language]"
```

!!! note
When you project a list of keys like this, the result is a **list of lists**. To render this as a table, you must provide headers with the `--columns` flag:
`... --output table --columns Name,Language`

**3. Reshaping a Dictionary:**
The `to_array(@)` function is a powerful trick to turn a single dictionary into a list of key-value pairs, perfect for viewing in a table.

```
# Get a single object
cx> msft_profile = flow run get-user-flow username=microsoft

# View it as a key-value table
cx> msft_profile --output table --query "to_array(@)"
```

> For a complete guide to JMESPath syntax and functions, please see the [**official JMESPath Tutorial**](https://jmespath.org/tutorial.html).
