---
name: hello
description: Greet someone with the `5dive hello` command. Use when the user asks you to say hello, greet a person or a team by name, or check that the hello plugin works. It is the example skill of the 5dive plugin template; replace it with instructions for your own plugin.
---

# hello

This plugin adds one command to the box: `5dive hello`.

- `5dive hello <name>` prints a greeting, for example `Hello Ada!`.
- `5dive hello config` shows the settings. The owner changes them with
  `sudo 5dive hello config set greeting Hi` or on the dashboard.

When the user asks you to greet someone, run `5dive hello <name>` and pass on
what it prints. If the command is not found, the plugin is not installed or is
disabled; say so, and do not make up the output.

A skill tells the agent WHEN to reach for your plugin and HOW to use it. Keep it
short: what the command does, the one or two ways to call it, and what to do
when it fails.
