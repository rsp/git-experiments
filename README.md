git-experiments
===============

https://github.com/rsp/git-experiments

Some experiments performed on Git repositories, originally created
to to add some experimental data in my answer on Stack Overflow to:
[How safely can I assume unicity of a part of SHA1 hash?](http://stackoverflow.com/questions/5388781/how-safely-can-i-assume-unicity-of-a-part-of-sha1-hash/5388847#5388847)

This is still work in progress and is not finished yet.

Prerequisites
-------------
* [Bash](https://www.gnu.org/software/bash/)
* [Git](http://git-scm.com/)
* sed, awk, grep, cut, sort, uniq, wc, etc. ([basic Unix tools](http://www.cs.toronto.edu/~maclean/csc209/unixtools.html))
* possibly more...

It should work on any standard Linux/Unix distribution. If it doesn't,
please [sumbit an issue](https://github.com/rsp/git-experiments/issues).

It was written and tested on Ubuntu 14.04.

Commands
--------

### Basic

#### `list-hashes`

Usage: `./list-hashes DIRECTORY`

Example: `./list-hashes .`

#### `limit-digits`

Usage: `cat hashes.txt | ./limit-digits NUMBER`

Example: `./list-hashes . | ./limit-digits 7`

### Complex

#### `list-collisions`

List hash collisions for a certain number of digits (7 by default) in a given repository.

Usage: `./list-collisions DIRECTORY [DIGITS]`

where DIRECTORY is a path to a Git repository (a dot for CWD) and DIGITS is the hash length.

Example:

```bash
git clone https://github.com/jquery/jquery.git
./list-collisions jquery 6
```

#### `count-collisions`

Count the number of hash collisions for every hash length for a given repository.

Usage: `./count-collisions DIRECTORY`

where DIRECTORY is a path to a Git repository (a dot for CWD).

```bash
git clone https://github.com/jquery/jquery.git
./count-collisions jquery
```

Examples
--------

Clone Bootstrap repository and check for 4-digit 

```bash
git clone git@github.com:twbs/bootstrap.git

```

```bash
git clone https://github.com/jquery/jquery.git

```

Author
------
Rafał Pocztarski - https://github.com/rsp

License
-------
MIT License (Expat). See [LICENSE.md](LICENSE.md) for details.
