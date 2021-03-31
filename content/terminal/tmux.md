---
draft: true
title: Tmux
categories:
  - Terminal
---
### Fix stale ssh-auth-sock envvar when rejoining detached sessions
```
eval $(tmux showenv -s SSH_AUTH_SOCK)
```
Reference https://stackoverflow.com/questions/21378569/how-to-auto-update-ssh-agent-environment-variables-when-attaching-to-existing-tm


### Vim keybindings for tmux

- [vim style tmux config · GitHub](https://gist.github.com/tsl0922/d79fc1f8097dde660b34)

### Copy and paste

- [Everything you need to know about Tmux copy paste - Ubuntu &middot; rushiagr](https://www.rushiagr.com/blog/2016/06/16/everything-you-need-to-know-about-tmux-copy-pasting-ubuntu/)
- setup copy to system clipboard
- [Copying between tmux buffers and the system clipboard](http://blog.joncairns.com/2013/06/copying-between-tmux-buffers-and-the-system-clipboard/)

### Managing sessions
`tmux new -s session_name` - creates a new tmux session named session_name
`tmux attach -t session_name` - attaches to an existing tmux session named session_name
`tmux switch -t session_name` switches to an existing session named session_name
`tmux ls` - lists existing tmux sessions
`tmux detach (prefix + d)` - detach the currently attached session
`tmux new-window (prefix + c)` create a new window
`tmux select-window -t :0-9 (prefix + 0-9)` - move to the window based on index
`tmux rename-window (prefix + ,)` - rename the current window

### Managing panes
`prefix + b + Spacebar` - rotate through layouts
`tmux split-window (prefix + ")` - splits the window into two vertical panes
`tmux split-window -h (prefix + %)` - splits the window into two horizontal panes
`tmux swap-pane -[UDLR] (prefix + { or })`- swaps pane with another in the specified direction
`tmux select-pane -[UDLR]` - selects the next pane in the specified direction
`tmux select-pane -t :.+` - selects the next pane in numerical order

`tmux list-keys` - lists out every bound key and the tmux command it runs
`tmux list-commands` - lists out every tmux command and its arguments
`tmux info` - lists out every session, window, pane, its pid, etc.
`tmux source-file ~/.tmux.conf` - reloads the current tmux configuration (based on a default tmux config)
