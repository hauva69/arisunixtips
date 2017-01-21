# Unix Tips and Tricks

## Git diff side by side on terminal

### Regular diff

```bash
git difftool -y -x "diff -y -W $COLUMNS" 0338108a7368dfc37ca9ad42a3d8260b0813e3c7...master .
```

### sdiff

```bash
git difftool -y -x "sdiff -w $COLUMNS" 0338108a7368dfc37ca9ad42a3d8260b0813e3c7...master .
```

### colordiff

```bash
git difftool -y -x "colordiff -y -W $COLUMNS" 0338108a7368dfc37ca9ad42a3d8260b0813e3c7...master . | less -R
```

## git search from all branches

```bash
git grep 'search-term' $(git ls-remote . 'refs/remotes/*')
```

## Git log even if directory or file was removed

Prepend the path with double hyphen.

```bash
git log -- directory_or_file_name
```

## All Languages Known to Github

[List of all languages known to Github](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)

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

### npm search Crashes Due to Insuffiecient Memory

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
``

### Rewrite to an URL with a Hashtag

```
RewriteRule "^/japan2007" "/#japan2007" [R=301,NE,L]
```

## Encryption

### Checking an SSL certificate

```bash
echo | openssl s_client -connect google.com:443 2>/dev/null | openssl x509 -noout -dates
```

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

Show dot files in Finder

```
cmd + shift + .
```

## Log from command line or script to the syslog

```bash
echo foo | /usr/bin/logger -t TAG_NAME_FOR_FOO
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
