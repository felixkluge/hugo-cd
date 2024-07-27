# CD pipeline deploying static website with Hugo

- Create Cloud9 instance

- Install Hugo: https://gohugo.io/installation/linux/ (using prebuilt binaries)
    - `wget  https://github.com/gohugoio/hugo/releases/download/v0.129.0/hugo_extended_0.129.0_Linux-64bit.tar.gz` (make sure you get the extended version for compatibility with the theme)
    - `tar zxvf hugo_0.129.0_linux-amd64.tar.gz`
    - `mkdir -p ~/bin`
    - `mv hugo ~/bin/` or `sudo  mv ~/bin/hugo /usr/local/bin/`
    - `hugo version`

- Create new site and flatten directory structure
    - `hugo new site quickstart`
    - `mv quickstart/* .`
    - `rm -r quickstart/`

- Add theme
    - `git submodule add git@github.com:theNewDynamic/gohugo-theme-ananke.git themes/ananke`
    - `echo 'theme = "ananke"' >> hugo.toml`
    
- Update .gitignore
    - Add `public`
    
- New post
    - `hugo new posts/first_post.md`
    - in content/posts/first_post.md, set `draft: false`
    
- Two ways to look at Hugo site:
    - Serve it on a EC2 instance
        - Open port 8080
        - AWS Management console -> EC2 -> Running instance -> select correct instance
        - Security -> Add inbound rules, port 8080

        - `curl ipinfo.io` -> get ip adress (can also be obtained from AWS console -> EC2 instance properties -> Public adress)
        - `hugo serve --bind=0.0.0.0 --port=8080 --baseURL=http://3.250.156.223`
        - Cons: What if EC2 instance goes down? -> Serverless would be the better option (Can just be copied over)
    - Look at directory offline
        - `hugo` -> creates public directory
        - Download public directory
    
    
- Move it to an S3 bucket to serve it out
    - Create a new bucket, e.g. hugocdwebsite
    - Enable *Static website hosting*
    - Disable "Block public access"
    - Add Bucket Policy 
        ```
            {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Sid": "PublicReadGetObject",
                        "Effect": "Allow",
                        "Principal": "*",
                        "Action": "s3:GetObject",
                        "Resource": "arn:aws:s3:::hugocdwebsite/*"
                    }
                ]
            }
        ```
    - Manual way:
        - Download public folder 
        - Upload it to the S3 Bucket
        - Open the link given in the "Static website hosting" section http://hugocdwebsite.s3-website-eu-west-1.amazonaws.com/
    
