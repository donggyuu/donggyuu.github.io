---
title:  "Install React JS"
excerpt: "Install React JS On Mac using npx"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/instll_react_js.png
categories:
  - Development
tags:
  - React
  - Javascript
last_modified_at: 2020-04-07T08:06:00-05:00
---

# Install Node.js
Simplest way is using brew
```bash
# npm install
brew install node

# check
node --version
--------------------
v10.15.1
```

In old macOS, cannot install Node.js by brew. Please download Node.js from official site.  
[https://nodejs.org/en/](https://nodejs.org/en/)

```bash
# cannot install by brew
Warning: You are using macOS 10.12.
We (and Apple) do not provide support for this old version.
You will encounter build failures with some formulae.
Please create pull requests instead of asking for help on Homebrew's GitHub,
Discourse, Twitter or IRC. You are responsible for resolving any issues you
experience while you are running this old version.
```

# Create React App
Had better to use "npx" instead of "npm -g" based on official documents.  
[https://create-react-app.dev/docs/getting-started/](https://create-react-app.dev/docs/getting-started/)

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

# Handling Errors

## errors_case1   
If "Permission denied" error occur when start npm, make sure that react-scripts binary is executable.
```bash
# ERROR - Permission denied 
> test-app@0.1.0 start /Users/donggyu/Dropbox/git/test-app
> react-scripts start

sh: /Users/donggyu/Dropbox/git/test-app/node_modules/.bin/react-scripts: Permission denied
npm ERR! code ELIFECYCLE
npm ERR! errno 126
npm ERR! test-app@0.1.0 start: `react-scripts start`
npm ERR! Exit status 126
npm ERR!
npm ERR! Failed at the test-app@0.1.0 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/donggyu/.npm/_logs/2020-04-07T11_26_37_733Z-debug.log
```

**Solution**  
please make scripts as executable
```bash
# Solution
chmod +x node_modules/.bin/react-scripts
```

## errors_case2    
Below errors can be occur when you clone your React project from git. It because there were no "node_modules" for npm.   
```bash
$ npm start

> is-fukushima@0.1.0 start /Users/don/Dropbox/git/is-fukushima
> react-scripts start

sh: react-scripts: command not found
npm ERR! file sh
npm ERR! code ELIFECYCLE
npm ERR! errno ENOENT
npm ERR! syscall spawn
npm ERR! is-fukushima@0.1.0 start: `react-scripts start`
npm ERR! spawn ENOENT
npm ERR!
npm ERR! Failed at the is-fukushima@0.1.0 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
npm WARN Local package.json exists, but node_modules missing, did you mean to install?

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/don/.npm/_logs/2020-12-10T10_14_09_297Z-debug.log
```

**Solution**  
please install npm
```bash
# Solution
npm install
```