# Unix Tips and Tricks

## Awk

My awk expertise is near non-existent, but I find it to be an extremely useful tool. Some tips collections:

- [10 Awk Tips, Tricks and Pitfalls](https://catonmat.net/ten-awk-tips-tricks-and-pitfalls)
- [Awk One-Liners Explained, Part I: File Spacing, Numbering and Calculations](https://catonmat.net/awk-one-liners-explained-part-one)
- [Awk One-Liners Explained, Part II: Text Conversion and Substitution](https://catonmat.net/awk-one-liners-explained-part-two)
- [Handy awk commands](http://www.dba-oracle.com/t_awk_commands.htm)

## bash

### Google Shell Style Guide

Google has a very useful [Shell Style Guide](https://google.github.io/styleguide/shell.xml).

### shellcheck

[shellcheck](https://www.shellcheck.net/) is a highly useful tool for linting shell scripts. Most shell shell scripts
I've seen fail gloriously when linted. And it is a good thing, because then one can fix them.

## make: Check That a Variable Is Defined

Check that a variable is defined. Note that _ifndef_ and _endif_ are not
intended.

```make
check-env:
ifndef HOST
	$(error HOST is undefined)
endif
```

## Render Markdown Locally

There are several options, one is Grip.

- [grip](https://github.com/joeyespo/grip)

```bash
# On Ubuntu
apt-get install grip
# On OS X
brew install grip
```

## Git

### Setting the Default Editor

```bash
git config --global core.editor vim
```

### Configuring Git to handle line endings

[Configuring Git to handle line endings](https://docs.github.com/en/github/using-git/configuring-git-to-handle-line-endings)

### Git Markdown Supported Languages

- [Github Markdown Supported Languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)

### Git diff side by side on terminal

#### Regular diff

```bash
git difftool -y -x "diff -y -W $COLUMNS" 0338108a7368dfc37ca9ad42a3d8260b0813e3c7...master .
```

#### sdiff

```bash
git difftool -y -x "sdiff -w $COLUMNS" 0338108a7368dfc37ca9ad42a3d8260b0813e3c7...master .
```

#### colordiff

```bash
git difftool -y -x "colordiff -y -W $COLUMNS" 0338108a7368dfc37ca9ad42a3d8260b0813e3c7...master . | less -R
```

### git search from all branches

```bash
git grep 'search-term' $(git ls-remote . 'refs/remotes/*')
```

### Git log even if directory or file was removed

Prepend the path with double hyphen.

```bash
git log -- directory_or_file_name
```

## Combine Several Lines into One Line

```bash
printf '<a href="http://www.example.com/">www</a>\n<a href="http://foo.example.com/">foo</a>\n' | awk -F '"' '{ print $2 }' | paste -s -d '|' -
```

## Calculate the sum of a column in a file

The example takes the second column in a file _bc.in_ and sums the values.

```bash
cut -f 2 -d : bc.in | paste -sd+ - | bc
```

## Processes

### When did a process start?

```bash
ps -p 1955 -o args,lstart
COMMAND                                      STARTED
/usr/lib/jvm/jdk1.6.0_45/bi Thu Dec  8 12:59:25 2016
```

## DNS query from stdin

This works on Red Hat, for other operating systems different kinds of
ways for obtaining the addresses is required.

```bash
egrep ^IPADDR= ifcfg-bond* | cut -f 2 -d = | xargs -l host
```

## HTTP headers with tcpdump

```bash
sudo tcpdump -A -s 10240 'tcp port 29983 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' | egrep --line-buffered "^........(GET |HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'
```

## Chef

### Knife - search by recipe

```bash
knife search node 'recipes:recipe-name\:\:*' -i
```

### Chef - run a single recipe

```bash
chef-client -o 'recipe-name'
```

## Node.js

### Set default Node with nvm

Node.js can be quite a challenge and the ecosystem changes so rapidly
that I recommend using (nvm)[https://github.com/creationix/nvm] in order
to be able easily to change from a Node version to another and back.

```bash
nvm alias default 0.12.7
```

### npm search Crashes Due to Insufficient Memory

Try:

```bash
node --max-old-space-size=4000 `which npm` search angular
```

## Apache

### Location matches everything but the regular expression

```
  <LocationMatch "^((?!/api)/[^/]+/$)">
    ErrorDocument 403 http://www.example.com/403.html
  </LocationMatch>
```

### Set Logging Level by Module

```
LogLevel info mod_rewrite.c:trace3
```

### Rewrite to an URL with a Hashtag

```
RewriteRule "^/japan2007" "/#japan2007" [R=301,NE,L]
```

## Docker

### Set the Clock

```bash
docker run -it --rm --privileged --pid=host debian nsenter -t 1 -m -u -n -i date -u $(date -u +%m%d%H%M%Y)
```

## Encryption

### Checking an SSL certificate

[OpenSSL commands to check and verify your SSL certificate, key and CSR](https://support.asperasoft.com/hc/en-us/articles/216128468-OpenSSL-commands-to-check-and-verify-your-SSL-certificate-key-and-CSR)

```bash
echo | openssl s_client -connect google.com:443 2>/dev/null | openssl x509 -noout -dates
```

## tmux

tmux is a terminal multiplexer i.e. it enables number of terminals (or windows) in which one can create panes and windows, and even more importantly detach and attach sessions.

### The Control-B Problem

tmux uses _ctrl-b_ as the default keystroke for commands and it can be painful, especially to the Emacs users. It can be reconfigured,
though, but the suggested _ctrl-`_ may be problematic MySQL/MariaDB users. Some food for thought:

- [What's the least conflicting prefix/escape sequence for screen or tmux?](https://superuser.com/questions/74492/whats-the-least-conflicting-prefix-escape-sequence-for-screen-or-tmux)
- [Benefits of using backtick (`) in MySQL queries?](https://dba.stackexchange.com/questions/23129/benefits-of-using-backtick-in-mysql-queries)

### The Security Problem

Do not leave windows or panes with with administrative rights (root, database clients etc.) running when detaching a session.

### Commands

This table is based on [A Quick and Easy Guide to tmux](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/). See the manual 
page for more.

| Command | Description |
| ------- | ----------- |
| tmux    | start tmux  |
| tmux new -s \<name\> | start a session with a name |
| tmux rename-session -t \<number\> \<name\> | rename a session |
| tmux ls | list sessions |
| C-b d   | detach the session|
| C-d     | exit pane or window, alternatively, just type _exit_ |
| C-b -t \<number\|name\> | attach to session \<number\|name\> |
| C-b     | the command prefix |
| C-b %   | split horizantally |
| C-b "   | split vertically |
| C-b \<arrow key\> | navigating panes |
| C-b c   | create window |
| C-b p   | previous window |
| C-b n   | next window |
| C-b \<number\> | change to a window number \<number\> |
| C-b z   | maximize and minimize |
| C-b ,   | rename the current window |

## Tar over SSH

```bash
# tar zcvf - /data | ssh root@example.com "cat > /backup/data.tar.gz"
```

## Resizing a PDF file

```bash
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```

## OS X

### Show dot files in Finder

```
cmd + shift + .
```

### Take a screenshot of a window

```
cmd + shift + 4, then space, then click
```

### Make Idea et al Work on Case Sensitive File System

While I kind of like OS X, I do prefer using Linux on Desktop. There are a few major annoyances like
case insensitive file system. This annoyance is easy to get around: split your hard drive in two partitions and make the
non-system disk case sensitive. However, after that IntelliJ software starts to complain. Add to **idea.properties** line

```
idea.case.sensitive.fs=true
```

## Log from command line or script to the syslog

```bash
echo foo | /usr/bin/logger -t TAG_NAME_FOR_FOO
```

## Setting editor defaults

```bash
EDITOR='emacsclient -a ""'
VISUAL="$EDITOR"
FCEDIT='vim'

export EDITOR VISUAL FCEDIT
```

## Emacs

### Incrementally change font size.

Bigger:

```
C-x C-+
```

Smaller:

```
C-x C--
```

### Disable colour

Dark terminal backgrounds often makes life miserable. One way of getting rid of
it is disabling the global-font-lock-mode, permamently with _.emacs_ modification:

```elisp
;; no colours, paint it black
(global-font-lock-mode 0)
```

### Golang autocomplete and autoformat

```elisp
(require 'auto-complete)
(global-auto-complete-mode t)
(add-hook 'before-save-hook 'gofmt-before-save)
(add-to-list 'load-path "~/.emacs.d/vendor/gocode")
(require 'go-autocomplete)
(require 'auto-complete-config)
(add-hook 'before-save-hook 'gofmt-before-save)
```

## Vim

### Showing the carriage returns in the end of the lines

```bash
vim -b foo.md
```

## VS Code

### Too many files for the watching to work

In a Python project there were so many files that VS Code complained for them. Added the *bin/*, *lib/*, *lib64/* and *share/* to *files.watcherExclude* resolved the problem.

```json
{
    "python.pythonPath": "bin/python",
    "editor.codeActionsOnSave": {},
    "files.watcherExclude": {
        "**/.git/objects/**": true,
        "**/.git/subtree-cache/**": true,
        "**/node_modules/*/**": true,
        "**/bin/**": true,
        "**/lib*/**": true,
        "**/share/**": true
      }
}
```

## FTP

### Recursively remove directories and files

Use lftp

```bash
glob -a rm -r *
```

## Finding your disks

```bash
inxi -d
```

## Burning DVD from command line

Replace sr0 with your own device if your's is not /dev/sr0.

```bash
growisofs -dvd-compat -Z /dev/sr0=myiso.iso
```
