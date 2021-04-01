Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder "html/", "/var/www/html", \
    create: true, group: "www-data", owner: "www-data"
  config.vm.network :forwarded_port, guest: 80, host: 8080
    config.vm.network :forwarded_port, guest: 9090, host: 9090
  config.vm.provider "virtualbox" do |virtualbox|
    virtualbox.memory = 3072
  end
  config.vm.provision "shell", inline: <<-SHELL
export DEBIAN_FRONTEND=noninteractive
apt-get update && apt-get install -y lamp-server^
sudo apt-get install -y php7.4-zip php7.4-gd cockpit adminer
bash -c 'echo -e "<Directory /var/www/html>\n\tAllowOverride All\n</Directory>" >> /etc/apache2/apache2.conf'
sed -i "s/memory_limit = 128M/memory_limit = 1G/" /etc/php/7.4/apache2/php.ini
sed -i "s/post_max_size = 8M/post_max_size = 8G/" /etc/php/7.4/apache2/php.ini
sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 8G/" /etc/php/7.4/apache2/php.ini
a2enmod rewrite && a2enconf adminer && systemctl restart apache2
mysql -u root -p --skip-password -e \
"CREATE DATABASE wordpress; \
CREATE USER 'username' IDENTIFIED BY 'password'; \
GRANT ALL ON wordpress.* TO 'username';"
  SHELL
end
