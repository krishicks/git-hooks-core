# git-hooks-core

> *An opinionated use of the `core.hooksPath` feature released in Git 2.9.*

## ABOUT

This repository is meant to be used as the directory that the Git `core.hooksPath` config value is pointed at.

It exists because using the `core.hooksPath` feature is used in place of any local, repository-specific hooks in `.git/hooks`. This isn't necessarily what you want, as you may have repository-specific hooks that you want to run in addition to some hooks that should be global.

If you use this repository as your `core.hooksPath`, you will be able to:

* Retain any repository-specific hooks in `.git/hooks`
* Add hooks that will apply to all repositories in their respective *.d* folder
* Use multiple files for hooks rather than a single file, as Git expects
* Whitelist specific repositories against specific hooks (See [Whitelists](https://github.com/pivotal-cf/git-hooks-core#whitelists))

And now that the hooks dir is outside of your repository, you can commit the global hooks. Hooray!

## INSTALLATION

Clone this repo to your directory of choice, e.g. $HOME/workspace/git-hooks-core.

```
git clone https://github.com/pivotal-cf/git-hooks-core $HOME/workspace/git-hooks-core
```

## USAGE

Point `core.hooksPath` at the directory you cloned this repo to:

```
git config --global --add core.hooksPath $HOME/workspace/git-hooks-core
```

Add any global hooks you'd like to their respective *.d* folder:

```
chmod +x my-commit-msg-hook
cp my-commit-msg-hook $HOME/workspace/git-hooks-core/commit-msg.d
```

### Whitelists

This repository supports whitelisting repositories against specific files in
the *hook_name*.d folders.

```
.
├── whitelists      # contains a mapping of hooks to whitelists
└── whitelists.d    # contains whitelists
    └── cred-alert  # a whitelist
```

The `whitelists` file within git-hooks-core is used to declare which hooks are
affected by the whitelists that live in `whitelists.d/`. The structure is as
follows:

```
$HOOK_PATH $WHITELIST
```

Where `HOOK_PATH` is a relative path pointing to a file in a *hook_name*.d
folder in the git-hooks-core directory and `WHITELIST` is the name of the file
that resides in `git-hooks-core/whitelists.d/`.

Each whitelist file should contain absolute paths to the repositories that the
whitelist affects. For example, if you had a repository
`/home/username/my-repo` that you wanted to whitelist against a commit-msg hook
called `add_footer`, you would:

1. Create a file, `git-hooks-core/whitelists.d/my-whitelist` with a single entry: `/home/username/my-repo`
1. Add an entry to git-hooks-core/whitelists: `commit-msg.d/add_footer my-whitelist`

## LINKS

* [githooks](https://git-scm.com/docs/githooks)
