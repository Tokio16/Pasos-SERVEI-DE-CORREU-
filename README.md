# Guía de Instalación: Servidor de Correo en Ubuntu

Este documento detalla los pasos seguidos para configurar un servidor de correo completo con Postfix, Dovecot y Roundcube en un entorno Ubuntu.

## 1. Actualización y LAMP

Primero, actualizamos el sistema e instalamos el servidor web y la base de datos necesarios para el webmail.

sudo apt update && sudo apt upgrade
sudo apt install apache2 mysql-server -y
sudo apt install php php-mysql php-intl php-xml php-mbstring php-curl php-zip -y


## 2. Configuración de Postfix (Envío)

Instalamos Postfix y editamos el archivo /etc/postfix/main.cf para definir nuestro dominio y la estructura de las carpetas de correo:

Dominio: caparrella.local

Mailbox: Maildir/

sudo service postfix restart


## 3. Configuración de Dovecot (Recepción)

Instalamos los paquetes de Dovecot y configuramos la ubicación del correo en /etc/dovecot/conf.d/10-mail.conf:

mail_location = maildir:~/Maildir


En /etc/dovecot/conf.d/10-auth.conf permitimos el acceso con texto plano para las pruebas:

disable_plaintext_auth = no

auth_mechanisms = plain login

sudo service dovecot restart


## 4. Roundcube (Webmail)

Configuramos la interfaz web de Roundcube editando el archivo de configuración /etc/roundcube/config.inc.php:

$config['smtp_host'] = 'localhost:25';
$config['mail_domain'] = 'caparrella.local';


Habilitamos la configuración en el servidor Apache para que sea accesible desde el navegador:

sudo a2enconf roundcube
sudo service apache2 restart


## 5. Usuarios de Prueba

Para verificar que todo funciona, creamos dos usuarios y generamos sus directorios de correo correspondientes:

# Crear usuario Batman
sudo adduser batman
sudo maildirmake.dovecot /home/batman/Maildir
sudo chown -R batman:batman /home/batman/Maildir

# Crear usuario Robin
sudo adduser robin
sudo maildirmake.dovecot /home/robin/Maildir
sudo chown -R robin:robin /home/robin/Maildir

