# udacity-linux-server-config
Linux server configuration project


# Changes
- full system upgrade
- added grader user
- sudo access given to grader user
- grader ssh key created
- ssh pub key added to the server
- root login and password login disabled
- added catalog user
- changed ssh port to 2200
- upgraded lightsail firewall config
  - allow tcp connections on port 2200
  - disallow connections on port 22
  - allow UDP connections on port 123 (NTP)
  - allow tcp connections on port 80
  NOTE: aside from configuring the ufw firewall in the vm I needed to do that
        in lightsail's web interface too. It seems using the ufw firewall in the
        VM is not necessary but I did it anyways as this was part of the project.


- cloned catalog project
- installed make zip unzip postgresql python3 python3-pip
- configured catalog database

- installed libapache2-mod-wsgi apache2
- created wsgi script for the catalog app
- configured apache2 to load the catalog app at the site root
- changed db connection url to provide username and password

- installed ntpd
- configured ufw firewall

NOTE: login with google is not working because it doesn't accept ip addresses
      as redirect urls and I don't have a domain name to use.
      facebook is not working because my account is not active anymore, although
      I'd guess the same domain name problem (or lack of it) would occur with FB.

# references

https://www.linux.com/learn/linux-101-updating-your-system
https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
https://askubuntu.com/questions/335961/create-default-home-directory-for-existing-user-in-terminal
http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/
http://modwsgi.readthedocs.io/en/develop/user-guides/quick-configuration-guide.html
https://lists.debian.org/debian-user/2008/10/msg02027.html
http://leonwang.me/post/deploy-flask
http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html

# command log

apt-get -qqy update
apt-get upgrade -y
useradd grader
passwd grader (12qwaszx)

nano /etc/sudoers
# added grader ALL=(ALL:ALL) ALL

ssh-keygen (for grader)

# local machine
ssh-copy-id -i ~/.ssh/udacity-grader.pub grader@54.91.73.130

nano /etc/ssh/sshd_config (disabled root login and password login)

useradd catalog
passwd catalog (master)

nano /etc/ssh/sshd_config (changed port to 2200)

sudo su
mkhomedir_helper catalog

su catalog
git clone https://github.com/tiagoengel/udacity-item-catalog
cd fullstack-webdev-catalog


apt-get -qqy install make zip unzip postgresql
apt-get -qqy install python3 python3-pip
pip3 install --upgrade pip
pip3 install flask packaging oauth2client passlib flask-httpauth flask-wtf
pip3 install sqlalchemy flask-sqlalchemy psycopg2 bleach
su postgres -c 'createuser -dRS catalog'
su postgres -c "psql -c \"ALTER USER catalog WITH PASSWORD 'catalog'\""
su catalog -c 'createdb catalog'
su catalog -c 'createdb catalog_test'
su catalog -c 'psql -d catalog -f catalog.sql'
su catalog -c 'psql -d catalog_test -f catalog.sql'
su catalog -c 'python3 seed.py'

apt-get install libapache2-mod-wsgi-py3 apache2

mkdir /var/www/wsgi-scripts
touch /var/www/wsgi-scripts/catalog.wsgi
nano /var/www/wsgi-scripts/catalog.wsgi

nano /etc/apache2/sites-enabled/000-default.conf
ps -aux | grep apache2 (find apache group)

chown -R www-data:www-data udacity-item-catalog/

cd /var/www/html
ln -s /home/catalog/udacity-item-catalog/catalog/static/

sudo apt-get install ntpstat
sudo apt-get install ntpq

ufw default allow outgoing
ufw default deny incoming
ufw allow 2200/tcp
ufw allow ntp
ufw allow http
ufw enable

cat /etc/timezone (was configured to UTC already)















