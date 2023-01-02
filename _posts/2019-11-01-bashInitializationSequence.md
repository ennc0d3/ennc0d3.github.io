# Bash initialization sequence
Recently, while creating a docker image based on theia(cloud-IDE) for golang, i
had few hiccups with the setup of user environment. The problems that i faced
are due to not having a complete understanding of the bash initialization
sequence and the involved startup files.

After reading the manual and a couple of hours of experiments to understand the
bash initialization sequences, the involved startup files and finally managed to
fix my docker image.

This entry is my future reference on the topic.

## Bash shell invocation modes
Bash shell can be invoked either as interactive login shell, interactive shell
and non-interactive shell

```bash bash [options]```

### Interactive login shell
When we are logging in, the startup files used in this sequence in order are,
* /etc/profile ~/bashrc_profile ~/.bash_login ~/.profile
of course this can be inhibited by usage ```--noprofile```

When an interactive login shell executes bash execute
```~/.bashrc_logout``` (if exists)

### Interactive non-login shell
The shell is started at the command-line using a shell program for example
```/bin/bash``` or ```/bin/zsh``` It can as well be started by running
```/bin/su``` command.

Additionally, an interactive non-login shell can as well be invoked with a
terminal program such as *konsole*, *terminator* or *xterm* from within a
graphical environment.

When the shell is started in this state, it copies the environment of the parent
shell, and reads the user-specific ```~/.bashrc``` file for additional startup
configuration instructions.

This could be inhibited by using ```--norc``` option. Also we can use
```--rcfile``` option to source from an different file rather than using
```~/.bashrc```

Typically, We can use,

```bash
 if [ -f ~/.bashrc ]; then . ~/.bashrc fi
 ```

### Non-interactive shell
The shell is invoked when a shell script is running. In this mode, it’s
processing a script (set of shell or generic system commands/functions) and
doesn’t require user input between commands unless otherwise. It operates using
the environment inherited from the parent shell.

```bash
if [ -n "$BASH_ENV" ]; then . "$BASH_ENV"; fi
```

### Invoke as sh/posix
Bash runs the startup login sequence in case of sh mode and follows the posix
standard for startup files for posix mode

## Startup files ## System wide
These applies to all users and usually are _/etc/profiles_, _/etc/bashrc_

### User specific
Customized for the user, usually ```~/.bashrc_profile```, ```.bashrc,```,
```.bashrc_login```

### Login startup files ## Interactive startup files

## Example(s)
When is ```~/.bashrc``` executed? 
```bash
bash; bash
```

When not executed?
 ```bash
 bash -c 'echo hi'; bash -c 'echo hi'
 ```

When is ```~/.bashrc_profile``` and ```~/.profile``` executed?

At login,

```bash
bash --login
```

To inhibit profile use,

```bash
 bash --login --no-profile
 ```

When does shell invoke ```~/.bashrc``` while running a login shell?

Invoked interactive non-login, it depends, if the ```~/.bashrc_profile``` has the
following line and also on the **BASH_ENV** variable setting,

```bash
 if [ -f ~/.bashrc ]; then . ~/.bashrc fi
 ```

## Tricks
```bash set -a source ./env set +a ```

## References
[Bash manual startup files
](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)
