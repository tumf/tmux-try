# tmux-session

A simple frontend script for tmuxinator that automatically creates and manages local tmux session configurations.

## Features

- Automatically creates `.tmuxinator.yml` in project directories
- Supports both tmuxinator project names and directory paths
- Uses current directory if no argument is provided
- Creates project directory if it doesn't exist
- Simple, single-file Bash script with no external dependencies (except tmuxinator)
- **NEW**: Integration with [try](https://github.com/tobi/try) for experiment directory management

## Installation

1. Copy the scripts to a directory in your PATH:
   ```bash
   cp tmux-session /usr/local/bin/tmux-session
   # Optional: Install try integration scripts
   cp try-tmux /usr/local/bin/try-tmux
   cp try-resume /usr/local/bin/try-resume
   ```

2. Make them executable:
   ```bash
   chmod +x /usr/local/bin/tmux-session
   chmod +x /usr/local/bin/try-tmux      # Optional
   chmod +x /usr/local/bin/try-resume    # Optional
   ```

## Usage

### Basic Usage

```bash
tmux-session [<project-dir>|<tmuxinator-name>]
```

### Examples

```bash
# Start in current directory (creates .tmuxinator.yml if needed)
tmux-session

# Start in specific directory
tmux-session ~/projects/myapp

# Start existing tmuxinator project by name
tmux-session myproject

# Create and start in new project directory
tmux-session ./new-project
```

### Integration with `try` (Experiment Directory Management)

If you have [try](https://github.com/tobi/try) installed, you can use these additional features:

#### Option 1: Using `tmux-session --try`

```bash
# Create experiment directory with try and start tmux session
tmux-session --try redis-pool

# This creates ~/src/tries/2026-01-31-redis-pool/ and starts a session
```

#### Option 2: Using `try-tmux` (Recommended)

```bash
# Create new experiment and start tmux session in one command
try-tmux redis-experiment

# Clone a GitHub repo and start tmux session
try-tmux clone https://github.com/user/repo.git
```

#### Option 3: Using `try-resume`

```bash
# Use try's fuzzy search to find existing experiment, then start tmux session
try-resume

# Search for specific experiment
try-resume redis
```

### Installing `try`

**RubyGems (Recommended):**
```bash
gem install try-cli
eval "$(try init)"  # Add to .zshrc or .bashrc
```

**Homebrew:**
```bash
brew tap tobi/try https://github.com/tobi/try
brew install try
```

**Rust version:**
```bash
cargo install try-cli
```

For more details, see the [try documentation](https://github.com/tobi/try).

## Requirements

- **tmux**: Terminal multiplexer
- **tmuxinator**: Tmux session manager
- **bash**: Shell (version 4.0 or later)
- **try**: (Optional) Experiment directory manager for `--try` features

### Installing tmuxinator

**macOS (using Homebrew):**
```bash
brew install tmuxinator
```

**Linux (using gem):**
```bash
gem install tmuxinator
```

**Using package managers:**
```bash
# Debian/Ubuntu
sudo apt install tmuxinator

# Arch Linux
sudo pacman -S tmuxinator

# Fedora
sudo dnf install tmuxinator
```

For more installation options, see the [official tmuxinator documentation](https://github.com/tmuxinator/tmuxinator).

## How it works

The `tmux-session` script intelligently handles two modes:

1. **Tmuxinator project mode**: If the argument matches an existing tmuxinator project name, it runs `tmuxinator start <project-name>`

2. **Local directory mode**: Otherwise, it:
   - Creates the directory if it doesn't exist
   - Changes to that directory
   - Creates a default `.tmuxinator.yml` if not present
   - Runs `tmuxinator local` to start the session

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

You can customize the default template by editing the heredoc section in the `tmux-session` script.

## Customization

Edit the `.tmuxinator.yml` template in the script to change default window/pane layout:

```bash
# Edit the heredoc starting at line 35
cat >.tmuxinator.yml <<'EOF'
# Your custom template here
EOF
```

## Development

See [AGENTS.md](AGENTS.md) for development guidelines, code style, and testing instructions.
