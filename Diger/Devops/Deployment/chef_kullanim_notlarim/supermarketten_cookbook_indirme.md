```bash
yum install -y git
cd /PATH/chef-repo/cookbooks
git init
echo "New repo" > readme.txt
git add readme.txt
git commit -am "initializing repo for chef"
knife cookbook site install <Cookbook_adi> <versiyon>
```
