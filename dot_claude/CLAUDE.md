# Global Claude Code Guidance

## Git
- Use present tense verbs in commit messages (e.g., "Adds feature", "Fixes bug", "Removes unused code")
- GPG signing is enabled. Before asking Claude to commit, unlock the GPG agent: `echo "test" | gpg --clearsign`. Passphrase is cached for 24 hours.
- SSH key should be loaded in the agent before pushing: `ssh-add --apple-use-keychain`

## Python
- Always use `uv` to manage Python — use `uv run` to execute scripts, `uv pip` for packages, `uv venv` for environments

## Node.js
- Always use `fnm` to manage Node.js versions — use `fnm use` to switch versions, `fnm install` to add new ones

## Dotfiles
- `~/.claude/CLAUDE.md` is managed by chezmoi — always use `chezmoi re-add`, commit, and push in the chezmoi source directory (`~/.local/share/chezmoi`) to persist changes

## Communication
- Short confirmations (y, n, ✅, n1) are normal — not curt, just efficient
- Match that energy: keep responses concise, skip filler
