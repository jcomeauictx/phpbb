For installing from this repository clone into a Debian Buster system as
of August 10, 2019. These instructions assume use of Postgresql and Apache2.
NOTE that these are not for a shared system, but rather for a VPS owned and
operated solely by the bulletin board operator. It's not using a password
for the database connection, and there are other potential vulnerabilities,
such as those related to `chown`ing the files to the Apache user `www-data`.

* sudo apt-get install php7.3 php7.3-json php7.3-xml php7.3-pgsql php7.3-gd php7.3-zlib php7.3-cli php7.3-curl php7.3-mbstring postgresql-all
* cd phpBB
* mkdir vendor
* php ../composer.phar install  # NOTE: all this does is install packages under the vendor directory
* cd ..
* sudo cp -a phpBB /path/to/installation
* sudo chown -R www-data.www-data /path/to/installation  # makes directories writable for installation

# now create the database

* sudo su - postgres
* psql
* create role phpbb login;
* create database phpbb owner phpbb;
* \q

because we're using postgresql roles instead of system users, we must
make sure to configure postgresql accordingly, with "trust" method instead
of "md5"

* `sudo sed -i 's%\(^host\s\+all\s\+all\s\+127\.0\.0\.1/32\s\+\)md5$%\1trust%' /etc/postgresql/11/main/pg_hba.conf`
* sudo systemctl restart postgresql

# configure Apache

not going to deal with this here. if you're a system administrator, you've
likely done this a few hundred times already.

# now finish the installation via browser

* # NOTE: this is horribly insecure! you're in a race with millions of wannabe "hackers" who will already be probing your system to see if there's an unconfigured phpbb system to take over. work fast!

* firefox http://installation-url/phpBB/  # and follow the instructions

* for easiest installation, accept all defaults except:

    * first page, Administrator configuration, fill it all out
    * second page, Database configuration, enter:
        * DSN: 127.0.0.1
        * Database username: phpbb
        * Database name: phpbb
    * final page, Bulletin board configuration, set title and description

the email setup, if necessary, isn't discussed here. from memory, I had to
install exim4, `dpkg-reconfigure exim4-config` and change it to an "Internet
connected" system, add SPF records to the domain, and set a PTR record for
the IP number, in order for it to successfully send mail to other systems.

# when installation is complete

* sudo mv /path/to/installation/phpBB/install ~/phpbb.install  # save the install directory to your home or elsewhere
