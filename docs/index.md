# Welcome to Contextually

**The Universal Computational Fabric.** Contextually is a declarative, multi-stage automation platform for modern data and operations teams. It's built around a powerful command-line shell, `cx`, that allows you to interact with any API or data source using simple, version-controlled "blueprints."

Our core philosophy is to eliminate **context fragmentation**—the loss of lineage, intent, and nuance when work moves between different tools, people, and AI agents. With Contextually, your workflows become persistent, auditable, and transferable assets.

<div class="grid cards" markdown>

- :material-rocket-launch:{ .lg .middle } **Getting Started Tutorial**

  ***

  Our hand-holding guide to installing `cx` and running your first blueprint-driven API call in under 5 minutes. Start here!

  [:octicons-arrow-right-24: Start the Tutorial](tutorials/getting-started.md)

- :material-book-open-page-variant:{ .lg .middle } **Blueprint Ecosystem**

  ***

  Understand the core concepts behind Blueprints, the reusable packages that contain the "knowledge" of how to interact with external services.

  [:octicons-arrow-right-24: Learn about Blueprints](explanation/blueprint-ecosystem.md)

</div>

## Core Concepts

- **The `cx` Interactive Shell:** A stateful REPL that holds your connections and variables, enabling a fast, exploratory workflow.
- **Pipelining (`|`):** Chain commands together using the pipe operator to create powerful, in-terminal data flows, embodying the UNIX philosophy.
- **Workspace & Asset Management:** Treat your connections, variables, sessions, and reusable workflows (`flows`, `queries`, `scripts`) as first-class assets you can manage directly from the shell.
- **Universal Output Formatters:** Flexibly control the presentation of any command's output using flags like `--output table` and `--query`.
- **Blueprints & The Compiler:** The foundation of the platform. Blueprints are version-controlled packages that define how to interact with an external service, and `cx compile` automatically generates them from specifications like OpenAPI.

Contextually is built on three foundational principles:

1.  **Specification-First:** We don't build integrations for services; we build engines for protocols. We onboard new services by ingesting their machine-readable specifications.
2.  **Declarative over Imperative:** All workflows are defined as simple, version-controllable YAML scripts. The _what_ is in the script; the _how_ is in the engine.
3.  **Compose, Don't Command:** Our platform is built on the idea of composing small, powerful tools together to create complex workflows.
