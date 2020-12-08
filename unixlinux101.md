# UNIX and Linux

Some basics of command line usage on UNIX like systems.

## The Quintessial old friends foo, bar, baz and qux

_foo_, _bar_, _baz_ and _qux_ are used as example strings: file names, input data etc. A bit like _hello, world_ in programming.

## Shells

```sh
# delete file with starts with a hyphen
rm -- -foo
```

### TODO

## Danger Zone: Differences Between Operating Systems Can Be Fatal

### TODO 

- BSD vs. System V
- Dangerous differences like killall

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
