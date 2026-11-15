# Libraries

Third-party libraries a project needs on disk.

Part of the **Webship Workspace** (`~/workspace`), a container-based harness (DDEV by default). The rules in the root
`CLAUDE.md` apply here too; what follows is what is specific to this folder.

## The rule that matters here

Everything goes through DDEV. Never a host `composer`, `drush` or `mysql`:

```bash
ddev composer require drupal/something
ddev drush cr
ddev export-db --file=…        # not mysqldump
ddev delete -y -O              # not a dropped database
```

`ddev start` takes `-y`; **`ddev stop` does not**. Never write a `$databases` block into
`settings.php` — DDEV's `settings.ddev.php` owns the connection.

## Commands here

```bash
bash cmd-tool-git-change-filemode-to-false.sh         # Bootstrap.
bash cmd-tools-graphify.sh                            # Map a project in this workspace into a queryable knowledge graph with
bash cmd-tools-obsidian.sh                             # Turn a project graph into an Obsidian vault
bash cmd-tools-ragify.sh                              # Index a project in this workspace into the Milvus vector database
```

Every one locates the workspace from its own path, so there is nothing to export and nothing to
install. `--help` on any of them lists its arguments.

## AI items for this folder

The plugin of this folder (`@webship.co/workspace-libraries`) places its own AI items in the AI folders of the
workspace, when they are active:

- `libraries-keeper` (`ai-agents/`): Looks after the third-party libraries of `libraries/`: what is there, which version, from where; runs the checks a library declares; never edits one in place
- `libraries-workflow` (`ai-skills/`): How work is done in `libraries/`: see what is there, add as published, check, leave as published; the commands with their flags, the settings keys
- `/libraries-check` (`ai-commands/`): `/libraries-check <library> [checks]`: prints the plan, runs the checks the library declares and says what is red and why
- `libraries-inventory` (`ai-prompts/`): A new session that lists the libraries on disk with the version, the origin and the licence of each
