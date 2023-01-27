# Understanding the docker streams and terminal

It all started with this question why do we have option *-t* and *-i*, okay that is
fine to have separate option just in case we only want to have STDIN connected

But the point that really was not clear is using *-it* that is using both the
terminal and interactive option. 

The manual says,

``````
 -i, --interactive           Keep STDIN open even if not attached
 -t, --tty                   Allocate a pseudo-TTY

 ``````
 Here comes the question, when we have tty allocated via *-t* option why do we
 still need the *-i* option. If that is an TTY then it should probably have the
 STDIN also attached, Isn't?

 Exactly, it looks like STDIN is not connected when a pseudo-TTY is allocated,
 it might be by design.

 I wanted to also understand what is an pseudo-TTY is and how it is different
 from a TTY. Based on my chatGPT interaction and searches , i have come to the
 following understanding,

 A terminal application is a software program that runs on the host machine and
 allows you to interact with that machine through a command-line interface.
 It provides a window in which you can enter commands and see the output.

A pseudo-TTY, on the other hand, is a software-based implementation of a 
terminal that runs on the host machine and provides a terminal-like interface
to other programs or processes running inside a container.
It simulates a terminal interface for the container. The container sees the
pseudo-TTY as a real terminal, while the host machine uses it as an interface to
communicate with the container. The remote machine, in this case, the container,
assumes it as an terminal, it doesn't matter whether the terminal is a real one
or a software-based one.

So in summary, a terminal application is what we use when we are on the host 
machine to interact with the host machine itself, while a pseudo-TTY is a
software-based terminal emulator that runs on the host machine and provides a
terminal-like interface for other programs or processes running inside a 
container.

The catch here is the pseudo-TTY that docker allocates is not like a typical
TTY emulator that we can expect, i.e not all the streams are connected. Only
STDOUT and STDERR are connected. The STDIN is not connected this could be the
docker decision to keep this way.

So we should be using *-i*. I have examples to add but that it is next update.





