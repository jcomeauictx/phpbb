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
* cp -a phpBB /path/to/installation
* chown -R www-data.www-data /path/to/installation  # makes directories writable for installation
* rm -rf phpBB/vendor  # doesn't belong in repo

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

# now finish the installation

* firefox http://installation-url/phpBB/  # and follow the instructions

accept all defaults except possibly for the last page, the bulletin board
name and description.

the email setup, if necessary, isn't discussed here. from memory, I had to
install exim4, `dpkg-reconfigure exim4-config` and change it to an "Internet
connected" system, add SPF records to the domain, and set a PTR record for
the IP number, in order for it to successfully send mail to other systems.
