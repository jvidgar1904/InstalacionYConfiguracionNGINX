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
  SHELL
  end #javier

end