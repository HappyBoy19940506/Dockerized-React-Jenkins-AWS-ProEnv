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


# 2.   运行command权限不够要sudo， sudo了又要密码。我ssh连接的，哪来的密码？？

```
pipeline {
agent {
        label'local'
}
    stages {
        stage('Hello') {
            steps {
                sh 'whoami'
                ----> Jenkins 
                sh 'apt-get update'
                sh 'apt install python3-pip -y'
                sh 'pip3 install awscli --upgrade'
                ----> Reading package lists...
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)
W: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)
            }
        }
    }
}
权限不够 --> 抬旗 +_sudo
pipeline {
agent {
        label'local'
}
    stages {
        stage('Hello') {
            steps {
                sh 'whoami'
---------------> jenkins
                sh 'sudo apt-get update'
                sh 'sudo apt install python3-pip -y'
                sh 'sudo pip3 install awscli --upgrade'
--------------> + sudo apt-get update
sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper
            }
        }
    }
}

```
## Resolved: 
```
+ args， 以root身份执行jenkinsifle
pipeline {
agent {
    docker {
        image 'node:12.16.1-alpine'
        label 'local'
        args  '-u root:root'
    }
}
    stages {
        stage('Hello') {
            steps {
                sh 'whoami'
                --> root
                sh 'node -v'
                ---> 12.16.1
            }
        }
    }
}
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
