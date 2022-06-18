# 1. work flow
  ```
  Code PR -- GitHub -- Jennkins Pipeline ---Cloud

  IF something pushed in MASTER/DEV Branch :
        BUILD
        TEST
        DEPLOY by 'Npm START' in Jenkins NODE-JS AGENT

        --> You can see a website at xxxxxxurl:3000

  If something pushed in Prod Branch: 
       BUILD
       TEST
       DEPLOY by 'NPM RUN BUILD' ---> upload THE build Folder to a Hosting e.g. S3 / Azure Blob
                                            
  
      ----> Folder upload to s3, you can use s3 url to visit the website ---> CLOUDFRONT(CDN) --->ROUTE 53 DOMAIN
   ```
# 2.WHAT I NEED
  ## 1 JENKINS ON EC2.
      ```
      create a ec2 
      open port 8080 /3000
      ssh to ec2
      install java
      install jenkins
      ----
      install docker
      
      ````
  ## 2 play JENKINS
     ```
     go in to jenkins
     blue ocean --> link github
     
     jenkinsfile - test build test
      
      ````

  ## 3 code dockerize
      ```
      write dockerfile for prod and dev
      
      local test
      
      ````
## 4 deploy
    ```
    create a s3 
    
    secure s3 with jenkins pipeline
    
    dev - docker run on jenkins ec2. port out 3000, can visit
    
    prod - docker run , cp build folder aws s3 
    
    ```
