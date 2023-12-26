# All you ever wanted to know about Tmux

A bunch of recipies based on my custom tmux.conf. Some of these commands will not work by default. For example, we have remap the prefix key to `C-a`, instead of `C-b`.

## Basics

Create a new tmux session:
`tmux new -s session_name`

Detach session:
`C-a d`

Kill a tmux session:
`tmux kill-sessin -t session_name`

Split panes using | and -
`C-a |`
`C-a -`

Reload tmux config file
`C-a r`

Fast pane switching with Option + arrow keys:
`Opt-Arrows`
Pane switching works with mouse as well.

## Cool stuff

Toggle a panel full screen:
`C-a z`

Rename current window:
`C-a ,`
