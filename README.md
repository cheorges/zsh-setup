# ZSH Setup

This repository contains my personal zsh setup: an [Oh My Posh](https://ohmyposh.dev/) prompt configuration, the Dracula color scheme and a few plugins and CLI tools.

Last checked against the official docs in October 2026 with Oh My Posh 31.8, fzf 0.74, eza 0.23, bat 0.26 and Nerd Fonts 3.5.

## Prerequisites

[Homebrew](https://brew.sh/) must be loaded in `~/.zprofile` (the Homebrew installer prints this line at the end):

```bash
eval "$(/opt/homebrew/bin/brew shellenv)"
```

`brew shellenv` sets `$HOMEBREW_PREFIX` and adds Homebrew's completions to `fpath`. It has to run before `compinit`, otherwise completion for Homebrew-installed tools does not work ([docs](https://docs.brew.sh/Shell-Completion)). On Intel Macs the path is `/usr/local/bin/brew`.

## Setup

1. Install Oh My Posh, the tools and the Homebrew plugins:

   ```bash
   brew install oh-my-posh fzf eza bat zsh-autosuggestions zsh-syntax-highlighting
   ```

2. Install the Meslo Nerd Font and select it in your terminal emulator:

   ```bash
   oh-my-posh font install meslo
   ```

3. Clone the plugins that are not available on Homebrew:

   ```bash
   mkdir -p ~/.config/oh-my-posh/plugins
   git clone https://github.com/agkozak/zsh-z ~/.config/oh-my-posh/plugins/zsh-z
   git clone https://github.com/fdellwing/zsh-bat ~/.config/oh-my-posh/plugins/zsh-bat
   ```

4. Copy the configuration file into your home configuration directory:

   ```bash
   cp .cheorges.omp.json ~/.config/oh-my-posh/
   ```

5. Add the [`.zshrc` block](#zshrc) below to your `~/.zshrc`.

Oh My Posh caches the configuration per shell session. After changing the theme, open a new terminal to see the result.

## Add Dracula color scheme

Add the [Dracula color scheme](https://draculatheme.com/) to your terminal emulator.

## Plugins and tools

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
- [zsh-z](https://github.com/agkozak/zsh-z)
- [zsh-bat](https://github.com/fdellwing/zsh-bat): aliases `cat` to `bat` (the original is kept as `rcat`) and colors man pages
- [fzf](https://github.com/junegunn/fzf)
- [eza](https://github.com/eza-community/eza)
- [bat](https://github.com/sharkdp/bat)

## .zshrc

The order matters: zsh-z before `compinit`, zsh-syntax-highlighting as the last plugin ([docs](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)) and Oh My Posh as the last line ([docs](https://ohmyposh.dev/docs/installation/prompt)).

```bash
# Completion (Homebrew completions come from `brew shellenv` in ~/.zprofile)
source ~/.config/oh-my-posh/plugins/zsh-z/zsh-z.plugin.zsh

autoload -Uz compinit
compinit
zstyle ':completion:*' menu yes select
zstyle ':completion:*' matcher-list '' 'm:{a-zA-Z}={A-Za-z}'
setopt MENU_COMPLETE
setopt no_list_ambiguous

# fzf key bindings and completion (needs fzf 0.48 or later)
source <(fzf --zsh)

export FZF_DEFAULT_OPTS='--color=fg:#f8f8f2,bg:#282a36,hl:#bd93f9 --color=fg+:#f8f8f2,bg+:#44475a,hl+:#bd93f9 --color=info:#ffb86c,prompt:#50fa7b,pointer:#ff79c6 --color=marker:#ff79c6,spinner:#ffb86c,header:#6272a4'

# Aliases. eza needs --icons=auto: a bare --icons takes the next argument as its value.
function lt() {
    local dir=${1:-2}
    eza --icons=auto --long -TL "$dir"
}

# Browse files with a bat preview, Enter opens the selection in VS Code (Tab selects several)
alias here="fzf --multi --preview 'bat --color=always --style=header,grid --line-range :500 {}' --preview-window 'up,60%,border-bottom,~3' --bind 'enter:become(code {+})'"
alias ls="eza --icons=auto"
alias la="eza --long --all --group --icons=auto"
alias gw='./gradlew'

# Plugins, zsh-syntax-highlighting must be the last one
source ~/.config/oh-my-posh/plugins/zsh-bat/zsh-bat.plugin.zsh
source "$HOMEBREW_PREFIX/share/zsh-autosuggestions/zsh-autosuggestions.zsh"
source "$HOMEBREW_PREFIX/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh"

# Prompt, last line. Apple Terminal does not render the Nerd Font glyphs correctly.
if [ "$TERM_PROGRAM" != "Apple_Terminal" ]; then
  eval "$(oh-my-posh init zsh --config ~/.config/oh-my-posh/.cheorges.omp.json)"
fi
```

## Update

```bash
brew upgrade
git -C ~/.config/oh-my-posh/plugins/zsh-z pull
git -C ~/.config/oh-my-posh/plugins/zsh-bat pull
```
