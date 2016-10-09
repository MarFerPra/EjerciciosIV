## Tema 1: Ejercicios

#### 1. Consultar en el catálogo de alguna tienda de informática el precio de un ordenador tipo servidor y calcular su coste de amortización a cuatro y siete años.

**Equipo:** [*Dell Precision T7810 Workstation*](https://www.theserverstore.com/content/dell-precision-t7810-workstation)  
**Precio:** 2645 $ = 2369 €  
**Base imponible (precio sin IVA):** 2116 $ = 1895 €

Según [esta](https://ayuda.cuentica.com/tabla-anos-porcentajes-amortizacion-simplificada-autonomos-y-profesionales/) tabla de amortización simplificada para autónomos vemos que un ordenador entraría dentro del grupo 5: *Equipos para tratamiento de la información y sistemas y programas informáticos*.
Por lo tanto su coeficiente lineal máximo es del **26 % anual** durante un período máximo de 10 años.  
Eso significa que el **máximo amartizable** para la base imponible de nuestro servidor es de **492'7 € anuales** ( 26 % de 1895 ).

* Para 4 años: 473'75 € anuales (1895 / 4).
* Para 7 años: 270'75 € anuales (1895 / 7).

Por lo que en ningún caso excedemos el máximo amortizable anual ni el período máximo.

#### 2. Usando las tablas de precios de servicios de alojamiento en Internet y de proveedores de servicios en la nube, Comparar el coste durante un año de un ordenador con un procesador estándar (escogerlo de forma que sea el mismo tipo de procesador en los dos vendedores) y con el resto de las características similares (tamaño de disco duro equivalente a transferencia de disco duro) en el caso de que la infraestructura comprada se usa sólo el 1% o el 10% del tiempo.

* 1 % de uso mensual: 7,2 horas.
* 10 % de uso mensual: 72 horas.

| Servicio  | Procesador | RAM | Disco | €/hora | Coste mensual 1% uso | Coste mensual 10% uso |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| Microsoft Azure Dv2 V2  | Intel Xeon E5-2673 v3 (Haswell) | 7 GB | 100 GB SSD | 0,236 | 1,7 € | 17 € |  
| Amazon EC2 m4.large | Intel Xeon E5-2676 v3 (Haswell) | 8 GB | 100 GB SSD (+8'9€/mes)| 0.107 | 0,77 € (+8'9€) | 16,6 € (con disco) | |

#### 3.1 ¿Qué tipo de virtualización usarías en cada caso? Comentar en el foro.
* **Virtualización para entornos de desarrollo**: Programar utilizando distintas versiones de Ruby con [RVM](https://rvm.io/rvm/basics).
*  **Virtualización de aplicaciones**: Para tener un entorno aislado de los demás por temas de seguridad. (i.e Distribución Linux [Qubes](https://www.qubes-os.org/) o [Windows Edge Virtual instances](http://arstechnica.com/information-technology/2016/09/windows-10-will-soon-run-edge-in-a-virtual-machine-to-keep-you-safe/))
* **Virtualización plena**: Uso de un sistema operativo para un determinado fin sin alterar la máquina huésped. ([Kali Linux](http://es.docs.kali.org/general-use-es/kali-linux-en-virtual-box))
*  **Virtualización a nivel de sistema operativo:** Para el despliege de distintos servicios web sobre una misma máquina, compartiendo características de una misma instancia del kernel de Linux entre varios SO. ([Docker](https://es.wikipedia.org/wiki/Docker_(software)))

*Comentario añadido: [link](https://github.com/JJ/IV16-17/issues/1)*

#### 3.2 Crear un programa simple en cualquier lenguaje interpretado para Linux, empaquetarlo con CDE y probarlo en diferentes distribuciones.

La función del programa es encontrar la clave de cifrado Vigenére de un texto en castellano, probando las posibles combinaciones para claves de longitud máxima 8.  
El código del programa puede encontrarse en el siguiente [repositorio](https://github.com/MarFerPra/vigenere-cracker).

* Empaquetado en Ubuntu 16.04:  
![Empaquetado](http://i1268.photobucket.com/albums/jj576/marcofp0/enpaquetado-cde_zpshiwg3ffz.jpg)

* Ejecución en Ubuntu 16.04:  
![Ejecución en Ubuntu 16.04](http://i1268.photobucket.com/albums/jj576/marcofp0/en-ubuntu_zpsxuibsgpu.jpg)

* Ejecución en Debian 8.6 Stable:  
![Ejecución en Debian 8.6](http://i1268.photobucket.com/albums/jj576/marcofp0/en-debian_zpstxeoirfe.jpg)

#### 4. Comprobar si el procesador o procesadores instalados tienen estos flags. ¿Qué modelo de procesador es? ¿Qué aparece como salida de esa orden?

La ejecución de la orden ``` egrep '^flags.*(vmx|svm)' /proc/cpuinfo ``` produce como salida:  

> flags: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts
acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts
rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm pcid sse4_1 sse4_2 popcnt tsc_deadline_timer aes xsave avx lahf_lm > epb tpr_shadow vnmi flexpriority ept vpid xsaveopt dtherm ida arat pln pts

(Repetido 8 veces)  
Lo cual significa que si existen lineas donde figuran los flags buscados. El modelo del procesador podemos averiguarlo ejecutando ``` cat /proc/cpuinfo ```.
En la linea correspondiente vemos la siguiente salida:  
> model name	: Intel(R) Core(TM) i7-2600K CPU @ 3.40GHz

#### 5.1 Comprobar si el núcleo instalado en tu ordenador contiene este módulo del kernel usando la orden ``` kvm-ok ```.
Le ejecución produce de salida:  
> INFO: /dev/kvm exists  
> KVM acceleration can be used

##### 5.2 Instalar un hipervisor para gestionar máquinas virtuales, que más adelante se podrá usar en pruebas y ejercicios.
El hipervisor instalado es virtualbox:  
![Virtualbox](http://i1268.photobucket.com/albums/jj576/marcofp0/virtualbox_zpseoqvquol.png)
