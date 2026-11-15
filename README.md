# @webship.co/workspace-libraries

The **Libraries** workspace of the [Webship Workspace](https://github.com/webship/workspace) dashboard:
the `libraries/` folder (type `code`), its settings file (`core/config/workspace.libraries.settings.yml`),
its commands and its docs, as a plugin.

## Use it

```bash
cd ~/workspace
bash core/scripts/plugins.sh --require github:webship/workspace-libraries
bash core/scripts/plugins.sh --activate libraries --with-needs     # puts `libraries` on the workspaces list of settings.yml
bash core/scripts/plugins.sh --install libraries      # the folder, its settings file (once), its commands, its docs (once)
```

The settings file and the docs are placed once and never overwritten: they hold what you put there.
`--deactivate` takes the workspace off the list; the folder and its content stay.

## What it needs

The AI items land in the four AI folders, so the manifest needs them (`needs`): `ai-agents`, `ai-skills`, `ai-commands`, `ai-prompts`. `--activate libraries` and `--install libraries` stop while one is missing and name it; with `--with-needs` they are required, activated and installed first.

## The AI items

The plugin carries its own AI items (`ai/` in the package, the manifest's `ai` block), written for the work this folder is for. `--install` places each one in the workspace folder of its kind, when that folder is an active workspace folder; `--uninstall` removes them, and keeps one that was edited in its folder.

| Item | Kind | Lands in | What it is for |
| --- | --- | --- | --- |
| `libraries-keeper` | Agent | `ai-agents/libraries-keeper.md` | Looks after the third-party libraries of `libraries/`: what is there, which version, from where; runs the checks a library declares; never edits one in place |
| `libraries-workflow` | Skill | `ai-skills/libraries-workflow/` | How work is done in `libraries/`: see what is there, add as published, check, leave as published; the commands with their flags, the settings keys |
| `/libraries-check` | Command | `ai-commands/libraries-check.md` | `/libraries-check <library> [checks]`: prints the plan, runs the checks the library declares and says what is red and why |
| `libraries-inventory` | Prompt | `ai-prompts/libraries-inventory.md` | A new session that lists the libraries on disk with the version, the origin and the licence of each |

From its folder an item is installed into the AI CLI (Claude Code by default) like any other: the folder's page on the dashboard, or `bash cmd-tools-ai-items.sh <name> --install` in the AI folder (`--to dev/<project>` for one project). A prompt is not installed: the New screen form lists it as the start of a new session. The body of every item is plain Markdown any AI CLI can follow; Claude Code is the default shape (the frontmatter, the launch line of a prompt). Every command, flag, settings key and path an item names is checked by `tests/items.test.ts`; the shape of the items is the one of the reference, [`docs/plugin-items-pattern.md` of the WordPress plugin](https://github.com/webship/workspace-wordpress/blob/1.0.x/docs/plugin-items-pattern.md).

## The commands

| Command |
| --- |
| `cmd-tool-git-change-filemode-to-false.sh` |
| `cmd-tools-checks.sh` |
| `cmd-tools-reclaim.sh` |

The bash functions these commands call live in the dashboard's `core/scripts/functions`.
