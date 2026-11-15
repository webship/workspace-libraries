List the third-party libraries on disk in the libraries folder, with the version and the origin of each.

The launch line is for Claude Code, the default; the text between the triple quotes can be given to another AI CLI as it is.

```
claude --permission-mode bypassPermissions """
Take the inventory of the libraries folder: <what-for>
# ~/workspace/libraries · third-party code, kept as published · container-based (DDEV by default)
# nothing is edited, updated, deleted or cloned in this session

1. ls -d ~/workspace/libraries/*/
2. for each library that is a git checkout:
   git -C <library> describe --tags --always
   git -C <library> remote get-url origin
   git -C <library> status -sb
3. for each library that is not a checkout: look for the version in its package.json, its
   composer.json or its changelog, and say when none is stated
4. look for a licence file in each one

Report one line per library: its name, the version or the commit, where it came from, its
licence, and whether its files were changed since they were published.
Change nothing. Ask me before anything is updated.
"""
```
