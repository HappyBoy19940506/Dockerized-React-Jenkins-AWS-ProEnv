# 1. work flow
  ```
  Code PR -- GitHub -- Jennkins --- when in (master/dev branch) --build/test           ----ON JENKINS ec2
                                                                ---deploy to dev       ---on  jenkinsEC2
                                    when in (production branch) ---deploy to prod       ---ON aws s3  ---> CLOUDFRONT(CDN) --->ROUTE 53 DOMAIN
  
  
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
