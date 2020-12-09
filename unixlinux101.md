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
rm -- -foo
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

##### Standard Output

##### tee

## File Systems

### TODO

- df (-k, -h on GNU)
- du (-k, -h on GNU)
- sort (-u, -h on GNU)
- find (. on legacy, -print0 and xargs -0 on GNU)

## Packing and Unpacking

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
- sed
- awk
- perl

## XML

## xmllint

A linter for XML. It can:

- Most obviously it can check the well-formedness of files.
- Validate files against a DTD with option _-valid_.

```bash
xmllint --noout foo.xml
```
## Links
