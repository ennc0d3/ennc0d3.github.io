# UID/GID in docker

By default docker run as root user if USER is
specified in the Dockerfile

FIXME:
    Add an example here
The following sleep process in running on host with root user, Pretty dangerous
```
    docker run -d ubuntu:latest sleep infinity
    ps -ef | grep sleep
    root     19819 19801  2 16:37 ?        00:00:00 sleep infinity
```

One more example, If i can bind mount the root fs, we can do the following
```bash
 docker run  -v /root:/root -d ubuntu:latest touch /root/docker-can-touch
sudo ls -l /root/docker-can-touch
[sudo] password for xxxxx:
-rw-r--r-- 1 root root 0 nov 21 16:40 /root/docker-can-touch
```

Getting access as that of the host
##
Create a user with same UID as that of the host, then docker would have the
same rights as that of the host user
