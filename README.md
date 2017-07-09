# udacity-linux-server-config
Linux server configuration project

**SSH: grader@34.226.38.251 -p 2200 -i [private key]**

**WEB APP HOSTED AT: http://34.226.38.251/**

A detailed list of the actual commands I've used to configure to server can be seen [here](COMMANDS)


# Softwares installed

- make
- zip
- unzip
- postgresql
- python3
- python3-pip
- libapache2-mod-wsgi-py3
- apache2
- ntpstat
- ntpq


# Changes

- full system upgrade
- grader user added
- sudo access given to grader user
- grader ssh key created
- ssh pub key copied to the server
- remote root login and remote password login disabled
- catalog user added
- ssh port changed to 2200
- lightsail firewall config updated to:
  - allow tcp connections on port 2200
  - disallow connections on port 22
  - allow UDP connections on port 123 (NTP)
  - allow tcp connections on port 80

  *NOTE: aside from configuring the ufw firewall in the vm I needed to do it
        in lightsail's web interface too. It seems using the ufw firewall in the
        VM is not necessary but I did it anyways as this was part of the project.*


- catalog project cloned to `/home/catalog/udacity-item-catalog`
- catalog user added to postgres with password
- changed catalog's connection url to provide username and password
- catalog database created and configured
- wsgi script for the catalog app created at `/var/www/wsgi-scripts/catalog.wsgi`
- apache2 default config changed to load the catalog app at `/`
  - see /etc/apache2/sites-enabled/000-default.conf
- ufw firewall configured


*NOTE: login with google is not working because it doesn't accept ip addresses
      as redirect urls and I don't have a domain name to use.
      facebook is not working because my account is not active anymore, although
      I'd guess the same domain name problem (or lack of it) would occur with FB.*



# references

https://www.linux.com/learn/linux-101-updating-your-system

https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

https://askubuntu.com/questions/335961/create-default-home-directory-for-existing-user-in-terminal

http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/

http://modwsgi.readthedocs.io/en/develop/user-guides/quick-configuration-guide.html

https://lists.debian.org/debian-user/2008/10/msg02027.html

http://leonwang.me/post/deploy-flask

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html















