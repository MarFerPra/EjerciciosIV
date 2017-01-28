## Tema 5: Ejercicios


#### 1.Instalar los paquetes necesarios para usar KVM. Se pueden seguir estas instrucciones. Ya lo hicimos en el primer tema, pero volver a comprobar si nuestro sistema está preparado para ejecutarlo o hay que conformarse con la paravirtualización.

Mediante los comandos:   
`sudo apt-get install qemu-kvm`  
`sudo apt-get install libvirt-bin`

Añadimos el usuario al grupo de *kvm* y *libvirt* con:  
`adduser <youruser> kvm` y `adduser <youruser> libvirt`  

Lanzamos el demonio libvirtd con:  `/etc/init.d/libvirtd start`  

Para la creación y manejo de máquinas virtuales también podemos valernos de `virt-manager`, que instalamos con la orden `sudo apt-get install virt-manager`


#### 2. Crear una máquina virtual Linux con 512 megas de RAM y entorno gráfico LXDE a la que se pueda acceder mediante VNC y ssh.

![01](http://i1268.photobucket.com/albums/jj576/marcofp0/01_zpsoifnugsl.jpg)

![02](http://i1268.photobucket.com/albums/jj576/marcofp0/02_zpsubb58pai.jpg)

![03](http://i1268.photobucket.com/albums/jj576/marcofp0/03_zps2r8bjzg7.jpg)

![04](http://i1268.photobucket.com/albums/jj576/marcofp0/04_zpsxblyl4vt.jpg)

![05](http://i1268.photobucket.com/albums/jj576/marcofp0/05_zpskbwpyqqm.jpg)

Una vez creada e instalado el SO, instalamos la librería para permitir a la máquina virtual actuar como servidor ssh con: `sudo apt-get install openssh-server`  
Y posteriormente hacemos uso de `ifconfig` para ver la IP en la red local que le corresponde, posteriormente  nos conectamos desde la máquina anfitriona.  

![06](http://i1268.photobucket.com/albums/jj576/marcofp0/06_zpsqfczbq3q.jpg)

#### 3. Crear una máquina virtual ubuntu e instalar en ella alguno de los servicios que estamos usando en el proyecto de la asignatura.

Para preparar el entorno antes de lanzar la aplicación han sido necesarias las siguientes ordenes:  
```
sudo apt-get install build-essential
sudo apt-get install git
sudo apt-get install nodejs
sudo apt-get install npm
sudo npm install npm@latest -g
sudo ln -s /usr/bin/nodejs /usr/bin/node
sudo git clone https://github.com/MarFerPra/teletorrent
sudo cd teletorrent/
sudo mkdir database
sudo npm install
```
Y postereriormente, lanzamos la aplicación con `npm start`, y encadenando la declaración de las variables de entorno antes, tal y como se muestra en la captura.  

![07](http://i1268.photobucket.com/albums/jj576/marcofp0/07_zps9ajk2jtt.jpg)
