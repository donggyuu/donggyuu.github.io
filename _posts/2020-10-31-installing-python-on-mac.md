---
title:  "Installing Python on Mac"
excerpt: "Install python3, pip and Setup VS code"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/python_vsc_setup_1.png
categories:
  - Development
tags:
  - Python
last_modified_at: 2020-10-31T08:06:00-05:00
---
**Video : https://youtu.be/cvsjSedzvB4**

## Install Python3
```bash
# install by brew
brew install python

# check
python3
-------------------------------
don@mac ~ % python3
Python 3.8.6 (default, Oct  8 2020, 14:06:32)
[Clang 12.0.0 (clang-1200.0.32.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>

# set alias
vi ~/.bash_profile
-------------------------------
echo alias python='python3'

# update alias
source ~/.bash_profile

# check using python3
python
-------------------------------
Python 3.8.6 (default, Oct  8 2020, 14:06:32)
[Clang 12.0.0 (clang-1200.0.32.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>

```


## Install pip
```bash
# download "get-pip.py"
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

# install
python get-pip.py

# check
pip --version
------------------
pip 20.2.4 from /usr/local/lib/python3.8/site-packages/pip (python 3.8)
```
https://pip.pypa.io/en/stable/installing/


## Setup VS code

**SETUP**  
Step_1. Download extension for python by Microsoft.  
![python_vsc_setup_1](/assets/images/python_vsc_setup_1.png)
 
Step_2. Write sample python file and run by "F5".  
![python_vsc_setup_2](/assets/images/python_vsc_setup_2.png)

Step_3. Click install button and install "linter-Pylint"  
![python_vsc_setup_3](/assets/images/python_vsc_setup_3.png)

<br>
**When faced an error as below...**

```bash
VSCode: There is no Pip installer available in the selected environment
```
solution_1 - speficy version of python  
```bash
# check which python I used
which python3
---------------------
/usr/local/bin/python3

# restart vs code
# click "There is no Pip installer"
# set "/usr/local/bin/python3" as path
```

solution_2 - setup environment path
```bash
vi ~/.bash_profile
---------------------
export PATH=$PATH:/usr/local/lib/python3.8/site-packages

# update
source ~/.bash_profile
```
https://stackoverflow.com/questions/50993566/vscode-there-is-no-pip-installer-available-in-the-selected-environment
