Ortam:
`
Debian sid amd64
3.2.0-4-amd64
ruby 2.1.4p265 
`
kurulan paketler: 
```
sudo apt-get install rubygems ruby2.1-dev libicu-dev
sudo gem install bundler
bundle config build.nokogiri --use-system-libraries
bundle install
```
### Kaynaklar
https://github.com/maciakl/Sample-Jekyll-Site/blob/master/_config.yml
https://github.com/kui/k-ui-octopress-theme/blob/master/_config.yml.example
