# Welcome to Contextually

**The Universal Computational Fabric.** Contextually is a declarative, multi-stage automation platform for modern data and operations teams. It's built around a powerful command-line shell, `cx`, that functions as a stateful **Workspace IDE** and an **AI Reasoning Partner**.

Our core philosophy is to eliminate **context fragmentation**—the loss of lineage, intent, and nuance when work flows between different tools, people, and AI agents.

<div class="grid cards" markdown>

- :material-rocket-launch:{ .lg .middle } **Getting Started Tutorial**

  ***

  Our 5-minute guide to installing `cx` and running your first command. This is the best place for new users to begin their journey.

  [:octicons-arrow-right-24: Start the Tutorial](tutorials/getting-started.md)

- :material-brain:{ .lg .middle } **The Agentic Shell**

  ***

  Discover the **AI Assistant** built into `cx`. Learn to translate natural language into commands with `//` and solve complex problems with `agent`.

  [:octicons-arrow-right-24: Meet Your Assistant](concepts/agentic-shell.md)

- :material-package-variant-closed:{ .lg .middle } **Application Ecosystem**

  ***

  Explore Applications—complete, "toolkit-in-a-box" solutions that bundle flows, queries, and blueprints to solve real-world problems.

  [:octicons-arrow-right-24: Find Packaged Solutions](concepts/application-ecosystem.md)

- :material-book-open-page-variant:{ .lg .middle } **Blueprint Foundation**

  ***

  Understand Blueprints, the version-controlled packages that contain the "knowledge" of how to interact with any external API or service.

  [:octicons-arrow-right-24: Learn about Blueprints](concepts/blueprint-ecosystem.md)

</div>

## Our Philosophy: The Contextually Way

Contextually is built on three foundational principles that guide every architectural decision.

- **Specification-First**

  We don't build integrations for services; we build engines for protocols. We onboard new services by ingesting their machine-readable specifications (like OpenAPI) with the `cx compile` command, not by writing bespoke code.

- **Declarative over Imperative**

  All workflows are defined as simple, version-controllable YAML scripts. The _what_ is in the script; the _how_ is in the engine. This makes workflows readable, portable, and maintainable.

- **Compose, Don't Command**

  Our platform is built on the UNIX philosophy. Small, powerful tools (Blueprints, Flows, agentic commands) are composed together using pipes (`|`) to create complex, emergent workflows.
