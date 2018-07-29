# flow-manager-microservice
Flow Manager Microservice (FMM) is example microservice written using Symfony Framework, Vagrant and VirtualBox. Mainly for learning purposes.

**The big picture**

This service is mainly created for learning purposes. I have had some experience with Symfony2 but none with Symfony 3 and 4. 

Imagine that you have multi-step e-commerce form that is splited into separate pages done in React or similar frontend framework. And these pages are able to talk to backend microservices to store and retrieve their data, etc. 

Now, let's say that business needs to A/B test the form, but without step 3, and then they need to check if it is better to switch step 2 and 3 with each other. See the point?

Usually, such requests would eventually lead you to another monolith application, full of config switchers, extensions, etc. My approach here is to have another microservice that would be able (when properly asked) tell what and where is your next (and prev) step. 

In other words, when user clicks "Next Step" link, then it will call that step related microservie. Next, this service will ask FMM for next steo that user should be redirected to and it will redirect to it.

That way we have flow separated from each step or page of the form. We just need to tell the first page for which flow we want to use it and pass that info further.

**What I want to learn**

- S4 (Symfony4) architecture, esp. configuration
- making S4 microservice up'n ready, creating own boilerplate
- Design Patterns and PHP7 features
- How to work in MacOS Sierra environment

**MVP Definition**
- MUST-HAVE: configuration architecture allowing to manage separate projects, with multiple flows (request params)
- MUST-HAVE: endpoint for responding with next/prev step link through POST protocol
- MUST-HAVE: REST architecture
- COULD-HAVE: JWT authentication

**Resources**

- Udemy courses: 

1. [Create the REST API in PHP Symfony Complete Class](https://www.udemy.com/creating-rest-api-in-symfony/learn/v4/content), 

2. [Learn PHP Symfony 4 Hands-On Creating Real World App](https://www.udemy.com/learn-symfony-4-hands-on-creating-a-real-world-application/learn/v4/content), 

**Learning Plan (each step for less than hour)**

- follow relevant materials from sections 1-4 from Udemy course [1]
- create microservice quickstart boilerplate and push it as separate repository
- add routing, handler, model for POST request
- find best way on how to describe flows of the client application in yaml config file
- add config file with a solution for reading from it

**Setting up project**

- Setting up Vagrant box
```
vagrant box add laravel/homestead

git clone https://github.com/laravel/homestead.git ./homestead

cd ./homestead

bash init.sh

ssh-keygen -t rsa -C "vagrant@vagrant"
```

this plugin is for file sharing with vagrant box:
```
vagrant plugin install vagrant-bindfs
```
- Configuring the box:

```
nano Homestead.yaml
```
update the following lines:
```yaml
folders:
- map: ~/Sites/flow-manager-microservice/code
to: /home/vagrant/code
type: "nfs"

sites:
- map: fmm.local
to: /home/vagrant/code/web

databases:
- fmm
```
- Setting up host

copy the ip address with domain name from the top of the Homestead.yaml file and:

```
sudo nano /etc/hosts
```

add line at the end:

```
192.168.10.10 fmm.local
```
save and close the editor

- Install Symfony 3.3

```
vagrant up

vagrant ssh

vagrant@homestead:~$ composer create-project symfony/framework-standard-edition code "3.3.*"
```
- modifying app_dev.php:

open the url in browser:
```http://fmm.local/app_dev.php```

you will get the message 

```You are not allowed to access this file. Check app_dev.php for more information.```

open web/app_dev.php and comment the following lines:

```php
if (isset($_SERVER['HTTP_CLIENT_IP'])
    || isset($_SERVER['HTTP_X_FORWARDED_FOR'])
    || !(in_array(@$_SERVER['REMOTE_ADDR'], ['127.0.0.1', '::1'], true) || PHP_SAPI === 'cli-server')
) {
    header('HTTP/1.0 403 Forbidden');
    exit('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
}
```
- connecting to MySQL database, available in vagrant box:

```yaml
host: 127.0.0.1
user: homestead
password: secret
port: 33060 (port forwarding)
```
you should see empty fmm database.

- setting up privileges for root url 

if you go to ```http://fmm.local``` then you'll get 403 Forbidden response. To solve this open Homestead.yaml file and under section "sites" add this line:

```yaml
      type: symfony
```
then save, exit and:
```yaml
vagrant reload --provision
```
this command needs to be done whenever you make changes to the machine configuration.

- Installing required packages

```yaml
vagrant@homestead:~/code$ composer require firendsofsymfony/rest-bundle
vagrant@homestead:~/code$ composer require jms/serializer-bundle
```
