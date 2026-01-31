# tmux-try

A simple tmux session manager with seamless [try](https://github.com/tobi/try) integration for experiment directory management.

## Features

- **Seamless try integration**: Automatically uses `try` for experiment management
- Works with or without tmuxinator (automatically detects and adapts)
- Automatically creates `.tmuxinator.yml` when tmuxinator is available
- Falls back to native tmux commands when tmuxinator is not installed
- Supports both tmuxinator project names and directory paths
- Prevents duplicate sessions (attaches to existing session if found)
- Simple, single-file Bash script

## Installation

1. Copy the script to a directory in your PATH:
   ```bash
   cp tmux-try /usr/local/bin/tmux-try
   ```

2. Make it executable:
   ```bash
   chmod +x /usr/local/bin/tmux-try
   ```

3. (Recommended) Install `try` for experiment management:
   ```bash
   gem install try-cli
   eval "$(try init)"  # Add to .zshrc or .bashrc
   ```

## Usage

### Path-based Sessions (Explicit Directory)

Use `./`, `~/`, or `/` prefix for explicit directory paths:

```bash
# Start in specific directory
tmux-try ./myproject
tmux-try ~/projects/myapp
tmux-try /absolute/path/to/project

# Create and start in new directory
tmux-try ./new-project
```

### Name-based Sessions (try or tmuxinator)

Without path prefix, the argument is treated as a name:

```bash
# Start existing tmuxinator project by name
tmux-try myproject

# Filter/create try experiment by name
tmux-try redis
# → Opens fuzzy finder filtered by "redis", or creates new experiment

# Clone a GitHub repo
tmux-try https://github.com/tobi/try
# → Clones to ~/src/tries/2026-01-31-tobi-try/ and starts a session

# Clone via SSH
tmux-try git@github.com:user/repo
```

### Interactive Mode (No Arguments)

```bash
# Open try interactive selector
tmux-try
# → Opens fuzzy finder to select existing experiment or create new one

# If try is not installed, uses current directory
tmux-try
# → Starts session in current directory
```

## Argument Resolution

| Argument | Detection | Behavior |
|----------|-----------|----------|
| `./foo`, `~/foo`, `/foo` | Path prefix | Use as directory |
| `https://...`, `git@...` | Git URL | Clone via try |
| `foo` | Name only | tmuxinator project → try experiment |
| (none) | Empty | try selector → current directory |

## Requirements

- **tmux**: Terminal multiplexer (required)
- **try**: Experiment directory manager (required for name-based sessions)
- **tmuxinator**: Tmux session manager (optional, enhances session configuration)
- **bash**: Shell (version 4.0 or later)

### Installing try

For installation instructions, see the official [try documentation](https://github.com/tobi/try).

### Installing tmuxinator (Optional)

**Note**: tmuxinator is optional. The script will work with plain tmux if tmuxinator is not installed.

**macOS (using Homebrew):**
```bash
brew install tmuxinator
```

**Linux (using gem):**
```bash
gem install tmuxinator
```

## How it Works

1. **Git URL**: If the argument looks like a Git URL, clone via `try` and start session
2. **Explicit path**: If the argument starts with `./`, `~/`, or `/`, use it as a directory
3. **No argument**: Show `try` interactive selector (or use current directory if `try` not installed)
4. **Name only**: Check tmuxinator projects first, then use `try` for experiment management

### Session Reuse

If a tmux session with the same name already exists, it attaches to that session instead of creating a new one.

### Default Configuration

When creating a new `.tmuxinator.yml`, the script uses this template:

```yaml
name: <directory-name>
root: <directory-path>

windows:
  - main:
      panes:
        - zsh
        - opencode
```

You can customize the default template by editing the heredoc section in the `tmux-try` script.

## Development

See [AGENTS.md](AGENTS.md) for development guidelines, code style, and testing instructions.
