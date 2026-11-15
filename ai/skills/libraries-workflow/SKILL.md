---
name: libraries-workflow
description: How work is done in the `libraries/` folder of the Webship Workspace — what a library is there (third-party code kept on disk as it was published), how to say which version is there and where it came from, how the checks a library declares are run, the commands of the folder with their flags and its settings keys. Use when asked "which libraries are on disk", "what version of this library do we have", "run the checks of this library", or when a command of `libraries/` is about to be run.
argument-hint: '[what you are about to do in libraries]'
---

# libraries-workflow

`~/workspace/libraries` is the Libraries folder of a Webship Workspace (container-based, DDEV by default).
Libraries are third-party libraries a project needs on disk: each one a folder, often a git checkout of the library's own project. It comes with the Libraries plugin (`@webship.co/workspace-libraries`), which places the commands below in the folder. Every command finds the workspace from its own path; run it from the folder, and read `bash <command> --help` before quoting a flag.

## The order of work

1. **See what is there**: `ls -d ~/workspace/libraries/*/`, and for a checkout its version with
   `git -C mylibrary describe --tags --always`.
2. **Add one** as its project publishes it (`git clone` at a tag, or the release archive unpacked),
   and say where it came from and under which licence.
3. **Check** one that declares checks: `bash cmd-tools-checks.sh mylibrary --list`, then
   `bash cmd-tools-checks.sh mylibrary`.
4. **Leave it as published.** A change is a patch or a fork, never an edit in place.

## Commands

| Command | What it does | Flags |
| --- | --- | --- |
| `cmd-tool-git-change-filemode-to-false.sh` | Sets `core.fileMode` to false in every checkout of the folder; takes no argument | none |
| `cmd-tools-checks.sh` | Runs the code-quality checks an item's `package.json` declares; `--list` prints the plan and runs nothing | `--browser` `--e2e` `--fail-fast` `--install` `--list` `--only` `--runner` `--skip` |
| `cmd-tools-reclaim.sh` | Disk held by projects whose folder is gone: a dry run unless `--apply` | `--apply` `--keep-days` `--list-protected` `--only` |

## Settings

| Key | File | What it says |
| --- | --- | --- |
| `doc.name` | `core/config/workspace.libraries.settings.yml` | The folder under the workspace root: `libraries` |
| `type` | `core/config/workspace.libraries.settings.yml` | `code`: the rows of the page are packages and repositories, with the Build, Backup, Remove and Checks actions |
| `assistant.name`, `assistant.placeholder` | `core/config/workspace.libraries.settings.yml` | What the assistant is called on this page; `default` takes the workspace's |
| `workspaces` | `core/config/settings.yml` | The active folders; `libraries` is on it once the plugin is activated |

## Rules

- **A library is somebody else's code.** Keep it as it was published: do not edit it in place;
  a change is a patch or a fork, in its own checkout.
- **Say where it came from**: the project, the version or the commit, and its licence, when you
  add one or report on one.
- **Never delete a library unasked**: a project may read it from this folder.
- The workspace is container-based (DDEV by default): a library is used by a project inside that
  project's container, never installed on the host.
- `bash cmd-tools-reclaim.sh` is a report until `--apply` is added: never add it unasked.

The Libraries plugin places these beside this one, each in the workspace folder of its kind: the agent `libraries-keeper` (`ai-agents/`), the command `/libraries-check` (`ai-commands/`) and the prompt `libraries-inventory` (`ai-prompts/`).
