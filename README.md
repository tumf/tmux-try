# start

A simple frontend script for tmuxinator with auto-completion support.

## Installation

1. Copy the `start` script to a directory in your PATH:
   ```bash
   cp start /usr/local/bin/start
   # or
   sudo cp start /usr/local/bin/start
   ```

2. Enable auto-completion:

   **For Bash:**
   ```bash
   # Add to your ~/.bashrc or ~/.bash_profile
   source /path/to/start-completion.bash
   
   # Or copy to system completion directory
   sudo cp start-completion.bash /etc/bash_completion.d/start
   ```

   **For Zsh:**
   ```bash
   # Add to your ~/.zshrc
   source /path/to/start-completion.zsh
   ```

3. Reload your shell configuration:
   ```bash
   # For Bash
   source ~/.bashrc
   
   # For Zsh
   source ~/.zshrc
   ```

## Usage

```bash
start <project_name> [additional_args...]
```

### Examples

```bash
# Start a tmuxinator project
start myproject

# Start with debug mode
start myproject --debug

# Tab completion works just like tmuxinator
start <TAB>  # Shows available projects
```

## Requirements

- tmuxinator must be installed and configured
- bash or zsh shell

## How it works

The `start` script is a simple wrapper that:
1. Passes all arguments to `tmuxinator start`
2. Provides helpful usage information when called without arguments

The completion scripts query `tmuxinator list` to provide project name suggestions, exactly like the native tmuxinator completion.
