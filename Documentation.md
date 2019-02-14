# Fail2Ban Documentation


### Case Story
Recently I setup a VPS to keep the logs of different server for analysis. I have strong evidence to believe that my server is under brute-force attack. I did harden the SSH and firewall (I will share it with different Repository Name), nevertheless, I would like to block the culprit in an automated way and fail2ban came to handy. Though to keep a documentation about it to share with friends and for future reference.


## Caution: 
    Fail2ban is intended to be used in conjection with an already hardened server and should not be used as a replacement for secure firewall rules.

### Target System uBuntu16.04 server 

### Installing Fail2Ban

    - sudo apt-get update 
    - sudo apt-get install fail2ban -y 

    - sudo service fail2ban restart 
    
    To check whether the service is running

    - sudo service fail2ban status 

    You will see <b>active running </b>. 

### Configuring Fail2ban 

    We have to create a file on which we can write custom rules for what services it protects, and how to handle violations. 

    vi /etc/fail2ban/jail.local 

***
    paste the following code 

    [DEFAULT] ignoreip = 127.0.0.1/8 ::1 
    bantime = 3600
    findtime = 600
    maxretry = 5
***

### Explaination 

* We are telling Fail2bn to ignore 127.0.0.1 and ::1  (loop back address of ip4 and ip6)
* Fail2ban will ban IP address for 1 hour (<b>bantime = 3600</b>), if they make 3 mistakes (<b>maxretry = 5</b>), within 10 minutes (<b>findtime = 600</b>). 
* We enabled the jail for sshd. 

### Send mail option 
- sudo apt-get install sendmail-bin sendmail 

### Configuration Code 

    destemail = myemail@gmail.com
    sendername = Fail2Ban
    mta = sendmail

### whitelist particular IP 

* fail2ban-client set JAIL addignoreip 192.168.56.1

Note: 
    
* Setting a ban time of -1 will result in a permanent ban. 
* If Fail2ban doesn't start successfully after creating your configuration file, check the typo in <b>/etc/fail2ban/jail.local</b>

### Fail2ban Usage 

- Run the following command to check the status of Fail2ban: 

    fail2ban-client status 

    fail2ban-client status sshd 


#### How to remove banned IP 

- fail2ban-client set (JAIL NAME) unbanip (IP ADDRESS)

eg. 
- fail2ban-client set sshd unbanip 192.168.56.1


#### Note

    Jails can also be configured as individual .conf files placed in the jail.d directory. The format will remain the same.



### Reference
- [Intersting](https://www.linode.com/docs/security/using-fail2ban-for-security/)
- [Short and concise](https://www.liquidweb.com/kb/install-configure-fail2ban-ubuntu-server-16-04/)
