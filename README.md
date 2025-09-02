# 📖 Contextually Docs

This repository contains the source files for the official documentation website for the **Contextually** platform, available at [flowcontextually.github.io/docs/](https://flowcontextually.github.io/docs/).

The site is built using [MkDocs](https://www.mkdocs.org/) with the excellent [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

## 🚀 Local Development Setup

To run a live-reloading local preview of the documentation site, you will need a modern version of Python and the `uv` package manager.

### Prerequisites

- Python 3.12+
- `uv` (can be installed via `pip install uv`)

### Installation & Serving

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/flowcontextually/docs.git
    cd docs
    ```

2.  **Create the virtual environment:**

    ```bash
    uv venv
    ```

3.  **Activate the environment:**

    ```bash
    source .venv/bin/activate
    ```

4.  **Install all dependencies:**
    This command reads the `pyproject.toml` file and installs `mkdocs` and the `material` theme.

    ```bash
    uv pip install -e .
    ```

5.  **Serve the site:**
    This command starts a local web server with live reloading. Any changes you make to the markdown files in the `docs/` directory will be instantly visible in your browser.

    ```bash
    mkdocs serve
    ```

6.  **View the site:**
    Open your web browser and navigate to **http://127.0.0.1:8000**.

## 🏛️ Project Structure

The content for the documentation site is structured according to the [Diátaxis framework](https://diataxis.fr/), which organizes documentation into four distinct types:

- `docs/tutorials/`: Learning-oriented guides that take the user through a complete project.
- `docs/how-to/`: Problem-oriented recipes for achieving a specific goal.
- `docs/reference/`: Information-oriented technical descriptions of the machinery.
- `docs/explanation/`: Understanding-oriented discussions about concepts and design.

The main configuration file for the site is `mkdocs.yml`. The navigation, theme, and markdown extensions are all defined here.

## 🤝 Contributing

Contributions to the documentation are incredibly valuable and always welcome! If you find a typo, a confusing explanation, or have an idea for a new tutorial, please feel free to open an issue or submit a pull request.

The documentation is automatically deployed to GitHub Pages via a GitHub Actions workflow defined in `.github/workflows/deploy-docs.yml` on every push to the `main` branch.
