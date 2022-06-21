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
## Resolved: 
```
npm run build生成build文件夹后，
1. 在本地 index.html空白问题。 在打包之前,在package.json中private下(位置任意)添加"homepage": "./" ，再重新build一下。
{
  "name": "bookinglet",
  "homepage": "./", <----------------------
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "bootstrap": "^5.2.0-beta1",
    "react": "^18.1.0",
    "react-dom": "^18.1.0",
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
  },

2. S3上 hosting的 index.html显示空白 -->
*不能上传build文件夹，而要把里面的 所有文件上传。你想啊，一个用户输入网址，www.xxx.com 不会输入www.xxx.com/homepage 这样，所以你一旦进入www.xx.com就相当于来到了root directory了，你得有个东西招待他呀，那root里面肯定要有个index.html啊，不然当然是光的了。 S3 static hosting里面问你填index.html问的是你主页name叫什么，而不是主页的path是什么。


*(???存疑）如果上传的是文件夹，那么s3给你的url是空白页，请去cloudfront中设置origin default path,把它设置成 ./build/index.html。 并且origindomianName选择s3的website entrypoint而不是 s3本身的地址。



https://www.freecodecamp.org/news/how-to-host-and-deploy-a-static-website-or-jamstack-app-to-s3-and-cloudfront/#storing-your-website-on-s3

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



# 6. Jenkins pipeline how to change to another folder

```
Jenkins pipeline how to change to another folder
```
## Resolved: 
```steps {
    sh "pwd"
    dir('./your-sub-directory/dir') {
      sh "pwd"
    }
    sh "pwd"
} 
<!-- dir后面的路径是按源根目录的root来算的路径，而不是 jenkinsifle所在的路径为根目录 -->
<!-- 本来前端后端就不应该放在一个repo里面 -->
```


# 7. Jenkins build fails with "Treating warnings as errors because of process.env.CI = true"
```
[ERROR] {some text}: {some text} is outdated. Please run next command `npm update`
[INFO] Treating warnings as errors because process.env.CI = true.
[INFO] Most CI servers set it automatically.
Failed to compile.
```
## Resolved: 
```
现在大部分项目都会支持ci工具来做持续集成，所以现在默认值为ci=true，这样让大部分的library会按照ci=true的环境来运行.而不是local环境。
但是有些library会因此报错，而且会halt build导致fail to compile。
1. jenkinsifle 中 
 environment {
        CI= 'false'
    }
2. 在 jenkins ui中，设置global config ->
-You can set CI env variable to false through Manage Jenkins > Configure System > Global properties section.
-Add a new env variable CI with the value false.
3. 在package.json中更改，改成默认为 false:
  "scripts": {
    "start": "react-scripts start",
    "build": "CI=false && react-scripts build",  // Add CI=False here
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },

```
# 8. fatal: couldn't find remote ref refs/heads/master

```
Started by user root
hudson.plugins.git.GitException: Command "git fetch --tags --force --progress --prune -- origin +refs/heads/master:refs/remotes/origin/master" returned status code 128:
stdout: 
stderr: fatal: couldn't find remote ref refs/heads/master

	at org.jenkinsci.plugins.gitclient.CliGitAPIImpl.launchCommandIn(CliGitAPIImpl.java:2671)
	at org.jenkinsci.plugins.gitclient.CliGitAPIImpl.launchCommandWithCredentials(CliGitAPIImpl.java:2096)
	at org.jenkinsci.plugins.gitclient.CliGitAPIImpl.access$500(CliGitAPIImpl.java:84)
	at org.jenkinsci.plugins.gitclient.CliGitAPIImpl$1.execute(CliGitAPIImpl.java:618)
	at jenkins.plugins.git.GitSCMFileSystem$BuilderImpl.build(GitSCMFileSystem.java:366)
	at jenkins.scm.api.SCMFileSystem.of(SCMFileSystem.java:197)
	at jenkins.scm.api.SCMFileSystem.of(SCMFileSystem.java:173)
	at org.jenkinsci.plugins.workflow.cps.CpsScmFlowDefinition.create(CpsScmFlowDefinition.java:118)
	at org.jenkinsci.plugins.workflow.cps.CpsScmFlowDefinition.create(CpsScmFlowDefinition.java:70)
	at org.jenkinsci.plugins.workflow.job.WorkflowRun.run(WorkflowRun.java:311)
	at hudson.model.ResourceController.execute(ResourceController.java:101)
	at hudson.model.Executor.run(Executor.java:442)
Finished: FAILURE
```
## Resolved: 
```
Master 现在叫main 了
```



# 9. Why there is no Source Code Management tab in a Jenkins pipeline job?
## Resolved: 
```
FreeStyle里面有soure code management来选定code来自哪个git-repo
Pipeline里面其实也有，但是不叫Source Code Management,直接在选定jenkinsfile script form SCM下啦菜单的Repository URL就已经是该repo地址，因为他
默认 jenkinsfile go with this repo together.

-Script Path :指定 jenkisnfile在哪，有可能不在root，那你就要写 relative-path ,e.g.  ./cicd/jenkinsfile 

-Branches to build : List of branches to build. Jenkins jobs are most effective when each job builds only a single branch. 不推荐 a single job builds multiple branches. 一个pipeline也就是一个job，一般只允许一个branch，规避multiple branch。


git code ==等价于 
在input script中写:
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'
            }
	   }
```

----------------------------------------------------

# 3. 

```
Grs/jso
```
## Resolved: 
```
```
----------------------------------------------------

# 3. 

```
Grs/jso
```
## Resolved: 
```
```
----------------------------------------------------

# 3. 

```
Grs/jso
```
## Resolved: 
```
```
----------------------------------------------------

# 3. 

```
Grs/jso
```
## Resolved: 
```
```
----------------------------------------------------

# 3. 

```
Grs/jso
```
## Resolved: 
```
```
----------------------------------------------------

# 3. 

```
Grs/jso
```
## Resolved: 
```
```
----------------------------------------------------
