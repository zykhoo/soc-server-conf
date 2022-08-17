The purpose of this documentation is to show how ```pyenv``` can be set up in the server. 
This allows installation of a new version of python, should a new user be unable to use python in the server. 
Note that setting up virtual environments in pyenv requires a plugin that can be found here: https://github.com/pyenv/pyenv-virtualenv
This document does not cover the use of this plugin. 
A user does not need root access to set up pyenv. 

These were done using the following guides:
1. https://github.com/pyenv/pyenv#usage
2. https://jupyter-notebook.readthedocs.io/en/stable/public_server.html
3. https://stackoverflow.com/questions/41560612/jupyter-opening-up-in-w3c

1. ```git clone https://github.com/pyenv/pyenv.git ~/.pyenv```
2. ```ls -la .pyenv```
3. ```cd ~/.pyenv && src/configure && make -C src```
4. ```echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc```
5. ```echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc```
6. ```echo 'eval "$(pyenv init -)"' >> ~/.bashrc```
7. ```echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile```
8. ```echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile```
9. ```echo 'eval "$(pyenv init -)"' >> ~/.profile```
10. ```exec "$SHELL"```
11. ```which pyenv```
12. ```pyenv versions```
13. ```pyenv install 3.7.8```
14. ```pyenv versions```
15. ```pyenv local 3.7.8```
16. ```jupyter notebook --generate-config```
17. set ```NotebookApp.open_browser = False``` and uncomment the line

The following is the set up of ```pyenv``` for a new user, called newuser:
```
newuser@suyeong:~$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
Cloning into '/home/newuser/.pyenv'...
remote: Enumerating objects: 21807, done.
remote: Counting objects: 100% (565/565), done.
remote: Compressing objects: 100% (176/176), done.
remote: Total 21807 (delta 404), reused 497 (delta 365), pack-reused 21242
Receiving objects: 100% (21807/21807), 4.40 MiB | 704.00 KiB/s, done.
Resolving deltas: 100% (14747/14747), done.
Checking connectivity... done.
newuser@suyeong:~$ ls -la .pyenv
total 276
drwxrwxr-x 12 newuser newuser   4096 Aug 17 17:23 .
drwxr-xr-x  7 newuser newuser   4096 Aug 17 17:27 ..
-rw-rw-r--  1 newuser newuser     19 Aug 17 17:23 .agignore
drwxrwxr-x  2 newuser newuser   4096 Aug 17 17:23 bin
-rw-rw-r--  1 newuser newuser  40418 Aug 17 17:23 CHANGELOG.md
-rw-rw-r--  1 newuser newuser  10582 Aug 17 17:23 COMMANDS.md
drwxrwxr-x  2 newuser newuser   4096 Aug 17 17:23 completions
-rw-rw-r--  1 newuser newuser   3380 Aug 17 17:23 CONDUCT.md
-rw-rw-r--  1 newuser newuser   6835 Aug 17 17:23 CONTRIBUTING.md
-rw-rw-r--  1 newuser newuser    680 Aug 17 17:23 Dockerfile
-rw-rw-r--  1 newuser newuser     38 Aug 17 17:23 .dockerignore
drwxrwxr-x  8 newuser newuser   4096 Aug 17 17:23 .git
drwxrwxr-x  3 newuser newuser   4096 Aug 17 17:23 .github
-rw-rw-r--  1 newuser newuser    107 Aug 17 17:23 .gitignore
drwxrwxr-x  2 newuser newuser   4096 Aug 17 17:23 libexec
-rw-rw-r--  1 newuser newuser   1092 Aug 17 17:23 LICENSE
-rw-rw-r--  1 newuser newuser    852 Aug 17 17:23 Makefile
drwxrwxr-x  3 newuser newuser   4096 Aug 17 17:23 man
drwxrwxr-x  3 newuser newuser   4096 Aug 17 17:23 plugins
drwxrwxr-x  5 newuser newuser   4096 Aug 17 17:23 pyenv.d
-rw-rw-r--  1 newuser newuser  24947 Aug 17 17:23 README.md
drwxrwxr-x  2 newuser newuser   4096 Aug 17 17:23 src
-rw-rw-r--  1 newuser newuser 104764 Aug 17 17:23 terminal_output.png
drwxrwxr-x  3 newuser newuser   4096 Aug 17 17:23 test
-rw-rw-r--  1 newuser newuser   1784 Aug 17 17:23 .travis.yml
-rw-rw-r--  1 newuser newuser     35 Aug 17 17:23 .vimrc
newuser@suyeong:~$ cd ~/.pyenv && src/configure && make -C src
make: Entering directory '/home/newuser/.pyenv/src'
gcc -fPIC     -c -o realpath.o realpath.c
gcc -shared -Wl,-soname,../libexec/pyenv-realpath.dylib  -o ../libexec/pyenv-realpath.dylib realpath.o
make: Leaving directory '/home/newuser/.pyenv/src'
newuser@suyeong:~/.pyenv$ cd
newuser@suyeong:~$ echo "$HOME"
newuser@suyeong:~$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
newuser@suyeong:~$ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
newuser@suyeong:~$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc
newuser@suyeong:~$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
newuser@suyeong:~$ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
newuser@suyeong:~$ echo 'eval "$(pyenv init -)"' >> ~/.profile
newuser@suyeong:~$ exec "$SHELL"
newuser@suyeong:~$ which pyenv
/home/newuser/.pyenv/bin/pyenv
newuser@suyeong:~$ pyenv versions
* system (set by /home/newuser/.pyenv/version)
newuser@suyeong:~$ which python
/usr/bin/python
newuser@suyeong:~$ pyenv which python
/usr/bin/python
newuser@suyeong:~$ pyenv install 3.7.8
Downloading Python-3.7.8.tar.xz...
-> https://www.python.org/ftp/python/3.7.8/Python-3.7.8.tar.xz
Installing Python-3.7.8...
patching file Misc/NEWS.d/next/Build/2021-10-11-16-27-38.bpo-45405.iSfdW5.rst
patching file configure
patching file configure.ac
Installed Python-3.7.8 to /home/newuser/.pyenv/versions/3.7.8

newuser@suyeong:~$ pyenv versions
* system (set by /home/newuser/.pyenv/version)
  3.7.8
newuser@suyeong:~$ pyenv local 3.7.8
newuser@suyeong:~$ pyenv versions
  system
* 3.7.8 (set by /home/newuser/.python-version)
newuser@suyeong:~$ pip install numpy
newuser@suyeong:~$ pip install jupyter
newuser@suyeong:~$ jupyter notebook --generate-config
Writing default config to: /home/newuser/.jupyter/jupyter_notebook_config.py

# then in the config file, i changed NotebookApp.open_browser = False and uncommented that line.
# after this, jupyter notebook runs. 
```



