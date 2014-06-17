# Git Hooks
This is a repository of both server and client git hooks,
all of which are designed to enforce a certain policy of working properly with git.

Consider using [Git Hooks](https://github.com/icefox/git-hooks) for easier management.

## Installation
Since hooks are not distributed automatically via git,
there should be a local template directory to pull the hooks from for each project.

This is done by:

``` bash
git clone https://github.com/iwfmp/git $HOME/.git.d
git config --global init.templatedir '$HOME/.git.d/templates'
```

Then these cases apply:

- New projects    = the hooks should be copied automatically.
- Old projects    = `git init` should be run from the project directory.
- Cloned projects = `git init` should be run from the project directory (I think).

## Local Hooks
### commit-msg
Enforce Commit Message Conventions:

- Summary line syntax must be `<type>(<scope>): <subject>`.
- Summary line must be 70 characters or less.
- Second line should be empty.
- No other line should be over 80 characters long.

Basically following [AngularJS Commit Message Conventions](https://github.com/camunda/camunda.org/blob/master/COMMIT_MESSAGES.md).

## Server Hooks
### pre-receive
See [hook-chain hook](#hook-chain)

### pre-receive.branch
Enforce Branch Management Conventions:

- There are only 3 available upstream branches: Master, Version/* and Hotfix/*.
- Master branch accepts only non-forwarded merge commits, one at a time.
- Version and Hotfix branches accept only fast-forward/rebased commits.
- Version and Hotfix branches must diverge from Master branch directly.
- Hotfix and Version branches can not be worked on after beign merged to master.
- Hotfix and Version branches can only be deleted after being fully merged to master - LOCAL ONLY.

- Master branch must only be merged into from Version or Hotfix branches - TODO
- Only one Version/Hotfix branch is allowed upstream at a time - TODO
- Hotfix branch should be both merged to Master and rebased on top latest Version  - TODO
- Push -f is not allowed - TODO

### pre-receive.message
See [commit-msg hook](#commit-msg)

Server implementation to enforce commit-msg hook.

### pre-receive.version
To be updated...

## Helper Scripts
### hook-chain
Designed to allow chaining of hooks for a smart separation of behavior.
The hook should be symlinked as the desired hook <E.g. update>,
after which every <hook-name>.* will iterated through and applied when the hook is run.
Basically the same design as the (soon-to-be?) gitlab update hook.