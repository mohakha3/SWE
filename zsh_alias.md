~ (main) $ cat ~/.zshrc
autoload -Uz compinit && compinit
# Load version control information
autoload -Uz vcs_info
precmd() { vcs_info }

# Format the git branch display (e.g., [main])
zstyle ':vcs_info:git:*' formats '(%b)'

# Set the terminal prompt layout
# Enable variable substitution in the prompt
setopt PROMPT_SUBST
# %~ shows the current folder, %F{cyan} changes color, %f resets color
PROMPT='%F{cyan}%~%f %F{green}${vcs_info_msg_0_}%f $ '

# Load external Git aliases
source ~/.git_aliases


~ (main) $ cat ~/.git_aliases
# Custom Git Shortcuts
alias gs="git status"
alias ga="git add"
alias gaa="git add --all"
alias gc="git commit"
alias gcm="git commit -m"
alias gp="git push"
alias gl="git pull"
alias gb="git branch"
alias gco="git checkout"
alias gd="git diff"
alias glog="git log --oneline --graph --decorate"

~ (main) $ 
~ (main) $ 
