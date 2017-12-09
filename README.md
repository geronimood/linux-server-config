# Linux Server Configuration

Descripton of the server credentials and my steps to complete the project.

## Server credentials

* IP adress: 18.194.38.252
* SSH port: 2200
* SSH Key: provided in the "Notes to Reviewer" field
* SSH Passphrase: provided in the "Notes to Reviewer" field

## Web Application

* URL: http://ec2-18-194-38-252.eu-central-1.compute.amazonaws.com/


## Installed Software, Third Party Resources & Configuration Changes

* Installed Software & Third Party Resources:
  * Finger
  * Apache2
  * mod_wsgi
  * pip
  * virtualenv
  * Postgresql and psycopg2 driver
  * Flask & SQLAlchemy
  * oauth2client
  * requests

* Update system packages:
  * Run "sudo apt-get update"
  * Run "sudo apt-get upgrade"

* Configuration Changes:
  * Create new user "grader" (adduser grader) with password
  * Add user "grader" to the sudo groupt (usermod -aG sudo grader)
  * Disable remote login of root:
    * sudo nano /etc/ssh/sshd_config
    * -> set "PermitRootLogin no"
  * Create new SSH Keypair for user grader and set it in .ssh/authorized_keys:
    * ssh-keygen on local machine (chose file "AmazonLighthouse")
    * cat AmazonLighthouse.pub and copy the public key
    * login as user "grader" on Server
    * create new directory ".ssh" (mkdir .ssh)
    * create new file "authorized keys" in directory ".ssh" (touch /.ssh/authorized_keys)
    * edit "authorized_keys" and paste in public key (nano .ssh/authorized_keys)
    * chmod 700 .ssh
    * chmod 644 .ssh/authorized_keys
  * Enforce key-based authentication:
    * sudo nano /etc/ssh/sshd_config
    * -> set "PasswordAuthentication no"
  * Set SHH Port (from 22 to 2200):
    * sudo nano /etc/ssh/sshd_config
    * -> # What ports, IPs and protocols we listen for
    * -> set "Port 2200"
    * restart SSH Server (sudo service ssh restart)
    * Add port 2200 / TCP on Amazon Lighthouse Firewall (via Online Dashboard)
  * Configure Firewall:
    * sudo ufw default deny incoming
    * sudo ufw default allow outgoing
    * sudo ufw allow 2200/TCP
    * sudo ufw allow www
    * sudo ufw allow 123/UDP
    * sudo ufw enable
  * Configure Apache2:
    * sudo nano /etc/apache2/sites-enabled/000-default.conf
    * -> Add: "WSGIScriptAlias / /var/www/html/myapp.wsgi"
    * sudo apache2ctl restart
    * Create first App (sudo nano /var/www/html/myapp.wsgi)
  * Configure Postgresql:
    * create new user "www-data" (createuser www-data -P --interactive)
    * create catalog database (createdb catalog)
    * Install psycopg2:
      * find the python executable (which python -> /usr/bin/python)
      * create a new virtualenv (virtualenv --python=/usr/bin/python venvs/postgrestest)
      * activate virtualenv (source ~/venvs/postgrestest/bin/activate)
      * install psycopg2 Python package (pip install psycopg2)
    * Do Not Allow Remote Connections:
      * /etc/postgresql/9.5/main/pg_hba.conf seems to be safe


###### Author

* **Christoph Zeller** - *Initial work* -
