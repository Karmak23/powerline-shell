Powerline style prompt for Bash (and now, ZSH)
==============================================

A powerline like prompt for Bash/ZSH. This versions adds the following features:

- **user@hostname** indicator on the left with different colors for each, usefull when you work on many machines (you must have the powerline patched font installed on each, but this is more or less easily done via my [sparks](https://github.com/Karmak23/sparks) deployment tool);
- **root user** (=uid 0) **indicator** via username change, color and “ # ” instead of “ $ ” at prompt end, usefull when you `sudo -s` a lot on each of you machines.
- **slight color changes**, which is just a matter of taste, but I find my colors less agressive visually, and the *dirty* color is now different from the *failed* one; these 2 statuses mean 2 different things, having different colors (albeit nearby) holds more sense to me.

![Powerline screenshot in an OSX terminal with bash](https://raw.github.com/Karmak23/powerline-shell/master/powerline-screenshot.png)

It retains the original features of the [original code](https://github.com/milkbikis/powerline-shell) I forked, which:

*  Shows some important details about the git branch:
    *  Displays the current git branch which changes background color when the branch is dirty
    *  A '+' appears when untracked files are present
    *  When the local branch differs from the remote, the difference in number of commits is shown along with '⇡' or '⇣' indicating whether a git push or pull is pending
*  Changes color if the last command exited with a failure code
*  If you're too deep into a directory tree, shortens the displayed path with an ellipsis
*  Shows the current Python [virtualenv](http://www.virtualenv.org/) environment
*  It's all done in a Python script, so you could go nuts with it

# Notes

`powerline-shell` uses ANSI color codes to display colors in a terminal. These are notoriously non-portable, so may not work for you out of the box. Try setting your `$TERM` environment variable to **xterm-256color** (that worked for me). 

I successfully tested it with:

- [iTerm2](http://www.iterm2.com/) On OSX 10.8
- [Terminator](https://launchpad.net/terminator) and [GNOME Terminal](https://help.gnome.org/users/gnome-terminal/stable/)) on Ubuntu 12.04 and 12.10

It has small issues with:

- `Terminal.app` on OSX 10.8 (harmless — not disturbing — color shift on segment separators); using `xterm-256colors` doesn’t help.


# Installation

* Patch the font you use for your terminal: see https://github.com/Lokaltog/vim-powerline/wiki/Patched-fonts

* Clone this repository somewhere:

        git clone https://github.com/Karmak23/powerline-shell

* Now add the following to your .bashrc:

        function _update_ps1() {
           export PS1="$(~/path/to/powerline-shell.py $?)"
        }

        export PROMPT_COMMAND="_update_ps1"

* ZSH fans, add the following to your .zshrc:

        function powerline_precmd() {
          export PS1="$(~/path/to/powerline-shell.py $? --shell zsh)"
        }

        function install_powerline_precmd() {
          for s in "${precmd_functions[@]}"; do
            if [ "$s" = "powerline_precmd" ]; then
              return
            fi
          done
          precmd_functions+=(powerline_precmd)
        }

        install_powerline_precmd

* Fish users, redefine `fish_prompt` in ~/.config/fish/config.fish:

        function fish_prompt
            ~/path/to/powerline-shell.py $status --shell bare
        end
