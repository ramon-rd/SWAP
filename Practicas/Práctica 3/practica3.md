#PRACTICA 3: BALANCEO DE CARGA

##Configurar una máquina e instalar nginx como balanceador de carga.

En primer lugar, en caso de no tener instalado nginx, debemos instalarlo. Para ello,
debemos descargar la clave del repositorio y lo almacenamos en el directorio /tmp/:

*cd /tmp*
*wget http://nginx.org/keys/nginx_signing.key*

Ahora, importamos la clave del repositorio:

*apt-key add /tmp/nginx_signing.key*
*rm -f /tmp/nginx_signing.key*

Por último, añadimos dicho repositorio editando el fichero /etc/apt/sources.list y añadimos las 
siguientes líneas:

*echo "deb http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list*
*echo "deb-src http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list*

Y por fin, podemos instalar **nginx**

*apt-get update*
*apt-get install nginx*

![Instalacion nginx](/home/ramon/capturasSWAP/p3-1.png?raw=true)

Debemos cambiar la configuración por defecto de nginx, para hacer esto debemos modificar el siguiente fichero
de configuración:

*/etc/nginx/conf.d/default.conf*

En este fichero debemos indicar qué máquinas formarán nuestro cluster web y en qué puertos está a la escucha
el servidor web.

![Nginx configuracion](/home/ramon/capturasSWAP/P3-7.png)

En el ejemplo de la captura anterior se ha usado balanceo de carga usando el algoritmo de round-robin donde la 
máquina 1 tiene el doble de capacidad que la máquina 2. Una vez configurado, si queremos lanzar el balanceador nginx debemos hacer:

*sudo service nginx restart*

Si todo ha ido bien podemos hacer peticiones a los servidores usando el comando **curl** hacia la máquina balanceadora.
El comando **curl** podemos ejecutarlo desde nuestra propia máquina o desde una cuarta máquina virtual que hallamos creado. 
En la siguiente captura de pantalla podemos comprobar que todo ha ido bien. 

Para poder distinguir de una máquina a otra, la máquina servidora 1 muestra el mensaje *Esto funciona 1* y la máquina servidora 2 
muestra el mensaje *Esto funciona 2*.

![Prueba curl1](/home/ramon/capturasSWAP/nginxMaquina1DoblePotente.png)

##Configurar una máquina e instalar haproxy como balanceador de carga.

Igual que con nginx, lo primero que debemos hacer es instalarlo:

*apt-get install haproxy joe*

Una vez instalado, configuramos **haproxy** como balanceador de carga, para ello, modificamos el archivo:

*cd /etc/haproxy*
*sudo nano haproxy.cfg*

Para configurar el balanceador haproxy de modo que realice el reparto de carga utilizando el algoritmo *round robin*
debemos añadir dentro del bloque *defaults* (para que se aplique a todos los servidores) la siguiente línea:

**balance roundrobin**

A continuación, fijamos en el bloque *frontend http-in* el puerto al que escucha la máquina balanceadora y por último,
fijamos en el bloque *backend servers* las direcciones ip de los servidores que vamos a *balancear*. Además, también debemos
indicar el peso que tiene cada máquina, como se indica en el guión de prácticas, la máquina 1 es el doble de potente que la máquina 2.

En este caso es necesario destacar que la configuarción no es la misma que con **nginx**, con el balanceador **haproxy** si estamos utilizando un algoritmo de ponderación (como es el de round robin) el peso de cada servidor se representa de *1 a 100*, por tanto si queremos
que la máquina 1 *cargue* con el doble de trabajo respecto a la segunda máquina debemos indicarlo como se muestra en la siguiente captura
de pantalla:

![Captura RoundRobin](/home/ramon/capturasSWAP/roundrobinHaproxy.png)

Si todo ha salido bien, lanzamos el servicio haproxy:

*sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg*

En mi caso, a la hora de lanzar el servicio haproxy obtuve un error ya que el servicio nginx aun seguía
ejecutandose. Para solucionar este problema hice lo siguiente:

*ps aux | grep nginx*

El comando anterior es para buscar si existe algún proceso con dicho nombre y a continuación, como podemos observar
en la captura de pantalla, detenemos la ejecución de nginx:

![Error nginx](/home/ramon/capturasSWAP/P3-9.png)

Ahora volvemos a lanzar el servicio haproxy y comprobamos el funcionamiento de haproxy utilizando el comando **curl**. 

![Prueba curl2](/home/ramon/capturasSWAP/pruebaRoundRobinHaproxy.png)
