# 1. work flow
  ```
  Code PR -- GitHub -- Jennkins --- when in (master/dev branch) --build/test           ----ON JENKINSec2
                                                                ---deploy to dev       ---on  jenkinsEC2
                                    when in (production branch) ---deploy to prod       ---ON aws s3  ---> CLOUDFRONT(CDN) --->ROUTE 53 DOMAIN
  
  
  ```
# 2.WHAT I NEED
  # 1
