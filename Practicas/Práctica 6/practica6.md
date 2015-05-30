#PRÁCTICA 6: DISCOS EN RAID

El objetivo de esta práctica consiste en configurar dos discos en RAID 1 por software. Para ello, el primer paso que debemos realizar es el siguiente:

Partiendo de una máquina virtual ya instalada, antes de iniciarla, añadimos dos discos del mismo tipo y de la misma capacidad. En las siguientes capturas de pantalla podemos ver cual es el proceso para crear nuevos discos:

Iniciamos VMWare, seleccionamos nuestra máquina virtual creada y seleccionamos *Edit virtual machine settings*, en este punto podemos ver las características hardware de nuestra máquina virtual. Si seleccionamos el botón *Add*, nos aparecerá la siguiente ventana:

![Creacion discos 1](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-1.png)

Seleccionamos la opción *Hard Disk* y le damos a *next:

![Creacion discos 2](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-2.png)

Seleccionamos el tipo de disco que queremos crear:

![Creacion discos 3](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-3.png)

Seleccionamos la opción de crear un nuevo disco virtual:

![Creacion discos 4](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-4.png)

Le asignamos la cantidad de memoria que tendrá el disco:

![Creacion discos 5](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-5.png)

De este modo, hemos creado un nuevo disco en nuestra máquina virtual. Repetiremos este mismo proceso una vez más para conseguir los dos discos necesarios para conseguir la práctica.

Una vez configurada la máquina virtual correctamente, la arrancamos e instalamos **mdadm**, software necesario para configurar nuestro RAID. Para ello:

*sudo apt-get install mdadm*

![Instalacion mdadm](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-6.png)

Una vez instalado, buscamos con el comando *sudo fdisk -l* información de ambos discos:

![Informacion discos](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-7.png)

El siguiente paso consiste en crear el RAID 1. Para ello, utilizaremos el comando *mdadm* y le pasamos como argumentos el dispositivo */dev/md0*, el número de dispositivos que vamos a utilizar y la ubicación:

sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc

![Cracion RAID](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-8.png)

Una vez creado el dispositivo RAID, le damos el formato correspondiente con el comando *sudo mkfs /dev/md0*

![Establecer formato](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-9.png)

Por último, una vez que le hemos dado el formato a nuestro dispositivo RAID. Creamos el directorio donde se montará la unidad RAID, y la montamos:

![Creacion directorios](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-10.png)

Una vez realizado esto, comprobamos el estado del RAID, para asegurarnos de que todo ha salido bien. Para ello, tecleamos la orden:

*sudo mdadm --detail /dev/md0*

![Comprobar detalles RAID](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-11.png)

Una vez que hemos realizado correctamente todos los pasos anteriores, lo recomendable sería automatizar este proceso. El objetivo sería que, cada vez que se inicie la máquina virtual, el dispositivo RAID esté creado al arrancar el sistema. Para hacer esto bastaría con modificar el fichero **fstab** que se encuentra en el directorio */etc* y añadir la siguiente línea:

*/dev/md127	/datos	auto	0	0*

![Automatizacion](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-12.png)

**OJO:** Tras el primer reinicio, ubuntu cambia el nombre de *md0* por *md127*. 

Una vez realizada esta configuración, reiniciamos la máquina virtual y comprobamos que todo ha ido bien:

![Comprobacion final](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-13.png)

##PARTE OPTATIVA: SIMULAR EL FALLO EN UNO DE LOS DISCOS RAID

Para simular un fallo en uno de los discos RAID, volvemos a hacer uso de la herramienta **mdadm**. Con la opción *--failure* podemos simular un fallo en uno de los discos:

![Simular fallo](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-14.png)

A continuación, con la opción *--remove*, quitaremos el disco que ha fallado:

![Eliminar disco](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-15.png)

Como se puede apreciar en la captura anterior, hemos conseguido eliminar el disco. El útlimo paso que nos queda sería volver a insertar el disco con la opción *add*:

![Insertar disco](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%206/imagenes/p6-16.png)

##BIBLIOGRAFÍA

http://avdeo.com/2008/09/19/simulating-the-raid-failure/
