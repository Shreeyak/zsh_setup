# zsh_setup
Instructions for setting up zsh shell, with oh-my-zsh, powerlevel10k theme and some plugins

![](shell_p10k_2.png)

# Setting up ZSH
ZSH is an alternative to bash. It has many cool features, plugins and themes.
Example: interactive cd, autocorrection of commands, fuzzy finder, syntax highlighting, etc.

Make sure zsh is installed:
```shell script
$ zsh --version
zsh 5.4.2 (x86_64-ubuntu-linux-gnu)
```

If not, install with:
```shell script
sudo apt install zsh

# Make it default shell
chsh -s $(which zsh)
```

## M1 Mac

M1 Macs are built on the ARM64 arch. Many open-source packages are not available for this architecture.
However, the terminal can run apps for x86 architecture using the Rosetta 2 emulator.
This can be done by
1. Starting the terminal with Rosetta 2. Go to ~/Applications, right-click on the terminal -> Get Info. 
  Select the "Open using Rosetta 2" checkbox to force the application to run on the emulator.
2. Start the shell using the arch command: `arch -x86_64 /bin/zsh`. 
    1. If using iTerm2: Preferences -> Profiles -> General tab. Change the drop-down option Command from 
"Login Shell" to "Command" and use the command: `arch -x86_64 /bin/zsh`.
    2. Inside Pycharm: Preferences -> Tools -> Terminal. Under "Application Settings", set "Shell Path" to `arch -x86_64 /bin/zsh`.

This will set the shell to run python (and other apps) on the x86 arch, enabling x86 packages to be installed.
To check if this was successful, in a new terminal type `uname -m` and you'll see the architecture:

```shell script
❯ uname -m
x86_64

```

### Install homebrew using similar command:
```shell script
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

## Install oh-my-zsh
https://github.com/ohmyzsh/ohmyzsh
```shell script
sh -c "$(wget --no-check-certificat -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Switch to PowerLevel10k theme

![](shell_p10k_1.png)

```shell
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Open `~/.zshrc` and modify the theme:
```shell script
# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/robbyrussell/oh-my-zsh/wiki/Themes
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Next time the terminal is started the configure wizard should start up. Select the options to customize the look.
To run the wizard again, call:
```shell
p10k configure
```

### Manual font installation (if configure wizard does not set them up for you)

The powerlevel10k theme uses custom fonts (Meslo NGS). The configure wizard should ask you to install them on first launch. If not, install the
fonts manuall. Download the below 4 tff files:

```shell script
wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf \
https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf \
https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf \
https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf
```

Double-click on each file and click "Install". This will make MesloLGS NF font available to all applications on your system. Configure your terminal to use this font:
- Terminator: Open Preferences using the context menu. Under Profiles select the General tab (should be selected already), uncheck Use the system fixed width font (if not already) and select MesloLGS NF Regular. Exit the Preferences dialog by clicking Close.
- GNOME Terminal (the default Ubuntu terminal): Open Terminal → Preferences and click on the selected profile under Profiles. Check Custom font under Text Appearance and select MesloLGS NF Regular.
- iTerm2: Type p10k configure and answer Yes when asked whether to install Meslo Nerd Font. Alternatively, open iTerm2 → Preferences → Profiles → Text and set Font to MesloLGS NF.
- Apple Terminal: Open Terminal → Preferences → Profiles → Text, click Change under Font and select MesloLGS NF family.

## Install Plugins

### Standard plugins
Clone the following plugin's repos into oh-my-zsh's plugin directory:
- zsh-syntax-highlighting: https://github.com/zsh-users/zsh-syntax-highlighting
- zsh-autosuggestions: https://github.com/zsh-users/zsh-autosuggestions
- zsh-256color: https://github.com/chrissicool/zsh-256color
- zsh-dircolors-solarized: https://github.com/joel-porquet/zsh-dircolors-solarized

```shell script
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/chrissicool/zsh-256color ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-256color
git clone --recursive git://github.com/joel-porquet/zsh-dircolors-solarized $ZSH_CUSTOM/plugins/zsh-dircolors-solarized
```

Within the `~/zshrc` file, search for the line with plugins and add:
```shell script
# Which plugins would you like to load?
# Standard plugins can be found in ~/.oh-my-zsh/plugins/*
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git zsh-autosuggestions zsh-256color zsh-syntax-highlighting zsh-dircolors-solarized)
```

See which dircolor schemes are available:
```shell script
lssolarized
```
Set the "ansi-dark" dircolor scheme:
```shell script
setupsolarized dircolors.256dark
```


### Fuzzy finder
https://github.com/junegunn/fzf
```shell script
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

Running install will add the following lines to `.zshrc`:
```shell script
# Fuzzy Finder
 [ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
```

## Configure ZSH params
Open `~/.zshrc` (zsh config file) and add the below lines. If they already exist, modify them:

```shell script
# Large history file
HISTSIZE=10000000
SAVEHIST=10000000

# Prevent duplicates in history
setopt hist_ignore_all_dups hist_save_nodups

# Um... colors?
# eval `dircolors ~/.dir_colors/dircolors` - This is not the default location of dircolors on ubuntu. Causes errors.
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=250'

# interactive cd
function cd() {
    if [[ "$#" != 0 ]]; then
        builtin cd "$@";
        return
    fi
    while true; do
        local lsd=$(echo ".." && ls -p | grep '/$' | sed 's;/$;;')
        local dir="$(printf '%s\n' "${lsd[@]}" |
            fzf --reverse --preview '
                __cd_nxt="$(echo {})";
                __cd_path="$(echo $(pwd)/${__cd_nxt} | sed "s;//;/;")";
                echo $__cd_path;
                echo;
                ls -p --color=always "${__cd_path}";
        ')"
        [[ ${#dir} != 0 ]] || return 0
        builtin cd "$dir" &> /dev/null
    done
}

# Sorts issues with tmux
export LC_CTYPE=en_US.UTF-8

# Set the autocomplete color (chosen: purple)
# Ref for colors: https://coderwall.com/p/pb1uzq/z-shell-colors
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=62'

# set the zsh no-match behavior is the same as bash (for glob patterns to scp and the like)
setopt nonomatch
```

## Init conda
If using anaconda to manage python environments, init it so you can access `conda` from zsh:
```shell script
conda init zsh
```

