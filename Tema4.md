## Tema 4: Ejercicios

#### 1. Instala LXC en tu versión de Linux favorita. Normalmente la versión en desarrollo, disponible tanto en GitHub como en el sitio web está bastante más avanzada; para evitar problemas sobre todo con las herramientas que vamos a ver más adelante, conviene que te instales la última versión y si es posible una igual o mayor a la 1.0.

Las ordenes ejecutadas son: `sudo apt-get install lxc` y `sudo apt-get install lxc-client`.
Después `sudo apt-get update && sudo apt-get upgrade` para actualizar a la ultima versión disponible.

![Ej1](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej1_zpsihiur77v.png)

#### 2. Comprobar qué interfaces puente se han creado y explicarlos.

Creamos la máquina con `sudo lxc-create -t ubuntu -n una-caja` y `sudo lxc-create -t ubuntu-cloud -n nubecilla`. Para arrancar la máquina escribimos: `sudo lxc-start -n nubecilla`.  
Con el fin de visualizar el estado de los contenedores usamos la orden `sudo lxc-ls --fancy`.  

![Ej2](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej2_zpsfpyvky2z.png)

Ejecutando `ifconfig` podemos ver las interfaces creadas en la máquina host, correspondiendo a la subred de la máquina lanzada.

![Ej2-1](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej2-1_zpsr1crezea.png)

#### 3.1. Crear y ejecutar un contenedor basado en Debian.

Para crear el contenedor: `sudo lxc-create -t debian -n contenedor_debian`.
Para lanzarlo, una vez más: `sudo lxc-start -n contenedor_debian`.

![Ej3-1](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej3-1_zpsfvtbcafv.png)


#### 3.2. Crear y ejecutar un contenedor basado en otra distribución, tal como Fedora. Nota En general, crear un contenedor basado en tu distribución y otro basado en otra que no sea la tuya. Fedora, al parecer, tiene problemas si estás en Ubuntu 13.04 o superior, así que en tal caso usa cualquier otra distro. Por ejemplo, Óscar Zafra ha logrado instalar Gentoo usando un script descargado desde su sitio, como indica en este comentario en el issue.

Para instalar un contenedor con el template de centos ha sido necesario instalar `yum`. Con la orden `sudo apt-get install yum`.  
Una vez hecho esto, la máquina se ha creado de la misma forma que las anteriores: `sudo lxc-create -t centos -n contenedor_centos`.  

Una diferencia respecto a la creación de las anteriores es que en este caso nos ha indicado un comando a ejecutar para cambiar la contraseña por defecto:   
`sudo chroot /var/lib/lxc/contenedor_centos/rootfs passwd`

![Ej3-2](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej3-2_zpsvdtkvkph.png)

#### 4.1. Instalar lxc-webpanel y usarlo para arrancar, parar y visualizar las máquinas virtuales que se tengan instaladas.

Para instalar el panel se ha ejecutado la orden:   
`sudo wget https://lxc-webpanel.github.io/tools/install.sh -O - | sudo bash`

![Ej4-1](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej4_zpscgzclw4a.png)

#### 4.2. Desde el panel restringir los recursos que pueden usar: CPU shares, CPUs que se pueden usar (en sistemas multinúcleo) o cantidad de memoria.

Al hacer click en el nombre de uno de los contenedores accedemos al menu de customización de recursos dedicados para el mismo.  

![Ej4-2](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej4-2_zpsw23qqi9a.png)

#### 5. Comparar las prestaciones de un servidor web en una jaula y el mismo servidor en un contenedor. Usar nginx.

Tras muchos intentos de crear una instancia de una jaula, me he visto en el problema de que al loggearme dentro aparecía un prompt similar a `I have no name!@<host>`. Tras leer varios tutoriales, explicaciones y post de issues relacionados no he encontrado la manera de ejecutar la solución que parecía ser la correcta, relacionada con ejecutar la "segunda fase" de la creación de la jaula una vez dentro de la instancia.  De manera que las pruebas las he realizado tanto en nginx lanzado en el host como nginx lanzado en un contenedor.

En la máquina host, primero nos aseguramos de no tener ningun otro servidor web corriendo, y en caso afirmativo lo detenemos.  
Después instalamos nginx con la orden: `sudo apt-get install nginx`  
Y posteriormente comprobamos el estado del servicio con `sudo service nginx status`.

![Ej5-1](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej5_zpsezo4d5tk.png)

Lanzamos una instancia de algun contenedor, en este caso el denominado `una-caja` (ubuntu). Y repetimos el mismo proceso que en el host para instalar y comprobar el estado del servico nginx.

![Ej5-2](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej5-2_zpsdkzunobf.png)

Para probar las prestaciones utilizaremos `siege`. Instalándolo con la orden `sudo apt-get install siege`.  
Y posteriormente lo lanzamos en el contenedor con la orden `sudo siege -b -c 1000 -t 25S 127.0.0.1`.
De forma que se realizarán 1000 peticiones en un lapso de 25 segundos a la dirección local.

![Ej5-3](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej5-3_zpsee7urm85.png)

Repetimos el mismo proceso en el host, con la misma orden.  

![Ej5-4](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej5-4_zpslhgtmocb.png)

También se ha probado a lanzar el "benchmark" desde el host hacia el contenedor, para observar como variaban los resultados.  

![Ej5-5](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej5-5_zpswhkozpow.png)

#### 6. Instalar docker.

Primero he probado a instalarlo utilizando la orden `sudo apt-get install docker`, pero no era posible lanzar el programa de forma normal desde un terminal ya que aparecía como comando desconocido. Finalmente, la solución ha sido instalarlo como `sudo apt-get install docker.io`

#### 7.1. Instalar a partir de docker una imagen alternativa de Ubuntu y alguna adicional, por ejemplo de CentOS

La orden para instalar imagenes de docker es la siguiente: `sudo docker pull <distribucion>`. Donde la distribución puede variar segun sea un keyword aceptado por el repositorio de imagenes por defecto que tenemos en docker.  

![Ej7-1](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej7_zps6rltz04l.png)

![Ej7-1-2](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej7-2_zpslbloidax.png)

#### 7.2. Buscar e instalar una imagen que incluya MongoDB.

![Ej7-2](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej7-3_zpsmjxecjns.png)

#### 8. Crear un usuario propio e instalar nginx en el contenedor creado de esta forma.

Primero entramos una de las instancias de docker con `sudo docker run -i -t ubuntu /bin/bash`. Posteriormente lanzamos `sudo apt-get update` e instalamos nging con `sudo apt-get install nginx`.


#### 9. Crear a partir del contenedor anterior una imagen persistente con commit.

Abrimos dos terminales, en una lanzamos y entramos en una instancia de docker con `sudo docker run -i -t ubuntu /bin/bash`.  Y por otro lado leemos la ID con `sudo docker ps -a=false`.

![Ej9-1](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej9_zpsbwmcgzhp.png)

Una vez que tenemos la ID creamos la imagen persistente con un commit:
`sudo docker commit <ID> <nombre_imagen>`.

![Ej9-2](http://i1268.photobucket.com/albums/jj576/marcofp0/IV%20P4/ej9-1_zpshdcktcda.png)

#### 10. Crear una imagen con las herramientas necesarias para el proyecto de la asignatura sobre un sistema operativo de tu elección.

El proceso y la documentación al respecto están indicadas en la rama de [documentación](https://github.com/MarFerPra/teletorrent/tree/documentation) del proyecto.
