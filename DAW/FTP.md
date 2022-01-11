### Prerrequisitos:

-   [[Configuración Red NAT]]
-   [[Configuración IP estática]]

---
    

# Instalación del servidor

```bash
sudo apt install proftpd -y
```

Una vez instalado ya podríamos conectarnos por FTP con un cliente como por ejemplo FileZilla a nuestro servidor con una cuenta de usuario que exista en el servidor. Cabe destacar que no podremos subir archivos en cualquier directorio.

![](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fub0q4WV9Z6MJbyvmj80I%2Fuploads%2FzdSzSVjyvEsidgm5FzI5%2F0?alt=media)



## Cambiar nombre al servidor

En la línea que pone `ServerName “Debian”` podemos cambiar el nombre por el que queramos, en nuestro caso hemos puesto **ivegamerayo** por ser el nombre corporativo.

---

# Configuración básica

## Encerrar a un usuario

Con las siguientes líneas encerramos a un usuario y pedimos los usuarios virtuales que se encuentran en el archivo ftpd.passwd que crearemos más adelante

```bash
DefaultRoot ~

RequireValidShell off

AuthUserFile /etc/proftpd/ftpd.passwd
```

El archivo final sería así 


```bash
Include /etc/proftpd/modules.conf
Include /etc/proftpd/virtuals.conf

UseIPv6				off

IdentLookups			off

ServerName			"ivegamerayo"
ServerType			standalone
DeferWelcome			off

MultilineRFC2228		on
DefaultServer			on
ShowSymlinks			on

TimeoutNoTransfer		600
TimeoutStalled			600
TimeoutIdle			1200

DisplayLogin                    welcome.msg
DisplayChdir               	.message true
ListOptions                	"-l"

DenyFilter			\*.*/


DefaultRoot			~
RequireValidShell off
AuthUserFile /etc/proftpd/ftpd.passwd

Port				21


<IfModule mod_dynmasq.c>
</IfModule>

MaxInstances			30

User				proftpd
Group				nogroup


Umask				022  022

AllowOverwrite			on

TransferLog /var/log/proftpd/xferlog
SystemLog   /var/log/proftpd/proftpd.log

<IfModule mod_quotatab.c>
QuotaEngine off
</IfModule>

<IfModule mod_ratio.c>
Ratios off
</IfModule>

<IfModule mod_delay.c>
DelayEngine on
</IfModule>

<IfModule mod_ctrls.c>
ControlsEngine        off
ControlsMaxClients    2
ControlsLog           /var/log/proftpd/controls.log
ControlsInterval      5
ControlsSocket        /var/run/proftpd/proftpd.sock
</IfModule>

<IfModule mod_ctrls_admin.c>
AdminControlsEngine off
</IfModule>

Include /etc/proftpd/conf.d/

```

---

# Creación usuarios

## Crear un usuario

En nuestro caso vamos a crear un usuario llamado **test** el cual tendrá su home en /var/www/html/todo-empresaivm04, para ello utilizaremos el siguiente comando:

```bash
sudo ftpasswd --passwd --file=/etc/proftpd/ftpd.passwd --name=test --uid=60 --gid=60 --home=/var/www/html/todo-empresa-ivm04 --shell=/bin/false
```

-   **--file=** ruta_archivo
    
-   **--name=** nombre_usuario
    
-   **--uid=** id_usuario
    
-   **--gid=** grupo
    
-   **--home=** ruta_home_usuario
    
-   **--shell**=shell_usuario
    

## Crear un grupo

No es totalmente necesario, además en esta documentación no explicamos como usarlo.

```bash
sudo ftpasswd --group --name=nogroup --file=/etc/proftpd/ftpd.group --gid=60 --member test
```

## Comprobar la configuración

```bash
sudo proftpd -t
```

## Reiniciar y comprobar los cambios

```bash
sudo service proftpd restart
```

```bash
sudo service proftpd status
```

## Cambiar la contraseña

```bash
sudo ftpasswd --passwd --file=/etc/proftpd/ftpd.passwd --name=test --change-password
```

## Eliminar usuario

```bash
sudo ftpasswd --passwd --file=/etc/proftpd/ftpd.passwd --name=test --delete-user
```

---
# Crear directorio para usuario
Aunque lo hemos indicado cuando hemos creado el usuario, el directorio no existe asi que tenemos que añadirlo nosotros con el siguiente comando.

```bash
sudo mkdir /var/www/html/todo-empresa-ivm04
```

__¡Importante!__ Damos permisos a la carpeta

```bash
sudo chmod -R 777 /var/www/html/todo-empresa-ivm04
```

Hemos elegido este directorio ya que el usuario sera para gestionar la web.

---

# Creación VirtualHosts
## Configuracion básica
Nos dirigimos al archivo virtuals que se encuentra en ```/etc/proftpd```  y lo rellenamos con el siguiente codigo.

```bash
<VirtualHost 192.168.1.122>

ServerAdmin ivegamerayo@server.com

ServerName "FTP Empresa IVM04"

TransferLog /var/log/proftpd/xfer/ftp.server.com

MaxLoginAttempts 3

RequireValidShell no

DefaultRoot /var/www/html/todo-empresa-ivm04

</VirtualHost>
```

En este caso tenemos la configuracion de nuestro virtualhost para que podamos acceder mediante el dominio de la empresa.


## Configuracion conexion por IP
En este caso nos vamos al archivo ```/etc/hosts``` el cual tendrá una configuracion parecida a la siguiente.

![[Pasted image 20211218152410.png]]
En la cual añadiremos al final o donde queramos la siguiente linea como podemos ver en nuestra captura de arriba.
```bash
127.0.0.1	ftp.empresa-ivm04.local
```

---

# Pruebas finales (Sin TLS)
En este caso reiniciamos el servidor FTP con los siguientes comandos:
```bash
sudo service proftpd restart
sudo service proftpd status
```


Si no nos ha dado ningun problema continuamos.

### Configuracion conexión por IP (en cliente)
En este caso nos vamos al archivo ```/etc/hosts``` el cual tendrá una configuracion parecida a la siguiente. **¡OJO! Cabe destacar que esto debe de hacerse en el ordenador cliente ya que no estamos disponiendo de servidor DNS**

En la cual añadiremos al final o donde queramos la siguiente linea como podemos ver en nuestra captura de arriba.
```bash
ip_servidor	ftp.empresa-ivm04.local
```

## Conexión desde un cliente
En este caso vamos a utilizar un programa para conectarnos de forma visual, el cual se llama **FileZilla** el cual su funcionamiento es muy sencillo.
Rellenamos los siguientes campos
![[Pasted image 20211218153730.png]]

**Servidor:** Puede ser la IP del servidor o el dominio.
**Nombre de usuario:** Un usuario que esté dado de alta en el servidor.
**Contraseña:** Contraseña del usuario.
**Puerto:** Por defecto es el 21 aunque si se diese el caso que fuese distinto escribiriamos el puerto necesario.

Una vez rellenados los campos pulsamos en **_Conexión rápida_**

![[Pasted image 20211218154111.png]] 
Veremos que a la derecha nos aparece /, en nuestro caso ya nos aparece un archivo, pero no deberia de aparecer si la carpeta está vacía.

Vamos a probar que todo funciona arrastrando un archivo del lateral izquierdo a la derecha.

Si todo funciona veremos abajo lo siguiente.
![[Pasted image 20211218154403.png]]

---

# Configuracion TLS
## Instalación de openssl
```
sudo apt update -y && sudo apt upgrade -y
sudo apt install openssl -y
```

Una vez terminada la instalación vamos a generar el certificado.

```bash
openssl req -x509 -newkey rsa:2048 -keyout /etc/ssl/private/proftpd.key -out /etc/ssl/certs/proftpd.crt -nodes -days 365
```

Nos hará las siguientes preguntas, aquí se encuentran nuestras respuestas:

```bash
Generating a 1024 bit RSA private key
.++++++
.......................++++++
writing new private key to '/etc/ssl/private/proftpd.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Spain
Locality Name (eg, city) []:Ponferrada
Organization Name (eg, company) [Internet Widgits Pty Ltd]:ivegamerayo
Organizational Unit Name (eg, section) []:ivegamerayo
Common Name (e.g. server FQDN or YOUR name) []:ftp.empresa-ivm04.local
Email Address []:admin@ftp.empresa-ivm04.local
```

Con el siguiente comando se nos ha generado dos archivos en ```/etc/ssl/private/proftpd.key``` y ```/etc/ssl/certs/proftpd.crt```

Damos los permisos a los siguientes archivos con los siguientes comandos:

```bash
sudo chmod 0600 /etc/ssl/private/proftpd.key
sudo chmod 0644 /etc/ssl/certs/proftpd.crt
```

## Configurando ProFTPD para usar SSL
Nos vamos al archivo ```/etc/proftpd/proftpd.conf```

Añadimos lo siguiente debajo de los dos Includes que tenemos al principio.

```bash
Include /etc/proftpd/tls.conf
```

Guardamos y nos vamos al fichero ```/etc/proftpd/tls.conf```

Descomentamos las siguientes lineas quitando # del principio

```bash
TLSRSACertificateFile /etc/ssl/certs/proftpd.crt
TLSRSACertificateKeyFile /etc/ssl/private/proftpd.key
TLSEngine on
TLSLog /var/log/proftpd/tls.log
TLSProtocol SSLv23
TLSRequired on
TLSOptions NoCertRequest EnableDiags NoSessionReuseRequired
TLSVerifyClient off
```

Guardamos el archivo, lo cerramos y reiniciamos el servidor.

Nos vamos a FileZilla e iniciamos sesion como anteriormente, a diferencia de la anterior, ahora deberia de aparecernos esto. (Solo es la primera vez)
![[Pasted image 20211218185413.png]]


---

# Creación de plantilla para FileZilla
1. Nos vamos a archivo y pinchamos en gestor de sitios.
2. Rellenamos los campos como vemos en pantalla y listo.
3. Pulsamos en guardar.

![[Pasted image 20211218175217.png]]

![[Pasted image 20211218190025.png]]
Ahora si pinchamos en ese servidor automaticamente se conecta.