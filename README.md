# travis_wait_log
tool for running [<img src="https://cdn.travis-ci.com/images/logos/TravisCI-Mascot-1-20feeadb48fc2492ba741d89cb5a5c8a.png" width=32 /> Travis CI][travis] jobs generating more than 4MB of output

## Usage

Travis worker terminates jobs which
[generates more then 4 MiB output][size limit] to the log.
Bigger builds must redirect output from terminal to not exceed log limit, but
jobs not generating [any output for more then 10 minutes][timeout] are
terminated too.

To run bigger builds on Travis CI you should redirect output to file, [generate
some output during build time][travis_wait] and display selected part of
logfile when task is done.

###Parameters

[`travis_wait_log`](travis_wait_log) script (written in [Bash]) do above tasks
for you.

```
travis_wait_log [minutes] [env vars] <command> [args]...
```
where
* `minutes` is optional time in minutes betweend printing dots to output, 5 minutes default
* `env vars` are optional environment variables assigments
* `command` is program to be run with output redirected

###Examples

```
./travis_wait_log LANG=C make all install
./travis_wait_log 5 LANG=C make all install
```
calls make with displaying 1 dot per 5 minutes and printing last 500 lines of
log, setting locale to C.

```
./travis_wait_log 1 make tests
```
run tests with make, displaying 1 dot per minute and printing last 500 lines of
log.

###Additional parameters

You can fine-tune script settings with additional variables:
* `LOG` - logfile name, /tmp/travis_wait.log default
* `LINES` - maximum number of lines from logfile to show, 500 default
* `COLUMNS` - number of dots displayed in one line, 70 default

Example:
```
LOG=/tmp/logfile1.txt LINES=100 ./travis_wait_log 3 LANG=C make all
```

[size limit]: //github.com/travis-ci/docs-travis-ci-com/issues/281
[timeout]: https://docs.travis-ci.com/user/common-build-problems/#My-builds-are-timing-out
[travis]: https://docs.travis-ci.com/
[travis_wait]: https://docs.travis-ci.com/user/common-build-problems/#Build-times-out-because-no-output-was-received
[Bash]: https://www.gnu.org/software/bash/bash.html
