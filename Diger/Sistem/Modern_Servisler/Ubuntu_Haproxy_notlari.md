hostnamectl set-hostname $HOSTNAME
add-apt-repository ppa:vbernat/haproxy-1.5
apt-get update && apt-get install haproxy
apt-get install -y supervisor openjdk-7-jre-headless syslog-ng-core
git clone https://github.com/o/dotfiles.git
locale-gen tr_TR tr_TR.UTF-8
