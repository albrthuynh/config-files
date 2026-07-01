# Why does this exist?
Albert (me) is a nerd, and likes using tools to make his developer experience not use the mouse as much as possible. This is his own personal configuration files that I use to make my developer experience as pleasant as possible.

# Directions for set up
For the tmux config, make sure that you have tmux installed. `brew install tmux`
And place the tmux file inside `~/.tmux.conf`

For Aerospace (MacOS) make sure you have Aerospace `brew install --cask nikitabobko/tap/aerospace`
And place the aerospace file inside `~/.aerospace.toml`
For some reason it works with version `0.20.3` and not the most up to date one yet

Command to do that
```
osascript -e 'quit app "AeroSpace"'
brew uninstall --cask aerospace
brew install --cask nikitabobko/tap/aerospace@0.20.3
open -a AeroSpace

Verify:

aerospace --version
```
