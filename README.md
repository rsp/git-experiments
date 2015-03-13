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
git clone https://github.com/gcc-mirror/gcc.git # 188174 commits
git clone https://github.com/torvalds/linux.git # 506320 commits
```

(numbers of commits as of 2015-03-13)

Note: Use the https protocol to get GitHub links in the output.

Example data from 2015-03-13:

Project | Total | 1DC | 2DC | 3DC | 4DC | 5DC | 6DC | 7DC | 8DC | 9DC | 10DC
------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ----
[jquery](https://github.com/jquery/jquery) | 6593 | 6593 | 6593 | 5231 | 590 | 41 | 4 | 0 | 0 | 0 | 0
[bootstrap](https://github.com/twbs/bootstrap) | 11232 | 11232 | 11232 | 10519 | 1786 | 118 | 6 | 0 | 0 | 0 | 0
[postgres](https://github.com/postgres/postgres) | 49405 | 49405 | 49405 | 49405 | 25935 | 2185 | 141 | 2 | 0 | 0 | 0
[gcc](https://github.com/gcc-mirror/gcc) | 188174 | 188174 | 188174 | 188174 | 177476 | 30726 | 2092 | 144 | 10 | 0 | 0
[linux](https://github.com/torvalds/linux) | 506320 | 506320 | 506320 | 506320 | 506068 | 193608 | 14947 | 924 | 54 | 0 | 0

(Total - total number of hashes, NDC - N-digit collisions)

A number of collisions is defined as the total number of hashes that collide with some other hashes (e.g. 3 hashes with the same prefix are counted as 3 colliding hashes).

Conclusions
-----------
I tested few popular Git repositories of various sizes and searched for collisions of the commit hashes prefixes of different sizes - this is if you only count commits and not other objects.

jQuery has two pairs of colliding 6-digit commit hashes:

* https://github.com/jquery/jquery/commit/a5dbca4
* https://github.com/jquery/jquery/commit/a5dbcaf
* https://github.com/jquery/jquery/commit/f717260
* https://github.com/jquery/jquery/commit/f717261

so eg. this is ambiguous:

* https://github.com/jquery/jquery/commit/a5dbca

and gives 404 even though a shorter one:

* https://github.com/jquery/jquery/commit/66975

works fine (at least for now).

Bootstrap has 3 pairs of 6-digit collisions, e.g.:

* https://github.com/twbs/bootstrap/commit/2a47034
* https://github.com/twbs/bootstrap/commit/2a47037

PostgreSQL has a 7-digit collision:

* https://github.com/postgres/postgres/commit/aaeef4d1
* https://github.com/postgres/postgres/commit/aaeef4da

And Linux has 54 colliding 8-digit commit hashes, e.g.:

* https://github.com/torvalds/linux/commit/05fda3b1a
* https://github.com/torvalds/linux/commit/05fda3b1d

When you get all of the objects with `git rev-list --all --objects` and not only `git rev-list --all` then you get even more collisions (but it takes much longer).
For example here are two pairs of colliding 11-digit hashes in the Linux repo:

```
d597639e2036f04f0226761e2d818b31f2db7820
d597639e203a100156501df8a0756fd09573e2de
ef91b6e893a00d903400f8e1303efc4d52b710af
ef91b6e893afc4c4ca488453ea9f19ced5fa5861
```

so for example:

```
$ git show d597639e203
error: short SHA1 d597639e203 is ambiguous.
error: short SHA1 d597639e203 is ambiguous.
fatal: ambiguous argument 'd597639e203': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
$
```

Author
------
Rafał Pocztarski - https://github.com/rsp

License
-------
MIT License (Expat). See [LICENSE.md](LICENSE.md) for details.
