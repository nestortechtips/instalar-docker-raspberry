![Nestor Tech Tips](https://nestortechtips.online/wp-content/uploads/2020/10/cropped-default-2-1.png)
# Cómo instalar Docker en Ubuntu 18.04 corriendo en Raspberry Pi

El día de hoy vamos a instalar Docker en una Raspberry Pi 3B ejecutando Ubuntu 18.04

Primero deshabilitaremos la memoría swap de la Raspberry Pi
```bash
root$ swapoff -a
```


A continuación indicaremos al Sistema Operativo que enforce los **controlgroups** para limitar el uso de recursos de los contenedores


```bash
root$ sed -i -e 's/fixrtc/fixrtc cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory/g' /boot/firmware/nobtcmd.txt 
```


Para que se apliquen los cambios reiniciaremos nuestra Raspberry Pi


```bash
root$ reboot
```


Una vez que se haya reiniciado la Raspberry Pi añadiremos la llave pública del repositorio de Docker


```bash

root$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
```


Ahora agregamos el repositorio a la lista de repositorios disponibles en el sistema


```bash
root$ add-apt-repository "deb &#91;arch=arm64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```


Actualizamos la lista de paquetes disponibles e instalamos Docker


```bash
root$ apt-get update &amp;&amp; apt-get install -y docker-ce
```


Docker se encuentra listo para su uso, como mejor práctica añadiremos nuestro usuario al grupo docker para poder crear contenedores sin ser root. Antes de ejecutar el comando no olvidemos reemplazar *$USUARIO **con el nombre de usuario con el que accedemos a nuestra Raspberry Pi


```bash
root$ usermod -aG docker $USUARIO
```


Para que se apliquen los cambios, cerraremos la sesión actual. Podemos cerrar la sesión o cerrar y abrir la terminal de nuevo.Comprobaremos que Docker funcione de manera correcta, haciendo un *pull* de la imagen nginx y ejecutaremos un contenedor que use esta imagen.


```bash
usuario$ docker image pull nginx
usuario$ docker container run --name webserver -d -p 5000:80 nginx 
```


Comprobarémos que el contenedor ha sido creado de forma exitosa realizando una petición


```bash
usuario$ curl localhost:5000
```


La respuesta del comando *curl** debe mostrarnos la página inicio de Nginx

## Contribuciones
Puedes contribuir utilizando **Pull Requests** en el repositorio correspondiente. De no existir crea un archivo *contrib.txt*, de existir modifica el archivo para agregar tu nombre de usuario o correo. Por ejemplo:
* `fulano.detal@correo.com`
* `perengano`

## Créditos
El contenido de este repositorio corresponde a la publicación homónima en [nestortechtips.online](https://nestortechtips.online). 