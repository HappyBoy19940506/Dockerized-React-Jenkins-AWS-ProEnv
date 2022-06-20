# 1. Pipeline中，使用agent docker{image xxxx}，报错 docked deamon permission denied.

```
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/json: dial unix /var/run/docker.sock: connect: permission denied
```
## Resolved: 
```
just open terminal and type this command

sudo chmod 666 /var/run/docker.sock

```
https://www.digitalocean.com/community/questions/how-to-fix-docker-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket


2.

3.
