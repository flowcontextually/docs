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

- **The `cx` Shell:** An interactive REPL that holds the state of your connections, allowing you to chain commands and explore APIs dynamically.
- **Blueprints:** Version-controlled packages that define how to interact with an external service. They are the reusable, shareable core of the ecosystem.
- **The Compiler (`cx compile`):** A powerful tool that takes a machine-readable specification (like OpenAPI) and automatically generates a complete blueprint package.
- **The UNIX Philosophy:** Small, powerful tools (`extract`, `transform`) are composed together using pipes (`|`) to create complex, robust workflows.

## The Big Picture

Contextually is built on three foundational principles:

1.  **Specification-First:** We don't build integrations for services; we build engines for protocols. We onboard new services by ingesting their machine-readable specifications.
2.  **Declarative over Imperative:** All workflows are defined as simple, version-controllable YAML scripts. The _what_ is in the script; the _how_ is in the engine.
3.  **Compose, Don't Command:** Our platform is built on the idea of composing small, powerful tools together to create complex workflows.
