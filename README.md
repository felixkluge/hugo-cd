# CD pipeline deploying static website with Hugo

- Create Cloud9 instance

- Install Hugo: https://gohugo.io/installation/linux/ (using prebuilt binaries)
    - `wget https://github.com/gohugoio/hugo/releases/download/v0.129.0/hugo_0.129.0_linux-amd64.tar.gz`
    - `tar zxvf hugo_0.129.0_linux-amd64.tar.gz`
    - `mkdir -p ~/bin`
    - `mv hugo ~/bin/`
    - `hugo version`

Create new site and flatten directory structure
    - `hugo new site quickstart`
    - `mv quickstart/* .`
    - `rm -r quickstart/`

Add theme
    - `git submodule add git@github.com:theNewDynamic/gohugo-theme-ananke.git themes/ananke`
    - `echo 'theme = "ananke"' >> hugo.toml`
    
Update .gitignore
    - Add `public`