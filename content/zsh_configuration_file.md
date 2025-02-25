◀️ [Home](../README.md)

# `.zshrc` file

Zsh (Z Shell) is an extended Unix shell that is compatible with Bash but offers additional features like better auto-completion, improved globbing, spelling correction, and powerful customization options. It is the default shell on macOS since Catalina.

> A Unix shell is a command-line interface that allows users to interact with the operating system by executing commands, running scripts, and managing files. It interprets user commands and provides features for scripting, automation, and system management. Common Unix shells include Bash, Zsh, and Sh.

The `.zshrc` file is the Zsh configuration file. It runs every time a new interactive Zsh shell is started.

It is used to:
- Customize the shell prompt (PS1)
- Set environment variables
- Define aliases and functions
- Enable plugins (e.g., Oh My Zsh)
- Modify shell behavior with options (setopt)

You can open it with:
```bash
nano ~/.zshrc
```

Loading changes After editing `.zshrc`, apply the changes with:
```bash
source ~/.zshrc
```

## Zsh (Z shell) configuration snippet that customizes the command prompt (PS1) 

It dynamically displays information about the current Conda environment, Google Cloud project, and Git repository/branch.

```bash
# Enable dynamic prompt substitution
setopt PROMPT_SUBST
# `setopt PROMPT_SUBST` allows for dynamic evaluation of commands inside PS1. This means that each time the prompt is displayed, embedded commands ($(...)) are executed, updating the prompt dynamically.

# Define colors
RESET="%f"
YELLOW="%F{yellow}"
GREEN="%F{green}"
BLUE="%F{blue}"
CYAN="%F{cyan}"

# Get Conda environment dynamically
CONDA_ENV='$(if [[ -n $CONDA_DEFAULT_ENV ]]; then echo "%F{magenta}($CONDA_DEFAULT_ENV)%f "; fi)'
# Checks if a Conda environment is active ($CONDA_DEFAULT_ENV). If a Conda environment is active, it prints it in magenta. If no Conda environment is active, it shows nothing.

# Get current Google Cloud project dynamically
GCP_PROJECT='$(GCP_PROJ=$(gcloud config get-value project 2>/dev/null); if [[ -n $GCP_PROJ ]]; then echo "${CYAN}[GCP: $GCP_PROJ] "; fi)'
# Runs gcloud config get-value project to get the active Google Cloud project. If a project is set, it displays it in cyan
# `2>/dev/null` suppresses error messages if gcloud is not installed or configured.

# Get active Git repository name
GIT_REPO_BRANCH='$(if git rev-parse --is-inside-work-tree >/dev/null 2>&1; then echo "${YELLOW}$(basename $(git rev-parse --show-toplevel))[$(git rev-parse --abbrev-ref HEAD)]${RESET} "; fi)'
# Checks if the current directory is inside a Git repository (git rev-parse --is-inside-work-tree).

# Set up prompt
PS1="${CONDA_ENV}${GCP_PROJECT}${GIT_REPO_BRANCH}${BLUE}%~ ${GREEN}$ ${RESET}"
```



