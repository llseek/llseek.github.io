---
author: Xiaojie Yuan
layout: post
title: "[Conf] .tmux.conf"
date: 2015-08-27
---

```
# reload config file
bind r source-file ~/.tmux.conf; display 'config reloaded'

# set terminal to 256 color in tmux
set -g default-terminal "screen-256color"

# move around panes with hjkl
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# split windows like vim
bind v split-window -h
bind s split-window -v

# resize panes like vim
bind < resize-pane -L 5
bind > resize-pane -R 5
bind - resize-pane -D 5
bind = resize-pane -U 5
```
