# UNIX and Linux

Some basics of command line usage on UNIX like systems.

## Danger Zone: Differences Between Operating Systems Can Be Fatal

There are two basic forms of UNIXes: BSD and System V. The most well known difference is `ps aux` vs `ps -ef`.

### Dangerous differences like killall

- On Linux `killall` kills processes by name.
- On legacy UNIXes `killall` kills everything except the process itself and it's parents.

## The Quintessial old friends foo, bar, baz and qux

_foo_, _bar_, _baz_ and _qux_ are used as example strings: file names, input data etc. A bit like _hello, world_ in programming.

## Shells

_bash_ is the default shell on GNU/Linux systems. On legacy UNIX systems _ksh_
is the most common shell. Many people use _zsh_, which is also the default shell of
recent _OS X_ versions.

_/bin/sh_ has been traditionally used for scripting, but for example the [Google Shell Style Guide](https://google.github.io/styleguide/shell.xml) recommends using 
bash and so do I: it's same everywhere and can be installed on any reasonable UNIX like operating system.

### Nasty Details

Basic usage is simple, but there are lot of things one just has to know like:

```sh
# delete file with starts with a hyphen
ari@víðarr:~/workspace/arisunixtips/example$ mkdir foo && touch foo/-bar && cd foo
ari@víðarr:~/workspace/arisunixtips/example/foo$ pwd
/home/ari/workspace/arisunixtips/example/foo
ari@víðarr:~/workspace/arisunixtips/example/foo$ rm *
rm: invalid option -- 'b'
Try 'rm ./-bar' to remove the file '-bar'.
Try 'rm --help' for more information.
ari@víðarr:~/workspace/arisunixtips/example/foo$ rm -- -bar
```

However, with modern computing power it is seldom necessary to be as efficient possible, but please do think before doing anything destructive. A classic blunder:

```bash
# trying to remove emacs backup files, but include an extra spaces rm *~ was intended, but...
rm * ~
# solution: use it with ls first:
ari@víðarr:~/workspace/arisunixtips$ ls * ~
README.md  docker.md  sql.md  tipsandtricks.md  unixlinux101.md

/home/ari:
go  software  workspace
```

### A Best Practise: use streams, not files

One of fundamental features of UNIX command line (CLI) applications is that 
they are consumers and producers. Complicated, and powerful, pipelines can 
be constructed if every piece of software uses standard input, standard output
and standard error instead of writing to files. It advisable to use redirection 
in order to put any data into files.

Some examples follow.

#### Pipes

##### The Good...

I once had to change a string in a web site with hundreds, if not thousands, of static HTML files. It was something like this:

```bash
ari@víðarr:~/workspace/arisunixtips$ find example/ -name '*.html'  | xargs perl -i -pe 's|http://arska.org|http://aijaruokaa.arska.org|g'
```

##### ...The Bad and Ugly

This is useful, but is it readable?

```bash
sudo tcpdump -A -s 10240 'tcp port 29983 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' | egrep --line-buffered "^........(GET |HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'
```

#### Standard Input

```bash
# command reads input from standard input
command < foo.tx
```

#### Useless Use of cat

_cat_ is short for concatenating i.e. the command concatenates server files to standard output. It's rare that the cat command is needed if the command has only 
one parameter. Use '<' instead.

- (Useless Use of Cat Award)[http://porkmail.org/era/unix/award.html]

#### Redirection

```bash
# redirect to a file
echo foo > bar.txt
# append to a file
echo baz >> bar.txt
# standard error to a file
command 2> err.out
```

It's not too simple, though:

```
  Note that the order of redirections is significant.  For example, the command

              ls > dirlist 2>&1

  directs both standard output and standard error to the file dirlist, while the command

              ls 2>&1 > dirlist

  directs  only the standard output to file dirlist, because the standard error was duplicated from the standard
  output before the standard output was redirected to dirlist.
```

##### An Useful Command: tee

```bash
# write output both to the standard output and a file given as a parameter.
command | tee foo.out
# append
command | tee -a foo.out
```

## File Systems

- df (-k, -h on GNU)
- du (-k, -h on GNU)
- sort (-u, -h on GNU)
- find (. on legacy, -print0 and xargs -0 on GNU)

## Packing and Unpacking

- `gzip` is the most common solution for packing files. gzip removes the original file!
- Ungzip to stdout `gzip -dc foo.txt.gz`.
- use `tar` for directories, compress with gzip: `tar zcf foo.tgz bar baz qux`
- verbose considered harmful. Use `tar ztf foo.tgz` to check the result instead.

## Process Management

### TODO

- killing children and parents
  - kill
  - pkill
  - different signals
- ps, -o parameter, wide 
- pstree
- top

## The Importance of Quoting

- Protect your data by quoting strings.
- The difference between \, " and ' .

## Commands

### ls

List files by modification time.

```bash
ls -ot *.csv | head
```

## Filtering Output

- grep (and egrep and fgrep)
- cut
- sed (TODO)
- awk
- perl
- tail -f *
- tail -f | egrep ...

## XML

## xmllint

A linter for XML. It can:

- Most obviously it can check the well-formedness of files.
- Validate files against a DTD with option _-valid_.

```bash
xmllint --noout foo.xml
```

## What Do I Want to Learn?

- more awk
- bash history expansion

```bash
hauva@víðarr:~/workspace/arisunixtips/example/splitting$ git add fibonacci_with_tab.in
hauva@víðarr:~/workspace/arisunixtips/example/splitting$ git commit -m "!$ added" !$
git commit -m "fibonacci_with_tab.in added" fibonacci_with_tab.in
[unixlinux101 0e48c12] fibonacci_with_tab.in added
 1 file changed, 1 insertion(+)
 create mode 100644 example/splitting/fibonacci_with_tab.in
```
