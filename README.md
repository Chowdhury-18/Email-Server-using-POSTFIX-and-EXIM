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
In Lab1 go to /etc/postfix/main.cf
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

## Configure EXIM4
In Lab2 configure EXIM4 to handle local emails. Use the standard debian package configuration tools to configure EXIM4 
```
$sudo dpkg-reconfigure exim4-config
```
and set the parameters as follows. Use Lab1 as smarthost.
```
General type of mail configuration:
  mail sent by smarthost; received via SMTP or fetchmail
System mail name:
  lab2.com
IP-addresses to listen on for incoming SMTP connections:
  127.0.0.1
Other destinations for which mail is accepted:
  lab2.com; lab2; localhost.localdomain; localhost
IP address or host name of the outgoing smarthost:
  lab1
Hide local mail name in outgoing mail?
  No
Keep number of DNS-queries minimal (Dial-on-Demand)?
  No
Delivery method for local mail:
  mbox format in /var/mail/
Split configuration into small files?
  No
Keep rest of the configuration empty
```
## Configure PROCMAIL and SPAMASSASSIN
Configure PROCMAIL and SPAMASSASSIN in Lab1.

Create procmailrc (global) file.
```
$sudo touch /etc/procmailrc
```
Edit procmailrc file and add the following lines.
```
:0fw
| /usr/bin/spamassassin


MAILDIR=$HOME/Mail
LOGFILE=$MAILDIR/procmaillog
LOGABSTRACT=all
VERBOSE=yes


:0:
* ^X-Spam-Flag: YES
SPAM
```

