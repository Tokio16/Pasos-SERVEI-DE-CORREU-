Guía de Instalación: Servidor de Correo en Ubuntu

Este documento detalla los pasos para configurar un servidor de correo completo con Postfix, Dovecot y Roundcube.

1. Actualización y LAMP

Primero, actualizamos el sistema e instalamos el servidor web y base de datos.

sudo apt update && sudo apt upgrade
sudo apt install apache2 mysql-server -y
sudo apt install php php-mysql php-intl php-xml php-mbstring php-curl php-zip -y


2. Configuración de Postfix (Envío)

Edita /etc/postfix/main.cf con los siguientes valores de red y dominio:

Dominio: caparrella.local

Mailbox: Maildir/

sudo service postfix restart


3. Configuración de Dovecot (Recepción)

Instalamos Dovecot y configuramos la ubicación del correo en /etc/dovecot/conf.d/10-mail.conf:

mail_location = maildir:~/Maildir


En /etc/dovecot/conf.d/10-auth.conf permitimos el acceso:

disable_plaintext_auth = no

auth_mechanisms = plain login

4. Roundcube (Webmail)

Configuramos la interfaz web editando /etc/roundcube/config.inc.php:

$config['smtp_host'] = 'localhost:25';
$config['mail_domain'] = 'caparrella.local';


Habilitamos la configuración en Apache:

sudo a2enconf roundcube
sudo service apache2 restart


5. Usuarios de Prueba

# Crear usuario Batman
sudo adduser batman
sudo maildirmake.dovecot /home/batman/Maildir
sudo chown -R batman:batman /home/batman/Maildir
