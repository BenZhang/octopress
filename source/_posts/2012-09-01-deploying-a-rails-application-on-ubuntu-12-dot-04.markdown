---
layout: post
title: "Deploying a Rails application on Ubuntu 12.04"
date: 2012-09-01 11:25
comments: true
categories: ['Ubuntu', 'Deploy', 'Nginx']
---
First thing first, make you server up to date.
```bash
sudo apt-get update
sudo apt-get upgrade
```
Install git
```bash
apt-get -y install curl git-core python-software-properties
```
Email service
```bash
apt-get -y install telnet postfix
```
MySQL
```bash
sudo apt-get install mysql-server libmysqlclient15-dev
```
Install RVM
```bash
curl -L https://get.rvm.io | bash -s stable
source /home/your name/.rvm/rvm.sh
```
Once it is finished, you can run this command to install the required libs
```bash
rvm requirements
```
Install required libs Ruby needed.
```bash
sudo apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion
```
Then install Ruby
```bash
rvmsudo rvm install ruby
rvm use --default 1.9.3
```
Install Passenger and Nginx. Note that if you want to use passenger do not install Nginx first, cause the passenger module has to be compiled into Nginx.
```bash
gem install passenger --no-ri --no-rdoc
rvmsudo passenger-install-ngnix-module
```
When you finish that. If you use default setting, go to /opt/nginx/nginx.conf, and add the Ruby and your project config according to the instruction.
Install and add nginx to system service
```bash
cd /etc/init.d
sudo wget https://raw.github.com/chloerei/nginx-init-ubuntu-passenger/master/nginx
sudo update-rc.d nginx defaults
sudo chmod +x nginx
sudo service nginx start
```
