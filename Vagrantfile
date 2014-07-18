# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chef/centos-6.5"
  
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"

  # config.ssh.forward_agent = true

  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provision :shell, :inline => <<-EOT
    # add remi-php55 repository
    rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
    # install packages
    yum -y install gcc make git
    yum -y install php php-devel pcre-devel php-mysql httpd --enablerepo=remi-php55
    # build and install Phalcon
    cd /tmp
    git clone --depth=1 git://github.com/phalcon/cphalcon.git
    cd cphalcon/build
    ./install
    sh -c "echo 'extension=phalcon.so' > /etc/php.d/phalcon.ini"
    # server setting
    cp -a /vagrant/provision/httpd.conf /etc/httpd/conf/httpd.conf
    chkconfig httpd on
    service httpd restart
    # document root setting
    chown vagrant /var/www/html
    echo '<?php phpinfo();' > /var/www/html/info.php
    # install composer
    mkdir /home/vagrant/bin
    chown vagrant /home/vagrant/bin
    cd /home/vagrant/bin
    curl -s http://getcomposer.org/installer | php
    ln -s composer.phar /usr/bin/composer
    chmod ugo+x /usr/bin/composer
  EOT
end
