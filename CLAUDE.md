# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Personal dotfiles repository for macOS, managed by [chezmoi](https://www.chezmoi.io/). Owner: Bill de hÓra (dehora). Manages shell, git, editor, and terminal configuration deployed to `~`.

## Chezmoi Commands

```bash
chezmoi apply           # Apply dotfiles to home directory
chezmoi diff            # Preview changes before applying
chezmoi add <file>      # Add a new dotfile to the repo
chezmoi edit <file>     # Edit a managed file
chezmoi init            # Re-initialize (re-prompts template variables)
chezmoi data            # Show current template variable values
```

## Chezmoi File Naming Conventions

- `dot_` prefix: deployed as dotfile (`dot_zshrc` → `~/.zshrc`)
- `private_dot_` prefix: deployed with mode 0600 (`private_dot_emacs.tmpl` → `~/.emacs`)
- `.tmpl` suffix: processed as Go template before deployment
- `dot_config/` maps to `~/.config/`
- `.chezmoiignore`: prevents files (README.md, LICENSE) from being deployed to `~`

## Templating

Templates use Go template syntax with chezmoi functions. Variables are defined in `.chezmoi.toml.tmpl` using `promptStringOnce` (prompts user once during `chezmoi init`, then cached):

- `git_email`, `git_name`, `git_signingkey` — used in `dot_gitconfig.tmpl`
- `emacs_name`, `emacs_email` — used in `private_dot_emacs.tmpl`

Access variables in templates as `{{ .git_email }}` (top-level data keys).

## Managed Configurations

| Source file | Target | Templated | Notes |
|---|---|---|---|
| `dot_zshrc` | `~/.zshrc` | No | Zsh with oh-my-zsh, Powerlevel10k, fnm/nvm/pyenv/sdkman/rtx/uv/cargo |
| `dot_zprofile` | `~/.zprofile` | No | Zsh login shell profile |
| `dot_p10k.zsh` | `~/.p10k.zsh` | No | Powerlevel10k prompt configuration |
| `dot_gitconfig.tmpl` | `~/.gitconfig` | Yes | GPG signing, user identity from template vars |
| `dot_config/git/ignore` | `~/.config/git/ignore` | No | Global gitignore |
| `private_dot_emacs.tmpl` | `~/.emacs` | Yes | org-mode, org-journal, Dracula theme, Irish keyboard hash key fix |
| `dot_config/ghostty/config` | `~/.config/ghostty/config` | No | Ghostty terminal settings |
| `private_dot_gnupg/gpg-agent.conf` | `~/.gnupg/gpg-agent.conf` | No | 24h passphrase cache TTL |
| `private_dot_gnupg/common.conf` | `~/.gnupg/common.conf` | No | Enables keyboxd backend |

## Git Commits

Use present tense verbs in commit messages (e.g., "Adds emacs", "Ignores license", "Renames gitconfig template").

GPG signing is enabled. Before asking Claude to commit, unlock the GPG agent by running any signing operation (e.g., `echo "test" | gpg --clearsign`). The agent caches the passphrase for 24 hours (`~/.gnupg/gpg-agent.conf`).

## When Editing Dotfiles

- Non-templated files (`dot_zshrc`): edit directly, standard shell syntax
- Templated files (`.tmpl`): use Go template syntax `{{ .varname }}` for variable interpolation; test with `chezmoi diff` before `chezmoi apply`
- Private files (`private_dot_`): will be deployed with restricted permissions; keep secrets in chezmoi template variables, not hardcoded
