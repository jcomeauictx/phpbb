For installing from this repository clone into a Debian Buster system as
of August 10, 2019. These instructions assume use of Postgresql and Apache2.

* sudo apt-get install php7.3 php7.3-json php7.3-xml php7.3-pgsql php7.3-gd php7.3-zlib php7.3-cli php7.3-curl php7.3-mbstring postgresql-all
* cd phpBB
* mkdir vendor
* php ../composer.phar install  # NOTE: all this does is install packages under the vendor directory
* cd ..
* cp -a phpBB /path/to/installation
* chown -R www-data.www-data /path/to/installation  # makes directories writable for installation
* rm -rf phpBB/vendor  # doesn't belong in repo
* firefox http://installation-url/phpBB/  # and follow the instructions
