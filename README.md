# Certbot/Let's Encrypt Renewal Script for IPA Servers

## Intro
This is a pretty basic script. Just needed a way to renew LE certs automagically and properly handle the renew hook, the main hurdle being the input required from IPA to update a cert. It's basic and has NO ERROR HANDLING. This is just an initial attempt. Please do not use this in production.

### Environment/Requirements
- I created this script using CentOS 8 and Certbot using these instructions: https://certbot.eff.org/lets-encrypt/centosrhel8-apache
- FreeIPA server (Other variations of IPA servers may work, I don't know)
- Let's Encrypt already initially set up for your IPA server
- Additional Packages Required: wget, unzip (or git)
- Personally, I added a cockpit renewal hook as well. Comment that part out if you don't want it.

### Instructions
#### I'm going to assume your environment is setup with Certbot properly installed

1. Download and extract the files
    ```
    $ wget https://github.com/tabp0le/certbot_ipa/archive/master.zip
    $ unzip master.zip
    ```
2. Create hidden file with your IPA directory administrator password and set permissions
    ```
    $ sudo /bin/sh -c 'echo YOURPASSWORD > /root/.directorymanager'
    $ sudo chmod 0600 /root/.directorymanager
    ```
3. Copy the files to the proper location
    ```
    $ cd certbot_ipa-master
    $ sudo cp le-* /usr/local/bin/
    $ sudo chown root:root /usr/local/bin/le-*
    $ sudo chmod 0750 /usr/local/bin/le-*
    ```
4. Set up your cron job
    ```
    $ echo "0 0,12 * * * root python3 -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/local/bin/le-renew" | sudo tee -a /etc/crontab > /dev/null
    ```
