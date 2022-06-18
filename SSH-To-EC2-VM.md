## Create a VM

## Get its Public IP

## Create a SSH public key + private Key

## Public Key gives your cloud 

## Privte Key save to your ./ssh/some-user-name/xxxxx.pem

## Ensure you have read-only access to the private key. Chmod is only supported on Linux subsystems (e.g. WSL on Windows or Terminal on Mac).
 
 ```
 chmod 400 <keyname>.pem
 
 ```
 
## Provide a path to your SSH private key file. 

```
e.g /Users/user/.ssh/fxy4560654/ssh-key.pem

```

## use SUDO

```
sudo ssh -i /Users/user/.ssh/fxy4560654/ssh-key.pem fxy4560654@jenkinsslonment.australiaeast.cloudapp.azure.com

```

## How to Fix "WARNING: UNPROTECTED PRIVATE KEY FILE!" on Mac and Linux
```
https://stackabuse.com/how-to-fix-warning-unprotected-private-key-file-on-mac-and-linux/
```

## How to install NVM

```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

```


## java install
```
Master node: java / jenkins 

Slave node: java / nodeJS

NOTED: JAVA SHOULD BE ALSO INSTALLED ON SLAVE NODE, 因为你仅仅ssh了两者，其实 双方还要互相发送一个jar的包 才能是Jenkins互相连接，你没java 怎么运行jar的包？？
```

