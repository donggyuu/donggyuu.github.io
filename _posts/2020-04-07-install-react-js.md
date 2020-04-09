---
title:  "Install React JS"
excerpt: "Install React JS On Mac using npx"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/My_iTerm2_Setup_teaser.png
categories:
  - Development
tags:
  - Setup
  - React
last_modified_at: 2020-04-07T08:06:00-05:00
---

<!--TODO
1. link활성화하기
-->

# Install Node.js
Simplest way is using brew
```bash
# npm install
brew install node
```

In old macOS, cannot install Node.js by brew. Please download Node.js from official site.  
→ https://nodejs.org/en/
```bash
# cannot install by brew
Warning: You are using macOS 10.12.
We (and Apple) do not provide support for this old version.
You will encounter build failures with some formulae.
Please create pull requests instead of asking for help on Homebrew's GitHub,
Discourse, Twitter or IRC. You are responsible for resolving any issues you
experience while you are running this old version.
```
<br>

# Create React App
Had better to use "npx" instead of "npm -g" based on official documents.  
https://create-react-app.dev/docs/getting-started/
```bash
# step_1. create React app
npx create-react-app test-app-name
# ------------------
npx: installed 99 in 11.535s
...
Success! Created test-app at /Users/donggyu/Dropbox/git/test-app
Inside that directory, you can run several commands:
...
We suggest that you begin by typing:
  cd test-app
  npm start


# step_2. start React app
cd test-app-name
npm start
# ------------------
Initial page will display automatically
```