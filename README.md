# tmux-session

A simple tmux session manager that works with or without tmuxinator, automatically creating and managing tmux sessions.

## Features

- Works with or without tmuxinator installed (automatically detects and adapts)
- Automatically creates `.tmuxinator.yml` when tmuxinator is available
- Falls back to native tmux commands when tmuxinator is not installed
- Supports both tmuxinator project names and directory paths
- Uses current directory if no argument is provided
- Creates project directory if it doesn't exist
- Prevents duplicate sessions (attaches to existing session if found)
- Simple, single-file Bash script
- **NEW**: Integration with [try](https://github.com/tobi/try) for experiment directory management

## Installation

1. Copy the scripts to a directory in your PATH:
   ```bash
   cp tmux-session /usr/local/bin/tmux-session
   # Optional: Install try integration script
   cp try-tmux /usr/local/bin/try-tmux
   ```

2. Make them executable:
   ```bash
   chmod +x /usr/local/bin/tmux-session
   chmod +x /usr/local/bin/try-tmux      # Optional
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

#### Using `try-tmux` (Recommended)

```bash
# Interactive selector - shows all experiments
try-tmux
# → Opens fuzzy finder to select existing experiment or create new one

# Create new experiment and start tmux session
try-tmux redis-experiment
# → Creates ~/src/tries/2026-01-31-redis-pool/ and starts a session

# Search and select existing experiments
try-tmux redis
# → Opens fuzzy finder filtered by "redis"

# Clone a GitHub repo (shorthand - no 'clone' or '.git' needed)
try-tmux https://github.com/tobi/try
# → Clones to ~/src/tries/2026-01-31-tobi-try/ and starts a session

# Clone with custom name
try-tmux https://github.com/user/repo my-fork
# → Clones to ~/src/tries/my-fork/

# Clone using SSH URL
try-tmux git@github.com:user/repo

# Clone from GitLab or other Git hosts
try-tmux https://gitlab.com/user/repo
```

#### Alternative: Using `tmux-session --try`

```bash
# Same functionality as try-tmux
tmux-session --try
tmux-session --try redis-pool
tmux-session --try https://github.com/tobi/try
```



#### Option 2: Using `try-tmux` (Recommended)

```bash
# Create new experiment and start tmux session in one command
try-tmux redis-experiment

# Clone a GitHub repo (shorthand - no 'clone' or '.git' needed)
try-tmux https://github.com/tobi/try
# → Creates ~/src/tries/2026-01-31-tobi-try/ with auto-generated name

# Clone with custom name
try-tmux https://github.com/user/repo my-fork
# → Creates ~/src/tries/my-fork/

# Clone using SSH URL
try-tmux git@github.com:user/repo

# Clone from GitLab or other Git hosts
try-tmux https://gitlab.com/user/repo
try-tmux git@host.com:user/repo
```

**Note**: The `.git` suffix is optional and will be automatically handled by `try`.

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

The `tmux-session` script intelligently handles multiple modes:

1. **Tmuxinator project mode** (if tmuxinator is installed): 
   - If the argument matches an existing tmuxinator project name, it runs `tmuxinator start <project-name>`

2. **Local directory mode with tmuxinator** (if tmuxinator is installed):
   - Creates the directory if it doesn't exist
   - Changes to that directory
   - Creates a default `.tmuxinator.yml` if not present
   - Runs `tmuxinator local` to start the session

3. **Local directory mode without tmuxinator** (fallback):
   - Creates the directory if it doesn't exist
   - Changes to that directory
   - Creates a simple tmux session
   - Attaches to the session

4. **Session reuse**: If a session with the same name already exists, it attaches to that session instead of creating a new one

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
