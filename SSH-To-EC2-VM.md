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
e.g /Users/user/.ssh/fxy4560654/Slave-Jenkins-for-Nodejs-ssh-key.pem

```

## use SUDO

```
sudo ssh -i /Users/user/.ssh/fxy4560654/Slave-Jenkins-for-Nodejs-ssh-key.pem fxy4560654@fxy4560654jenkinsslonment.australiaeast.cloudapp.azure.com

```
