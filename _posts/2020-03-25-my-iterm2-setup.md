---
title:  "My iTerm2 Setup"
excerpt: "Setup for fewer mistakse and fater operations"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/My_iTerm2_Setup_teaser.png
categories:
  - Development
tags:
  - Setup
last_modified_at: 2020-03-25T08:06:00-05:00
---

# Save Logs Automatically

```bash
# go to Preferences(command + ,)
Profiles -> Session -> Automatically log session input to files

# Change saving path
```

# Show Warning On Multiline Paste
go to "Edit" -> "Paste Special" and check as below.

![My_iTerm2_Setup_newline_warning](/assets/images/My_iTerm2_Setup_newline_warning.png)


 # Set Alias
 ```bash
vi ~/.bash_profile
# ----------------
alias ll='ls -la'
alias login='ssh leedonggyu@login~~'
...
# ----------------

# apply
source ~/.bash_profile
 ```


# Shortcut
[iterm2 cheatsheet](https://gist.github.com/squarism/ae3613daf5c01a98ba3a)
```bash
command + n
command + t
command + q
command + ${number} 
command + d
command + shift + d
command + [ or ]
```