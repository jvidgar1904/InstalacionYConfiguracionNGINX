# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"

  config.vm.define "javier" do |javier|
  javier.vm.network "private_network", ip: "192.168.57.10"

  javier.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y git
    sudo git config --global core.safecrlf true
    sudo apt-get update
    sudo apt-get install -y vsftpd
    sudo apt install -y nginx
    sudo apt install ufw
    sudo apt install -y openssl

    sudo mkdir -p /var/www/javiWeb/html
    git clone https://github.com/cloudacademy/static-website-example /var/www/javiWeb/html
    sudo chown -R www-data:www-data /var/www/javiWeb/html
    sudo chmod -R 755 /var/www/javiWeb

    sudo cp /vagrant/files/javiWeb /etc/nginx/sites-available/javiWeb

    sudo ln -sf /etc/nginx/sites-available/javiWeb /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl restart nginx

    # FTP

    mkdir /home/vagrant/ftp
    sudo echo "vagrant:vagrant" | chpasswd

    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
      -keyout /etc/ssl/private/vsftpd.key \
      -out /etc/ssl/certs/vsftpd.crt \
      -subj "/C=ES/ST=Andalucía/L=Granada/O=IZV/OU=WEB/CN=vsftpd/emailAddress=webmaster@vsftpd.com"
    
    sudo cp /vagrant/files/vsftpd.conf /etc/vsftpd.conf
    sudo ufw allow 40000:40100/tcp
    sudo systemctl restart vsftpd


    # Práctica 2.2. Autenticación en Nginx. + Práctica 2.3. Acceso seguro con Nginx

    sudo ufw allow ssh
    sudo ufw allow 'Nginx Full'
    sudo ufw delete allow 'Nginx HTTP'
    sudo ufw --force enable

    sudo openssl req -x509 -nodes -days 365 \
      -newkey rsa:2048 -keyout /etc/ssl/private/perfectLearn.com.key \
      -out /etc/ssl/certs/perfectLearn.com.crt \
      -subj "/C=ES/ST=Andalucía/L=Granada/O=IZV/OU=WEB/CN=perfectLearn.com/emailAddress=webmaster@perfectLearn.com"

    sudo mkdir -p /var/www/perfectLearn/
    sudo cp -r /vagrant/files/html /var/www/perfectLearn/
    sudo chown -R www-data:www-data /var/www/perfectLearn/html
    sudo chmod -R 755 /var/www/perfectLearn

    sudo cp /vagrant/files/perfectLearn /etc/nginx/sites-available/perfectLearn

    sudo ln -sf /etc/nginx/sites-available/perfectLearn /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl restart nginx


    sudo sh -c "echo -n 'Javier:' >> /etc/nginx/.htpasswd"
    sudo sh -c "openssl passwd -apr1 'javier' >> /etc/nginx/.htpasswd"

    sudo sh -c "echo -n 'Vidal:' >> /etc/nginx/.htpasswd"
    sudo sh -c "openssl passwd -apr1 'vidal' >> /etc/nginx/.htpasswd"
    
    sudo cat /etc/nginx/.htpasswd

    
    
    sudo nginx -t
    sudo systemctl restart nginx
    
  SHELL
  end #javier

end