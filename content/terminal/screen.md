---
draft: true
title: Screen
categories:
  - Terminal
---
You can do it in screen the terminal multiplexer.

To split vertically: ctrla then |.
To split horizontally: ctrla then S (uppercase 's').
To unsplit: ctrla then Q (uppercase 'q').
To switch from one to the other: ctrla then tab
Note: After splitting, you need to go into the new region and start a new session via ctrla then c before you can use that area.

EDIT, basic screen usage:

New terminal: ctrla then c.
Next terminal: ctrla then space.
Previous terminal: ctrla then backspace.
N'th terminal ctrla then [n]. (works for n∈{0,1…9})
Switch between terminals using list: ctrla then " (useful when more than 10 terminals)
Send ctrla to the underlying terminal ctrla then a.