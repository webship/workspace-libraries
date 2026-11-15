---
description: Run the code-quality checks a library of the libraries/ folder of the Webship Workspace declares, and say what is red and why, before anything is pushed.
argument-hint: '<library> [checks, comma separated]'
---

# /libraries-check

Check the library of `~/workspace/libraries` named in: `$ARGUMENTS`

The arguments are the text typed after the command: the first word is the library (its folder
name in `libraries/`), and a second word is a comma separated list of checks to run instead of all of
them (`format`, `js`, `css`, `yaml`, `spelling`, `types`, `php`, `twig`, `e2e`). With no library,
stop and ask for one: `ls ~/workspace/libraries` lists the candidates. The workspace is container-based
(DDEV by default); nothing is installed on the host for a check.

## 1. The plan

```bash
cd ~/workspace/libraries
bash cmd-tools-checks.sh <library> --list
```

It prints every check, the script the package declares for it and where it would run, and runs
nothing. A check that is "not declared" is not a failure: say which are declared.

## 2. Run

```bash
bash cmd-tools-checks.sh <library>
bash cmd-tools-checks.sh <library> --only <checks>
```

Use the second form when checks were named. If the plan says the dependencies are not installed,
say so and ask before adding `--install`: an install runs the package's own scripts.

## 3. Report

For each check: passed, failed or not declared; for a failed one the first lines of what it
printed and the file it points at. Do not fix anything unasked, and never skip a check to make
the run pass. For more, hand over to the `libraries-keeper` agent; the
`libraries-workflow` skill lists every command of the folder.
