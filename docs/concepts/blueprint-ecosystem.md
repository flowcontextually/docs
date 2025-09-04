# Explanation: The Blueprint Ecosystem

At the heart of Contextually is the **Blueprint Ecosystem**, a version-controlled, multi-layered package management system for integrations. Understanding this system is key to mastering the full power of the `cx` shell and the Contextually platform.

## The Core Problem

A scalable integration platform must manage the "knowledge" of how to interact with thousands of external services. This includes API endpoints, authentication methods, request parameters, and response shapes. Hardcoding this logic into a monolithic application is fragile and unsustainable. It creates a bottleneck where every new integration requires a change to the core application.

## The Solution: Blueprints as Self-Contained Packages

Contextually solves this by treating each integration as a self-contained, versioned **"package"** or **"Blueprint"**. A blueprint is a directory containing a collection of files that fully describe how to interact with a specific version of an external service.

A complete blueprint package contains:

- **`blueprint.cx.yaml`**: The executable contract. This file defines the base URL, authentication methods, and a list of named `action_templates` for the API.
- **`schemas.py`**: A file containing Pydantic models for the API's data structures and action parameters, providing runtime validation and a better developer experience.
- **`source_spec.json`**: The original OpenAPI or Swagger specification from which the blueprint was compiled, ensuring auditability and reproducibility.
- **`README.md`**: Human-readable documentation for the blueprint.
- **`examples/`**: A directory of sample scripts showing how to use the blueprint.

The key benefits of this approach are directly tied to our core architectural principles:

- **No Breaking Changes:** A workflow that depends on `community/stripe@v1.2.0` will _never_ break when a new version is released, because it points to an immutable, versioned directory.
- **Discoverability:** The system is programmatically queryable, allowing the `cx` shell's tab-completion and future AI agents to discover available capabilities by reading the blueprints.
- **Extensibility:** Users and organizations can extend and override official blueprints to suit their specific needs without modifying the original.
- **Frictionless Contribution:** The process for community members to contribute new blueprints is simple and highly automated via the `cx compile` command.

## The Four-Layer Namespace

The filesystem for blueprints is designed as a cascading namespace that allows for clear ownership and prioritized overrides. When `cx` needs to find a blueprint referenced by a connection (e.g., `community/petstore@v2.0`), it searches for the package inside the `~/.cx/blueprints/` directory.

The namespaces define a clear hierarchy:

#### 1. `user/`

- **Path:** `~/.cx/blueprints/user/`
- **Purpose:** Your personal workspace. This layer has the **highest priority**. Any blueprint you create or modify here will override all others. It is the default location for the `cx compile` command and is perfect for development, testing, and personal scripts.
- **Control:** Fully controlled by you.

#### 2. `organization/`

- **Path:** `~/.cx/blueprints/organization/` (by default)
- **Purpose:** A private, shared namespace for your company. This is typically synced from a private Git repository containing proprietary integrations and enterprise-specific customizations of public blueprints.
- **Control:** Managed by your organization's administrators.

#### 3. `community/`

- **Path:** `~/.cx/blueprints/community/`
- **Purpose:** The open-source "standard library" of integrations. These blueprints are contributed, reviewed, and maintained by the community. The `cx init` command populates this directory with its first example.
- **Control:** Managed by the Contextually open-source project.

#### 4. `system/`

- **Path:** `~/.cx/blueprints/system/`
- **Purpose:** A small number of core, essential blueprints maintained directly by the Contextually core team. These are considered part of the application itself.
- **Control:** Managed by the Contextually core team.

This layered approach provides a powerful and flexible system for managing integrations at every level, from individual developer experiments to enterprise-wide standards.
