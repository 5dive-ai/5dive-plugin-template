# 5dive plugin template

The smallest plugin that installs on [5dive](https://5dive.ai): one command, one skill, and
one setting. Copy it, rename it, and you have a working plugin to build on.

A 5dive plugin adds something to every agent on your server at once, such as a new
`5dive <verb>` command, a skill, or an MCP server. This template adds the command
`5dive hello` and a skill that tells your agents when to use it.

## Try it as it is

On a server with 5dive installed:

```sh
sudo 5dive plugin add 5dive-ai/5dive-plugin-template
5dive hello Ada                          # Hello Ada!
5dive hello config                       # greeting=Hello, style=plain
sudo 5dive hello config set style loud
5dive hello Ada                          # HELLO ADA!
```

Remove it when you are done: `sudo 5dive plugin remove hello@5dive-plugin-template`. Its
saved settings stay in `/var/lib/5dive/plugin-data/hello/` until you delete that folder.

## Make it yours

1. **Create your repository from this one.** On GitHub, press **Use this template**.
2. **Pick a name for your plugin**, for example `weather`. Lowercase letters, digits and
   dashes only. It must not be an existing 5dive command, such as `task` or `agent`. Two
   plugins cannot claim the same command, so a copy still called `hello` is refused on a
   server that already has this template installed.
3. **Rename the parts that carry the name.** They must all match:
   - the folder `hello/` becomes `weather/`
   - `"name"` in `weather/.claude-plugin/plugin.json`
   - `"name"` and `"source": "./weather"` in `.claude-plugin/marketplace.json`
   - the verb: `"verbs": [{"name": "weather", …}]`, `"settings": {"verb": "weather", …}`,
     and the file `weather/bin/hello` becomes `weather/bin/weather`
   - the skill folder `weather/skills/hello/` and the `name:` at the top of its `SKILL.md`
4. **Say who you are.** Set `author`, `homepage` and `fivedive.trust.publisher` in
   `plugin.json`, and `owner` in `marketplace.json`.
5. **Write your plugin.** Put your logic in `bin/<verb>` (any executable works; this one is
   bash). Rewrite `SKILL.md` so it tells an agent when and how to use your command.
6. **Push, then install it:**

   ```sh
   sudo 5dive plugin add <your-github-user>/<your-repo>
   ```

   5dive installs from your repository's default branch, so merge to `main` first.
   **Bump `version` in `plugin.json` every time you publish a change.** 5dive keys each
   install on its version, so a change published without a new version never reaches a
   server that already has the plugin. Those servers pick up a new version with:

   ```sh
   sudo 5dive plugin marketplace upgrade <your-repo>    # fetch what you published
   sudo 5dive plugin upgrade <plugin>@<your-repo>       # install the new version
   ```

You can also point your agent at this repository and ask it to build your plugin from it.
The template is small enough to read in one go.

## What is in it

```
.claude-plugin/marketplace.json     lists the one plugin in this repository
hello/.claude-plugin/plugin.json    the manifest, including its "fivedive" block
hello/bin/hello                     the command `5dive hello` runs
hello/skills/hello/SKILL.md         the skill your agents read
```

The `fivedive` block in `plugin.json` is what makes this a 5dive plugin. It tells 5dive,
before any of your code runs, what the plugin adds and what it needs:

- `"capabilities": ["verb", "skill"]` lists everything the plugin adds. Anything you ship
  but do not list here, 5dive does not register.
- `"verbs"` names the command. 5dive runs `<plugin>/bin/<verb>` with the user's
  arguments and nothing else. A manifest never gets to supply a command line.
- `"settings"` (optional) describes the knobs the 5dive dashboard can show as a form. Your
  command answers `config --json` and `config set <key> <value>`; `bin/hello` shows how.
- `"grants"` (not used here) is where a plugin asks for host resources, such as `network`
  or `fs-home`. The person installing sees every grant, in plain English, before agreeing.
  If you list none, the plugin gets nothing beyond its own folder.

The full rules are in [the 5dive plugin standard](https://github.com/5dive-ai/5dive/blob/main/docs/plugin-contract.md).

## "Not from 5dive": the community warning

Plugins from the 5dive organization install as **official**. A plugin from any other
repository, including one you made from this template, installs as **community**. Before
installing a community plugin, 5dive shows its repository and commit, what it will be
handed, and this line:

> Not from 5dive. It runs with your agents' access; there is no sandbox.

It means exactly that. A plugin runs as your agents do, with the same files and
credentials. 5dive has not reviewed it and cannot switch it off remotely. The off switch
is on your server: `sudo 5dive plugin disable <plugin>@<repo>`, or `remove`. Install
community plugins only from people you would trust with your agents' credentials, and
expect the people installing yours to read your code first.

This template declares `"review": "community"` itself. A manifest can lower its own tier
but never raise it, so the template installs as community even from the 5dive
organization. Its screen there says *"From 5dive's GitHub, but its publisher has not marked
it official"*. Your copy, from your own repository, gets the *"Not from 5dive"* line
above.

## License

MIT. See [LICENSE](LICENSE).
