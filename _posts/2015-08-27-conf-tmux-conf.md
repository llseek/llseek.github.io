---
author: Xiaojie Yuan
layout: post
category: config
title: "[Conf] .tmux.conf"
date: 2015-08-27
---

```apache
# unbind all keys once config is reloaded
unbind-key -a

# set TERM to screen-256color in tmux
set -g default-terminal "screen-256color"

# reload config file
bind r source-file ~/.tmux.conf; display 'config reloaded'

# list current bind keys
bind ? list-keys

# detach
bind d detach-client

# switch between sessions
bind c choose-session

# kill a session
bind k kill-session

# split windows like vim
# vim's definition of a horizontal/vertical split is reversed from tmux's
bind s split-window -v
bind v split-window -h

# move around panes with hjkl, as one would in vim after pressing ctrl-w
#bind h select-pane -L
#bind j select-pane -D
#bind k select-pane -U
#bind l select-pane -R

# resize panes like vim
# feel free to change the "1" to however many lines you want to resize by, only
# one at a time can be slow
bind h resize-pane -L 5
bind l resize-pane -R 5
bind j resize-pane -D 5
bind k resize-pane -U 5

# zoom in/out a window
bind z resize-pane -Z

# enter copy mode
bind [ copy-mode
# vi-style controls for copy mode
setw -g mode-keys vi

# smart pane switching with awareness of vim splits
is_vim='echo "#{pane_current_command}" | grep -iqE "(^|\/)g?(view|n?vim?)(diff)?$"'
bind -n C-h if-shell "$is_vim" "send-keys C-h" "select-pane -L"
bind -n C-j if-shell "$is_vim" "send-keys C-j" "select-pane -D"
bind -n C-k if-shell "$is_vim" "send-keys C-k" "select-pane -U"
bind -n C-l if-shell "$is_vim" "send-keys C-l" "select-pane -R"

# remap screen-clear key <C-l>
bind C-l send-keys 'C-l'
```
