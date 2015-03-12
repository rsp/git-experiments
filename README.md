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
* possibly more...

It should work on any standard Linux/Unix distribution. If it doesn't,
please [sumbit an issue](https://github.com/rsp/git-experiments/issues).

It was written and tested on Ubuntu 14.04.

Commands
--------

### `list-hashes`

Usage: `./list-hashes DIRECTORY`

Example: `./list-hashes .`

### `limit-digits`

Usage: `cat hashes.txt | ./limit-digits NUMBER`

Example: `./list-hashes . | ./limit-digits 7`

Author
------
Rafał Pocztarski - https://github.com/rsp

License
-------
MIT License (Expat). See [LICENSE.md](LICENSE.md) for details.
