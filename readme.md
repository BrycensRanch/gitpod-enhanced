
# `gitpod-enhanced`

Turbocharged [Gitpod.io](https://gitpod.io) [workspace image](https://hub.docker.com/r/nberlette/gitpod-enhanced) (built on-top of the official [**gitpod/workspace-full**](https://hub.docker.com/r/gitpod/workspace-full)) with some tasteful enhancements to improve developer experience.

## Getting Started

Add this line to your `.gitpod.yml` file to start using `gitpod-enhanced`:

```yaml
image: nberlette/gitpod-enhanced
```

---

## Features

### Git Enhancements

- Displays git info with ([`git-prompt.sh` from official git repo](https://git.io/git-ps1))
- [GitHub CLI](https://cli.github.com), [`git-extras`](https://github.com/tj/git-extras), `fzf`, and `neovim`
- [GPG support](#gpg-support) for PGP-signing commits (with VSCode integration)

### Package Management

- [`pnpm`](https://npm.im/pnpm), [`degit`](https://npm.im/degit), [`@antfu/ni`](https://npm.im/@antfu/ni) for simple package management
- [`typescript`](https://typescriptlang.org), [`ts-node`](https://npm.im/ts-node), [`ts-standard`](https://npm.im/ts-standard)
- `yarn global bin` prepended to the `$PATH` (e.g. allows command `next` instead of `npx next`!)

### Other terminal addons

- Installs sensible defaults for `ls`, `diff`, `grep` - all with `color`!
- Bundled with many useful aliases, tools, and various other functional upgrades
- Removes duplicate entries from `$PATH` variable ;)

---

## Configuration

### Git Prompt

The `GIT_PS1_` environment variables control the functions and display of the git-integrated shell prompt.

These are the currently available options and their default:

```bash
GIT_PS1_SHOWCOLORHINTS="1"
GIT_PS1_SHOWDIRTYSTATE="1"
GIT_PS1_SHOWSTASHSTATE=""
GIT_PS1_SHOWUNTRACKEDFILES=""
GIT_PS1_SHOWUPSTREAM="auto"
GIT_PS1_OMITSPARSESTATE="1"
GIT_PS1_STATESEPARATOR=""
GIT_PS1_DESCRIBE_STYLE="tag"
GIT_PS1_HIDE_IF_PWD_IGNORED="1"
```
---

### Formatting `PS1`

```bash
GIT_PS1_PREFIX="\[\e]0;\u \W\e\]\[\e[1;7;33m\] \u \[\e[0;7;36m\] \w \[\e[0;1m\] git:("
GIT_PS1_SUFFIX="\[\e[1m\])\[\e[0m\]\n\[\e[1;32;6m\]\$\[\e[0m\] "
GIT_PS1_FORMAT="%s"
```

The last three options are `GIT_PS1_PREFIX`, `GIT_PS1_SUFFIX`, `GIT_PS1_FORMAT`. These allow you to change the colors and format of the surrounding `PS1` prompt string. They have no `git config` equivalent, and must be set in `settings.json`, the Gitpod Dashboard, or through the command `gp env` in the terminal:

```bash
gp env GIT_PS1_PREFIX "\[\e[1m\] \w \[\e[0m\] ... " && eval $(gp env -e)
```

---

### Overrides

You may change/remove any of these (with scope!) in **Dashboard > Settings > Variables**.

#### `.vscode/settings.json`

```jsonc
// .vscode/settings.json
{
  "terminal.integrated.env.linux": {
    "GIT_PS1_SHOWUPSTREAM": "auto verbose name",
    "GIT_PS1_SHOWUNTRACKEDFILES": ""
  }
}
```

#### `.gitpod.yml`

```yaml
# .gitpod.yml
gitConfig:
  bash.showUpstream: "false"
  bash.hideIfPwdIgnored: "false"
```

Note: only ***some*** of the variables have an equivalent `git config` value, which allows you to override them on a per-repository level, right from the `.gitpod.yml` configuration file.

---

## Additional Features

### GPG Support

I've recently included (experimental) support for GPG commit signatures, via the command line or Visual Studio Code UI.

If you create a new PGP key within a Gitpod workspace using the GitHub CLI (`gh`), it's pretty straightforward:

```bash
# find <key-id> using gpg (or in output of `gh keys`)
gpg --list-secret-keys --keyid-format LONG

# save as $GPG_KEY_ID
GPG_KEY_ID="<key-id>"

# export to gitpod
gp env GPG_KEY=$(gpg --export-secret-keys $GPG_KEY_ID | base64 -w 0)

# saves key-id; exports all gp vars to current workspace
eval $(gp env -e GPG_KEY_ID=$GPG_KEY_ID)

# source our .bashrc file for changes to take effect
source ~/.bashrc
```

---

### License

[MIT](https://mit-license.org) © 2022 [Nicholas Berlette](https://github.com/nberlette) • all rights reserved
