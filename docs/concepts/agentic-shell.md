# Explanation: The Agentic Shell

The `cx` interactive shell is designed as a powerful **"Workspace IDE"**—a stateful environment for composing commands, managing assets, and building workflows. The shell's most advanced capability is its integrated **agentic layer**, which transforms the IDE into an intelligent **reasoning partner**.

This document explains the philosophy and architecture behind the `cx` agent.

## From Composition to Collaboration: The Intent Engine

While `cx` provides powerful primitives for manual work (`|`, `=`), its true potential is unlocked by bridging the gap between high-level **user intent** and low-level **command execution**. This is the core function of the shell's **Intent Engine**. The engine provides a spectrum of assistance, allowing you to choose the right level of collaboration for any task.

- **Fast Path (`//`):** For immediate, in-context assistance, the **Translate** feature acts as a "fast thinking" co-pilot, providing single-shot, natural-language-to-command translation. Its purpose is to lower the cognitive barrier to using the shell and accelerate common tasks.

- **Deliberate Path (`agent`):** For complex, multi-step problems, you engage the full **Reasoner**. This is the "slow thinking" component—a deliberative, stateful partner that can plan and execute complex workflows.

## The CARE Architecture: A Team of Specialists

The `agent` command is powered by the **Composite Agent Reasoning Engine (CARE)**, a hierarchical architecture inspired by how human expert teams operate. Instead of a single, monolithic AI, CARE is a team of specialist agents, each with a distinct role.

### 1. The Planner Agent

- **Role:** The High-Level Strategist.
- **Function:** When you provide a goal (e.g., `agent "Onboard the Spotify API..."`), the Planner is the first agent to act. It looks at your goal and the high-level context of your workspace (available flows, recent commands) and creates a concise, logical, high-level plan. It thinks in terms of meta-actions, like "Compile the blueprint," not specific command syntax.

### 2. The Tool Specialist Agent

- **Role:** The Expert Technician.
- **Function:** The Tool Specialist receives the Planner's strategy and focuses on executing a single step. It is given a "Mission Briefing"—the original goal, the full plan, and any facts discovered so far—to ensure it has all the context it needs. It then uses its knowledge of the shell's grammar and available tools to generate a precise, syntactically perfect `cx` command to accomplish that single step.

### 3. The Analyst Agent

- **Role:** The Observer and Sense-Maker.
- **Function:** After a command is executed, the Analyst agent examines the result (the "observation"). Its job is to interpret what happened. Did the command succeed? Did it reveal a new, important piece of information (like a URL or an ID)? Or did it fail in a way that invalidates the entire plan? The Analyst updates the team's shared understanding (the "belief state") and provides a concise summary, allowing the loop to continue to the next step or escalate back to the Planner for re-planning.

This structured, collaborative process allows the `cx` agent to break down complex problems and solve them more reliably and transparently than a single monolithic model.
