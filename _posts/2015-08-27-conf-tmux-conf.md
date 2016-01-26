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

# detach client
bind d detach-client

# switch client
bind p switch-client -p
bind n switch-client -n

# switch between sessions
bind c choose-session

# kill a pane
bind-key x confirm-before -p "kill-pane #P? (y/n)" kill-pane

# split windows like vim
bind s split-window -v -c "#{pane_current_path}"
bind v split-window -h -c "#{pane_current_path}"

# swap pane
bind-key { swap-pane -U
bind-key } swap-pane -D

# resize panes like vim
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
bind C-p paste-buffer
bind-key -t vi-copy 'v' begin-selection
bind-key -t vi-copy 'y' copy-selection

# smart pane switching with awareness of vim splits
is_vim='echo "#{pane_current_command}" | grep -iqE "(^|\/)g?(view|n?vim?)(diff)?$"'
bind -n C-h if-shell "$is_vim" "send-keys C-h" "select-pane -L"
bind -n C-j if-shell "$is_vim" "send-keys C-j" "select-pane -D"
bind -n C-k if-shell "$is_vim" "send-keys C-k" "select-pane -U"
bind -n C-l if-shell "$is_vim" "send-keys C-l" "select-pane -R"

# remap screen-clear key <C-l>
bind C-l send-keys 'C-l'
```
