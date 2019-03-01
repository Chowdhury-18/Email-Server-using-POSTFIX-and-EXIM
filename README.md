# Email-Server-using-POSTFIX-and-EXIM
We will learn how to setup an email server with filtering rules and spam detection using POSTFIX and EXIM.

# Environment setup
We will use two debian (Ubuntu) machines for our experiment. We named the machines Lab1 and Lab2 respectively. We set the IPv4 addresses using dhcp of Lab1 as 192.168.1.11 and Lab2 as 192.168.1.12.

Now, We will set the IP addresses in the hosts file of both the machines.
```
$sudo nano /etc/hosts
```
Add the following line in Lab1
```
192.168.1.12 lab2
```
and in Lab2
```
192.168.1.11 lab1
```
Install the following software in Lab1
```
$sudo apt-get update
$sudo apt-get install postfix
$sudo apt-get install procmail
$sudo apt-get install spamassassin
```
and in Lab2
```
$sudo apt install exim4
```

## Configure POSTFIX
Go to /etc/postfix/main.cf
```
$sudo nano /etc/postfix/main.cf
```
and edit the following variables
```
myhostname = lab1
mydomain = lab1.com
mydestination = $myhostname, $mydomain, localhost.localdomain, localhost
```
After changing the configuration file, restart the service
```
sudo service postfix restart
```
