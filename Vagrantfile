# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT

# timezone
cp -p /usr/share/zoneinfo/Japan /etc/localtime

# remove all rules
/sbin/iptables -F
# iptables stop
/sbin/service iptables stop
# iptables off
/sbin/chkconfig iptables off

### apache install ###
# install httpd
yum install -y httpd
# cp local httpd.conf
cp -a /vagrant/httpd.conf /etc/httpd/conf/
# apache restart
/sbin/service httpd restart
# enable launch settings
/sbin/chkconfig httpd on

### git install ###
# install dependency packages (temporariy install old version git)
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-ExtUtils-MakeMaker git
cd /usr/local/src/
# download latest version
git clone https://git.kernel.org/pub/scm/git/git.git
# remove old version
sudo yum remove -y git
cd git
# make
make prefix=/usr/local all
make prefix=/usr/local install

### php5.6 install ###
# install epel & remi
sudo rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
sudo rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
# install php & packages (& dependencies)
sudo yum install -y --enablerepo=remi --enablerepo=remi-php56 php php-common php-cli php-pdo php-devel php-mbstring php-mcrypt php-pear php-fpm php-pecl-igbinary php-pecl-imagick php-pecl-jsonc php-pecl-jsonc-devel php-pecl-redis php-pecl-zip php-pgsql php-process php-xml

### ref: other packages ###
# php-gd
# php-opcache
# php-mysqlnd
# php-phpunit-PHPUnit
# php-pecl-xdebug
# php-pecl-xhprof

# cp local php.ini
cp -a /vagrant/php.ini /etc/

### ref: update curl ###
sudo rpm -Uvh http://www.city-fan.org/ftp/contrib/yum-repo/city-fan.org-release-1-13.rhel6.noarch.rpm
sudo yum install -y libcurl

### FuelPHP install ###
# oil install
curl get.fuelphp.com/oil | sh

# create target directory
sudo mkdir -p /srv/example

# fuelphp1.7.3 download
cd /usr/local/src
sudo wget -O fuelphp-1.7.3.zip http://fuelphp.com/files/download/34
sudo unzip fuelphp-1.7.3.zip -d /srv/example
cd /srv/example
sudo mv fuelphp-1.7.3 fuelphp
cd fuelphp
# # update composer
# sudo php composer.phar self-update
# # pull composer dependency libraries
# php composer.phar update
sudo php oil refine install

# apache restart
sudo service httpd restart

SCRIPT

Vagrant.configure(2) do |config|

  config.vm.box = "puppetlabs/centos-6.6-64-nocm"
  config.vm.box_url = "https://vagrantcloud.com/puppetlabs/boxes/centos-6.6-64-nocm/versions/1.0.2/providers/virtualbox.box"

  config.vm.define :"web1" do |host|
    host.vm.hostname = "web1"
    host.vm.network :private_network, ip: "192.168.100.11", netmask: "255.255.255.0"
  end

  config.vm.provision "shell", inline: $script
end
