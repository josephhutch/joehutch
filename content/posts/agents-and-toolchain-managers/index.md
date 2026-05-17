---
title: "AI Agents Don’t Think in Shell Sessions"
date: 2026-05-17
description: "AI coding agents expose a subtle flaw in many modern development environments: tools designed around long-lived human shell sessions often break down when commands are executed through short-lived, non-interactive subprocesses. Devbox, worktrees, cloud agent environments, and shell activation all start behaving differently once the “developer” issuing commands is no longer human."
categories: ["Artificial Intelligence"]
toc: false
displayInMenu: false
displayInList: true
draft: false
---

Like many software engineers, I have spent plenty of time over the years trying to keep project dependencies from leaking into each other. One project wants a specific Python version, another needs a different Node runtime, a third depends on a handful of CLI tools, and eventually your laptop turns into a junk drawer of half-remembered setup steps. The usual goal is simple: make each project’s development environment reproducible, isolated, and easy to recreate.

Enter tools like Docker, Devbox, and mise.

I had been using Devbox when a full container felt unnecessary because it gave me clean dependency management and a project-local toolchain without much ceremony. But Devbox exposed a weakness in that model: it made the environment reproducible, but in an AI agent workflow, getting each command to reliably execute inside that environment became its own source of fragility.

## The Crux in a Nut-Shell

What works well for human developers does not always work well for AI agents.

Devbox is a good example of why.

For a human developer, the normal workflow is simple:

```bash
devbox shell
```

You enter the environment once, then continue working inside that initialized shell session. From that point forward, ordinary commands like `python`, `pytest`, `node`, or `task test` run with the tools and environment variables Devbox provided.

That model feels natural because human developers already think in sessions. We open a terminal, establish context, and keep using that context while we work.

AI agents often operate very differently.

In tools like Cursor and Codex, commands are frequently executed as short-lived, non-interactive subprocesses. The agent may run one command, observe the output, then run the next command in a fresh execution context. Any environment mutation that happened inside a prior shell may be gone by the time the next command runs.

That is where `devbox shell` becomes a poor fit. It is designed around entering an environment and staying there. But an agent workflow may not have a durable shell to stay inside.

The fallback is to make every command enter the environment explicitly:

```bash
devbox run <command>
```

That works, but it introduces two problems.

First, environment correctness moves into prompt discipline. The workflow now depends on the agent remembering to wrap every command correctly. One missed `devbox run` and the agent may be operating outside the intended project environment.

Second, it weakens command-level approval ergonomics. Coding tools often let you always allow simple commands like `ls`, `cat`, or `pwd`. But once every command is wrapped as `devbox run <command>`, the approval boundary becomes `devbox` or `devbox run`, not the underlying command. You lose the ability to distinguish between harmless reads and more consequential operations at the permission prompt.

Cloud environments exposed another version of the same issue. In some agent environments, Devbox was not available at all. The repository still had an environment definition, but the tool required to activate that environment was missing.

Worktrees amplified the friction because they create new project checkouts where activation, trust, paths, and setup assumptions all have to be re-established. The environment definition traveled with the repository, but that did not guarantee each agent command was actually running inside the environment.

That was the real problem: not reproducibility, but activation.

## Why mise’s Shim Model Fit Agents Better

After fighting with this for a while, I switched my agent-heavy projects to mise.

The important difference was not that mise is inherently “better” than Devbox. The important difference was that mise’s shim-based execution model fit agents much more naturally.

A shim is a lightweight executable that sits earlier on PATH than the real command. When you run a command like `python`, `node`, or `uv`, you may not be invoking the globally installed binary directly. You are invoking the shim. The shim then inspects the current directory, determines which tool version the project expects, and forwards execution to the correct binary.

That sounds like a small implementation detail, but it changes the interaction model.

With Devbox, the agent first needs to enter the environment correctly before ordinary commands behave properly.

With mise, the command itself resolves correctly at execution time.

Instead of this:

```bash
devbox shell
pytest
```

or this:

```bash
devbox run pytest
```

agents can simply run:

```bash
pytest
```

and rely on the shim layer to resolve the correct runtime and tooling automatically.

That turns out to be a much better fit for agents because agents are fundamentally command executors, not careful shell users. They are very good at running commands and observing outputs. They are much less reliable at preserving invisible shell state across many subprocess executions.

The shim model also keeps the command surface clean and natural:

```bash
uv run pytest
task test
npm run lint
python scripts/example.py
```

Those commands are easier for agents to reason about, easier to auto-approve safely, and less dependent on hidden environment activation steps.

## Applying This Locally, in Worktrees, and in Cloud Environments

Locally, the important thing was making sure mise shims were always available on PATH before the agent started running commands.

Conceptually, the setup is simple:

```bash
~/.local/share/mise/shims
```

needs to appear on PATH before conflicting system binaries.

This is where my Codex shell experiments were useful. Codex was launching short-lived non-interactive shells, so relying on `~/.zshrc` was not enough. The shims became reliable when they were activated in a place that affected the environment Codex inherited before the agent session started.

For me, that meant putting this in `~/.zprofile`:

```zsh
eval "$(mise activate zsh --shims)"
```

Once Codex inherited that PATH, each ephemeral command shell could still find the mise shims, and ordinary commands resolved through the project’s declared toolchain.

Worktrees introduced one extra requirement: trust.

Because mise treats project configuration as potentially sensitive, new worktree paths may need to be trusted before configuration is fully evaluated. You can trust a single project with `mise trust`, and that worked in some setups, but I found it inconsistent across agent workflows. It was workable in Codex setup steps, but less reliable for Cursor worktrees.

The more robust option was to trust the parent directories where those worktrees are created. mise exposes this through the `trusted_config_paths` setting in `~/.config/mise/config.toml`. You can update that setting with the `mise settings` CLI or by editing the config file manually.

For my setup, I added the Codex and Cursor worktree parent directories to my mise config file:

```toml
trusted_config_paths = ["~/.codex/worktrees/", "~/.cursor/worktrees/"]
```

That way, agent-created worktrees under those directories can use their project mise configuration without requiring me to manually trust each new checkout.

Then the worktree itself becomes straightforward:

```bash
mise install
```

At that point, the worktree can install the pinned toolchain from `mise.toml` and `mise.lock`, and agents can immediately begin running ordinary project commands.

Cloud agent environments ended up looking very similar.

One nice detail is that Codex cloud environments already come with mise installed, which made the workflow feel much more natural there. I am not sure whether Cursor’s cloud environments do the same.

My Codex cloud setup instructions simply became:

```bash
mise trust
mise install
```

That mental model felt much healthier. Agents should not depend on inheriting my terminal session. They should be able to clone the repository, trust the configuration, install the declared toolchain, and run commands predictably.

## Agentic Development Changes the Environment Model

The broader lesson here is not “Devbox is bad” or “mise is universally better.” Devbox remains an excellent tool for reproducible development environments.

The real lesson is that AI agents interact with their shell environment in a different way than humans do.

Agents interact with development environments through repeated command execution, often across fresh non-interactive subprocesses. Small activation assumptions that barely matter for humans suddenly become constant sources of friction because agents exercise those paths hundreds or thousands of times.

That is why environment activation complexity matters so much more in agent workflows. The more your tooling depends on hidden shell state, initialization rituals, or session persistence, the more likely those cracks are to show under agents.

The workflows that seem to hold up best are the ones that make correctness explicit at command execution time: pinned toolchains, lockfiles, shims, deterministic setup commands, and predictable task runners.

Humans think in shell sessions.

Agents think in command executions.
