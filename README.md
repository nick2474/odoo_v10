# odoo_v10
Odoo 10 on Ubuntu 16.04

## Prerequisites
Before starting, update your system:

`sudo apt-get update`

`sudo apt-get dist-upgrade`

Install Git in order to clone Odoo source code from Github

`sudo apt-get install git`

Next, install the necessary dependencies for Odoo:

`sudo apt-get install python-dateutil python-docutils python-feedparser python-gdata python-jinja2 python-ldap python-libxslt1 python-lxml python-mako python-mock python-openid python-psycopg2 python-psutil python-pybabel python-pychart python-pydot python-pyparsing python-reportlab python-simplejson python-tz python-unittest2 python-vatnumber python-vobject python-webdav python-werkzeug python-xlwt python-yaml python-zsi python-pypdf python-decorator python-requests -y`

Install the most recent version of gdata:

`sudo apt-get install python-pip -y`

`sudo pip install gdata --upgrade`

## Create a system user for Odoo application
create a new Odoo system user to run its processes

`sudo adduser --system --home=/opt/odoo --group odoo`

You can view the user and group

`sudo cat /etc/passwd`

# Install & Prepare Postgres

We install postgresql databank, this will probably be version 9.2. Make sure that you have not installed postgresql already, if so you can either uninstall it (recommended) or skip this step.

`sudo apt-get install postgresql -y`
`sudo apt-get install postgresql-9.5 -y`

After installing Postgres check the version of the Postgres using
`psql --version`

Now depending on the version you need to install postgresql-server-dev, if your version is 9.5.3 then 9.5 and if 9.4.0 then 9.4, run the below command
`sudo apt-get install postgresql-server-dev-9.5`

This will automatically create a (system) user named postgres. Now we log in as that user and create a database user.

`sudo su - postgres`

`createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo`

You will be prompted for a password, write this password down. We will now go back to root user, type 
`exit`

you can created superuser without
`createuser -s odoo`
Note: Create user parameter:-s means creating super user

## Postgresql Management

Changing password of postgresql user

After su as postgres user, run command in psql as following

`ALTER USER odoo WITH PASSWORD 'new_password';`

List databases and tables in postgresql

`psql`

`\du => List all users`
`\l => List all databases`
`\dt => List all tables in current database`
`\connect => Change current database`
`\q=> Quit psql mode`


Enable remote connect to postgresql

Edit postgresql.conf to listen the port from all interfaces
`cd /etc/postgresql/9.5/main/`
`nano postgresql.conf`

Change the line that contains listen_addresses=’localhost’ to listen_addresses=’*’
Edit pg_hba.conf to allow remote connection from other servers

`nano pg_hba.conf`

Add the following line,
`host all all 0.0.0.0/0 md5`

Note: To allow IPv6 or all IPs, use

`host all all ::0/0 md5 #ipv6 range`
`host all all all md5   #all ip`


check postgresql is running with:

`systemctl status postgresql`
If PostgreSQL is running, you'll see output that includes the text Active: active (exited).

If you see Active: inactive (dead), start the PostgreSQL service using the following command:

`systemctl start postgresql`
PostgreSQL also needs to be enabled to start on reboot. Do that with this command:

`systemctl enable postgresql`
