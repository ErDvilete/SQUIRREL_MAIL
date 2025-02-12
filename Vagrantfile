# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  

  
  config.vm.define "mail" do |mail|
    mail.vm.hostname = "mail"
    mail.vm.network "private_network", ip: "192.168.56.10"
    mail.vm.network "public_network",  ip: "192.168.57.10", bridge: "enp2s0"
    mail.vm.provision "shell", inline: <<-SHELL

      apt-get update && apt-get upgrade -y

      #Instalar paquetes

      apt-get install bind9 -y
      apt-get install apache2 -y
      apt-get install php libapache2-mod-php -y
      apt-get install dovecot-core dovecot-imapd dovecot-pop3d -y
      

      #Copiar ficheros DNS

      cp -v /vagrant/dns/named /etc/default/
      cp -v /vagrant/dns/db.56.168.192 /etc/bind
      cp -v /vagrant/dns/db.izv.test /etc/bind
      cp -v /vagrant/dns/named.conf.local /var/lib/bind
      cp -v /vagrant/dns/named.conf.options /var/lib/bind
      
      #Descargar SquirrelMail

      
      tar xvfz /vagrant/webmail/squirrelmail.tgz -C /usr/local
      ln -s /usr/local/squirrelmail-webmail-1.4.22 /var/www/mail
      sudo mkdir -p /var/local/squirrelmail/data
      chown www-data:www-data /var/local/squirrelmail/data
      mkdir -p /var/local/squirrelmail/attach
      chown www-data:www-data /var/local/squirrelmail/attach
      cp -v /vagrant/webmail/config.php /usr/local/squirrelmail-webmail-1.4.22/config/config.php
      cp -r /usr/local/squirrelmail-webmail-1.4.22/ /var/www/mail/

      #Cambiar el idioma
      locale-gen es_ES

      #Configirar el webmail

      cp -v /vagrant/webmail/hostname /etc/hostname
      cp -v /vagrant/webmail/hosts /etc/hosts
      cp -v /vagrant/webmail/resolv.conf /etc/resolv.conf

      #Configurar Postfix

      cp -v /vagrant/postfix/main.cf /etc/postfix/

      #Configurar apache2 y habilitar modulos

      cp -v /vagrant/apache2/apache2.conf /etc/apache2/apache2.conf
      cp -v /vagrant/apache2/mail.conf /etc/apache2/sites-available/mail.conf
      a2ensite mail
      a2dissite 000-default
      systemctl restart apache2

      # Instalamos postfix de manera manual con el comando
      #sudo apt install postfix -y
      #Y lo configuramos con internet site y de system name "Mail"



      #Creo los usuarios de manera manual dentro de la maquina con el comando adduser de mengano y fulano con contraseña m y f correspondientemente

      
    SHELL
    end

end
