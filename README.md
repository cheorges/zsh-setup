# ZSH Setup

This repository contains my personal configuration for [Oh My Posh](https://ohmyposh.dev/).

## Setup

1. Install Oh My Posh: `brew install oh-my-posh`
2. Install the Meslo Nerd Font: `oh-my-posh font install`
3. Download the configuration file and place it in your home configuration direcotry (e.g. `~/.config/oh-my-posh/`)
4. Load the configuration in your shell profile (e.g. `~/.zshrc`)

   ```bash
   eval "$(oh-my-posh init zsh --config '~/.config/oh-my-posh/.cheorges.omp.json')"

   if [ "$TERM_PROGRAM" != "Apple_Terminal" ]; then
     eval "$(oh-my-posh init zsh)"
   fi
   ```

## Add draclula color scheme

Add the [draclula color scheme](https://draculatheme.com/) to your terminal emulator.

## Add plugins

Recommended plugins:

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
- [zsh-z](https://github.com/agkozak/zsh-z)
- [zsh-bat](https://github.com/fdellwing/zsh-bat)
- [fzf](https://github.com/junegunn/fzf)
- [eza](https://github.com/eza-community/eza)

Check you have activate the plugins in your `.zshrc` file:

```bash
source /opt/homebrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /opt/homebrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source ~/.config/oh-my-posh/custom/plugins/zsh-z/zsh-z.plugin.zsh
```

### Additional aliases and setups

Add the following lines to your `.zshrc` file:

```bash
eval "$(fzf --zsh)"

export FZF_DEFAULT_OPTS='--color=fg:#f8f8f2,bg:#282a36,hl:#bd93f9 --color=fg+:#f8f8f2,bg+:#44475a,hl+:#bd93f9 --color=info:#ffb86c,prompt:#50fa7b,pointer:#ff79c6 --color=marker:#ff79c6,spinner:#ffb86c,header:#6272a4'

function lt() {
    local dir=${1:-2}
    eza --icons --long -TL $dir
}

alias here="fzf --ansi --preview 'bat --color=always --style=header,grid --line-range :500 {}' --preview-window 'up,60%,border-bottom,+{2}+3/3,~3' --bind 'enter:become(code {1} +{2})'"
alias ls="eza --icons"
alias la="eza --long --all --group --icons"
alias lt=lt
alias gw='./gradlew'

zstyle ':completion:*' menu select
```
