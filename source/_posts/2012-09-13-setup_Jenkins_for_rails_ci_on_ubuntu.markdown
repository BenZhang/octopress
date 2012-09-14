---
layout: post
title: "Setup Jenkins for Rails CI on Ubuntu"
date: 2012-09-13 9:50
comments: false
external-url:
categories: ['CI', 'Jenkins', 'Rails', 'Testing']
---
Continuous integration aims to improve the quality of our softwares. The theory behind the theme is quite simple, to make sure the software will work, we have to make every build tested.
From the technical point of view, it is basically a global hook that will be triggered when a push was made to our repository. As for static languages, such as Java, C, C++, languages needs to be compiled, CI comes with more steps cause we have to build the source code before we run the tests. The term continuous integration is pretty funcy when we are using static languages. In addition, We need to compile and build the source code and run the test across the whole project. That can be extremely complicated for a big project. However, as for the dynamic languages like Ruby, Python, CI can be very trivial. Since Rails comes with a great testing framework, there is not much we need to do for the CI but trigger the test command. This is an article provides some guidelines to setup a CI server using Jenkins on Ubuntu.
## Install Jenkins server
First of all, run the following commands to install Jenkins.
```bash
wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo aptitude update
sudo aptitude install jenkins
```
Once the installing is finished, your server is up. Go to http://localhost:8080. You should be able to see the welcome page.

## Jenkins Plugins
Jenkins comes with a lot of plugins. Go to Jenkins > Manage Jenkins > Manage Plugins. For our case, we need to have Github Plugin installed. 
Once it is done. You can create a new task. Put your ssh repository url into GitHub project and Source Code Management. You can make the task runs automatically whenever a push is made by choosing Build when a change is pushed to GitHub. You need to go to your github and set the Service Hooks / Post-Receive URLs to your url of CI server, http://your-url/github-webhook.

## Configure the build
One thing we need to explain is that the jenkins service is running under the user jenkins in your system. Therefore in order to run your tests, you need to have the Rails environment setup for user jenkins. First of all, generate ssh key for jenkins user
```ruby
# login as jenkins
sudo su - jenkins
# generate ssh key
ssh-keygen -t rsa
```
If you are using RVM in your server you will have to do the configuration for jenkins user or you have the RVM enabled for all the users in your server. Remember to set the jenkins user to trust .rvmrc file by going to /var/lib/jenkins/.rvmrc, and append this line

```bash
export rvm_trust_rvmrcs_flag=1
```
And create file /var/lib/jenkins/.bashrc with the following content
```bash
[ -s "/var/lib/jenkins/.rvm/scripts/rvm" ] && source "/var/lib/jenkins/.rvm/scripts/rvm" 
```
You are almost done here. The last step is create a Jenkins task fill up your repository url and go to Jenkins build session select Execute shell, and add the following command.
```bash
#!/bin/bash -x
source ~/.bashrc
cd .
bundle install
mv database.yml.example database.yml
bundle exec rake db:test:load RAILS_ENV=test
bundle exec rake RAILS_ENV=test
```
Now your Rails CI server should be up and running.

## Security problem
Jenkins does not come with the security default setting. I guess no one would like to share their CI server with the world. To enable the security is fairly easy. Go to Manage Jenkins > Configure System > Access Control. You can select the suitable solution for your own need.