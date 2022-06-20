# 1. Pipeline中，使用agent docker{image xxxx}，报错 docked deamon permission denied.

```
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/json: dial unix /var/run/docker.sock: connect: permission denied
```
## Resolved: 
```
just open terminal and type this command // 在运行docker的那个agent里面 ，给 docker daemon抬旗， 直接最高权限运行

sudo chmod 666 /var/run/docker.sock

```
https://www.digitalocean.com/community/questions/how-to-fix-docker-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket


# 2. 

```
运行一些命令时： 比如 apt-get update / aws cli install 这些命令时候， 提示: permission denied.


```
## Resolved: 
```
```

# 3. 

```
Grs/jso
```
## Resolved: 
```
```


# 3. 

```
Grs/jso
```
## Resolved: 
```
```


# 3. 

```
Grs/jso
```
## Resolved: 
```
```



# 3. 

```
Grs/jso
```
## Resolved: 
```
```



# 3. 

```
Grs/jso
```
## Resolved: 
```
```




# 3. 

```
Grs/jso
```
## Resolved: 
```
```



# 3. 

```
Grs/jso
```
## Resolved: 
```
```
