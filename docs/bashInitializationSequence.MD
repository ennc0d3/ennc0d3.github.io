# Bash initialization sequence
Recently I was creating an docker image based on theia(cloud-IDE) for golang and i was stuck with user setup/environment issues. I realized the problems are due to my lack of understanding of the bash initialization scripts.

I spent a couple of  hours to understand and experiment the bash initialization sequences, the involved startup files and finally managed to fix my docker image.

This entry is my future reference about the topic.

## Bash shell invocation modes
Bash shell can be invoked either as interactive login shell, interactive shell and non-interactive shell
``` bash [options]
```
### Interactive login shell
    When we are loggin in, the startup files used in this sequence are /etc/profile and ~/bashrc_profile, ~/.bash_login and ~/.profile in order 
    Ofcourse this can be inhibited by usage --noprofile

    When an interactive login shell executes bash execute ~/.bashrc_logout (if exists)

### Interactive non-login shell
 The shell is started at the command-line using a shell program for example $/bin/bash or $/bin/zsh. It can as well be started by running the /bin/su command.

 Additionally, an interactive non-login shell can as well be invoked with a terminal program such as konsole, terminator or xterm from within a graphical environment.

 When the shell is started in this state, it copies the environment of the parent shell, and reads the user-specific ~/.bashrc file for additional startup configuration instructions.

 This could be inhibited by using --norc option. Also we can use --rcfile option to source from an different file rather than using ~/.bashrc

 Typically,  we can use
  ```
  if [ -f ~/.bashrc ]; then . ~/.bashrc fi
  ```

### Non-interactive shell
 The shell is invoked when a shell script is running. In this mode, it’s processing a script (set of shell or generic system commands/functions) and doesn’t require user input between commands unless otherwise. It operates using the environment inherited from the parent shell.

 ```
if [ -n "$BASH_ENV" ]; then . "$BASH_ENV"; fi
```

### Invoke as sh/posix
Bash runs the startup login sequence in case of sh mode and follows the posix standard for startup files for posix mode

## Startup files
#
### System wide
   These applies to all users and usually are /etc/profiles, /etc/bashrc
### User specific
    Customized for the user, usually ~/.bashrc_profile, .bashrc., .bashrc_login

### Login startup files
### Interactive startup files
#
## Example
When is ~/.bashrc executed?
```
  bash; bash
```

When not executed?
```
    bash -c 'echo hi'; bash -c 'echo hi'
```

When is ~/.bashrc_profile and ~/.profile executed?
    At login
```
    bash --login
```
    Inhibit profile
```
    bash --login --no-profile
```

Does shell invoke ~/.bashrc while running a login shell?
    Invoked interactive non-login
    It depends, if the ~/.bashrc_profile has the following line
    ```
    if [ -f ~/.bashrc ]; then . ~/.bashrc fi
    ```

    Invoked non-interactive
    if BASH_ENV


## Tricks
set -a
source ./env
set +a

## References
[Bash manual Startup Files ](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)
