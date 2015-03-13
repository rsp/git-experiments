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

Clone PostgreSQL repository to check for interesting data:

```
$ git clone ttps://github.com/postgres/postgres.git
...
$
```

Count the collisions for all hash lengths:
```
$ ./count-collisions postgres
count-collisions from git-experiments
https://github.com/rsp/git-experiments
by Rafał Pocztarski - https://github.com/rsp

Directory:	postgres
Repository:	https://github.com/postgres/postgres.git
GitHub page:	https://github.com/postgres/postgres
Total objects:	49405

Digits: 1, Unique: 16, Colliding: 49405, Noncolliding: 0
Digits: 2, Unique: 256, Colliding: 49405, Noncolliding: 0
Digits: 3, Unique: 4096, Colliding: 49405, Noncolliding: 0
Digits: 4, Unique: 34863, Colliding: 25935, Noncolliding: 23470
Digits: 5, Unique: 48304, Colliding: 2185, Noncolliding: 47220
Digits: 6, Unique: 49334, Colliding: 141, Noncolliding: 49264
Digits: 7, Unique: 49404, Colliding: 2, Noncolliding: 49403
Digits: 8, Unique: 49405, Colliding: 0, Noncolliding: 49405
$
```

Look for those 2 colliding hashes with 7-digit hashes:

```
$ ./list-collisions postgres 7

list-collisions from git-experiments
https://github.com/rsp/git-experiments
by Rafał Pocztarski - https://github.com/rsp

Directory:	postgres
Repository:	https://github.com/postgres/postgres.git
GitHub page:	https://github.com/postgres/postgres
Total objects:	49405
Using digits:	7
Unique: 49404, Colliding: 2, Noncolliding: 49403

Hashes that collide with first 7 digits:
aaeef4d17db9ded501fa02c9ca6c00f86258b171
aaeef4dae843fc9ecab7e6989e3015fda7cbc826

GitHub commit URLs of hashes that collide with first 7 digits:
https://github.com/postgres/postgres/commit/aaeef4d17db9ded501fa02c9ca6c00f86258b171
https://github.com/postgres/postgres/commit/aaeef4dae843fc9ecab7e6989e3015fda7cbc826
$
```

Interesting data
----------------

Some example repositories:

```
git clone https://github.com/rsp/git-experiments.git # 13 commits
git clone https://github.com/jquery/jquery.git # 6593 commits
git clone https://github.com/twbs/bootstrap.git # 11232 commits
git clone https://github.com/postgres/postgres.git # 49405 commits
git clone https://github.com/torvalds/linux.git # 506320 commits
```

Note: Use the https protocol to get GitHub links in the output.

Author
------
Rafał Pocztarski - https://github.com/rsp

License
-------
MIT License (Expat). See [LICENSE.md](LICENSE.md) for details.
