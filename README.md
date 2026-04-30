# Guía de Instalación: Servidor de Correo en Ubuntu

Este documento detalla los pasos seguidos para configurar un servidor de correo completo con Postfix, Dovecot y Roundcube en un entorno Ubuntu.

## 1. Actualización y LAMP

Primero, actualizamos el sistema e instalamos el servidor web y la base de datos necesarios para el webmail.
```bash
sudo apt update && sudo apt upgrade
sudo apt install apache2 mysql-server -y
sudo apt install php php-mysql php-intl php-xml php-mbstring php-curl php-zip -y
```
## 2.Instalación de Postfix (Envío) 
Aquí se instala el entorno necesario para aplicaciones web. Apache permitirá acceder vía navegador, MySQL almacenará datos y PHP ejecutará aplicaciones como el webmail.
Instalamos el servidor SMTP.
```bash
sudo apt install postfix
```
Instalamos soporte con Dovecot.
```bash
sudo apt install postfix dovecot-imapd
sudo apt install postfix dovecot-pop3d
```
Configuramos el archivo principal.
```bash
sudo nano /etc/postfix/main.cf
```
```conf

# /etc/postfix/main.cf
myhostname = caparrella.local
mydomain = caparrella.local
myorigin = /etc/mailname
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
home_mailbox = Maildir/
```
![configuracion][configuracion.png]

## 3. Configuración de Postfix (Envío)

Instalamos Postfix 
```bash
sudo apt install postfix
sudo apt install postfix dovecot-imapd
sudo apt install postfix dovecot-pop3d
```


## 4. Configuración de Dovecot (Recepción)

Instalamos los paquetes de Dovecot y configuramos la ubicación del correo en /etc/dovecot/conf.d/10-mail.conf:

mail_location = maildir:~/Maildir


En /etc/dovecot/conf.d/10-auth.conf permitimos el acceso con texto plano para las pruebas:

disable_plaintext_auth = no

auth_mechanisms = plain login

sudo service dovecot restart


## 5. Roundcube (Webmail)

Configuramos la interfaz web de Roundcube editando el archivo de configuración /etc/roundcube/config.inc.php:

$config['smtp_host'] = 'localhost:25';
$config['mail_domain'] = 'caparrella.local';


Habilitamos la configuración en el servidor Apache para que sea accesible desde el navegador:

sudo a2enconf roundcube
sudo service apache2 restart


## 6. Usuarios de Prueba

Para verificar que todo funciona, creamos dos usuarios y generamos sus directorios de correo correspondientes:

# Crear usuario Batman
sudo adduser batman
sudo maildirmake.dovecot /home/batman/Maildir
sudo chown -R batman:batman /home/batman/Maildir

# Crear usuario Robin
sudo adduser robin
sudo maildirmake.dovecot /home/robin/Maildir
sudo chown -R robin:robin /home/robin/Maildir

