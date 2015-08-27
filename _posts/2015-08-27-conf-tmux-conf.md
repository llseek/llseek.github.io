---
author: Xiaojie Yuan
layout: post
title: "[Conf] .tmux.conf"
date: 2015-08-27
---

```
bind r source-file ~/.tmux.conf

set-window-option -g mode-keys vi
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

bind v split-window -h
bind s split-window -v
```
