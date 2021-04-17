Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder "html/", "/var/www/html", \
    create: true, group: "www-data", owner: "www-data"
  config.vm.network :forwarded_port, guest: 80, host: 8080
    config.vm.network :forwarded_port, guest: 9090, host: 9091
  config.vm.provider "virtualbox" do |virtualbox|
    virtualbox.memory = 3072
  end
  config.vm.provision "shell", inline: <<-SHELL

export DEBIAN_FRONTEND=noninteractive
apt-get update && apt-get install -y lamp-server^
apt-get install -y php7.4-zip php7.4-gd

sed -i "s/memory_limit = 128M/memory_limit = 1G/" /etc/php/7.4/apache2/php.ini
sed -i "s/post_max_size = 8M/post_max_size = 8G/" /etc/php/7.4/apache2/php.ini
sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 8G/" /etc/php/7.4/apache2/php.ini

bash -c 'echo -e "<Directory /var/www/html>\n\tAllowOverride All\n</Directory>" >> /etc/apache2/apache2.conf'
a2enmod rewrite

apt-get install -y cockpit

add-apt-repository -y ppa:ondrej/php
apt-get install -y php8.0 libapache2-mod-php8.0 php8.0-mysql php8.0-mbstring php8.0-zip php8.0-gd
a2dismod php7.4 && a2enmod php8.0

sed -i "s/memory_limit = 128M/memory_limit = 1G/" /etc/php/8.0/apache2/php.ini
sed -i "s/post_max_size = 8M/post_max_size = 8G/" /etc/php/8.0/apache2/php.ini
sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 8G/" /etc/php/8.0/apache2/php.ini

systemctl restart apache2

mysql -u root -p --skip-password -e \
"CREATE DATABASE wordpress; \
CREATE USER 'username' IDENTIFIED BY 'password'; \
GRANT ALL ON wordpress.* TO 'username';"
  SHELL
end
