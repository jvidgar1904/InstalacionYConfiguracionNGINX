# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"

  config.vm.define "javier" do |javier|
  javier.vm.network "private_network", ip: "192.168.57.10"

  javier.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y git
    sudo apt install -y nginx

    sudo mkdir -p /var/www/javiWeb/html
    git clone https://github.com/cloudacademy/static-website-example /var/www/javiWeb/html
    sudo chown -R www-data:www-data /var/www/javiWeb/html
    sudo chmod -R 755 /var/www/javiWeb

    sudo cp /vagrant/files/javiWeb /etc/nginx/sites-available/javiWeb

    sudo ln -sf /etc/nginx/sites-available/javiWeb /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl restart nginx

    # Práctica 2.2. Autenticación en Nginx.

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
    
  SHELL
  end #javier

end