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
---------------> jenkins  or unknown userid 1312312
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
# 3. 各种command not found

```
xxx command not found
```
## Resolved: 
```
1. 考虑所处的agent。 这台agent上 装了什么？有什么环境？

2.考虑是不是在docker +image的环境下，这种container里 有什么环境？
```

# 4. react deploy to S3 index.html显示空白的问题
```

```
## Resolved: 
```
1. folder
2. package.json
```


# 5. multiple-agent-labels-in-a-declarative-jenkins-pipeline

## Resolved: 
```
docker
agent {
    docker {
        image 'maven:3.8.1-adoptopenjdk-11'
        label 'my-defined-label'
        args  '-v /tmp:/tmp'
    }
}


agent {
    docker {
        image 'myregistry.com/node'
        label 'my-defined-label'
        registryUrl 'https://myregistry.com/'
        registryCredentialsId 'myPredefinedCredentialsInJenkins'
    }
}
dockerfile

agent {
    // Equivalent to "docker build -f Dockerfile.build --build-arg version=1.0.2 ./build/
    dockerfile {
        filename 'Dockerfile.build'
        dir 'build'
        label 'my-defined-label'
        additionalBuildArgs  '--build-arg version=1.0.2'
        args '-v /tmp:/tmp'
    }
}
dockerfile

agent {
    dockerfile {
        filename 'Dockerfile.build'
        dir 'build'
        label 'my-defined-label'
        registryUrl 'https://myregistry.com/'
        registryCredentialsId 'myPredefinedCredentialsInJenkins'
    }
}
kubernetes

agent {
    kubernetes {
        defaultContainer 'kaniko'
        yaml '''
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - sleep
    args:
    - 99d
    volumeMounts:
      - name: aws-secret
        mountPath: /root/.aws/
      - name: docker-registry-config
        mountPath: /kaniko/.docker
  volumes:
    - name: aws-secret
      secret:
        secretName: aws-secret
    - name: docker-registry-config
      configMap:
        name: docker-registry-config
'''
   }
   
url ->
https://www.jenkins.io/doc/book/pipeline/syntax/
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
