#PRÁCTICA 5: REPLICACIÓN DE BASES DE DATOS MYSQL

En esta práctica veremos como realizar una copia de seguridad de nuestra base de datos de un servidor a otro.
En primer lugar, para poder realizar las diferentes técnicas para copiar la base de datos, crearemos una base de datos en MySQl e insertaremos algunos datos en ella.

Tecleamos el comando:

*mysql -u root -p*

Para acceder a nuestra base de datos, a continuación creamos la tabla de la siguiente manera:

![Captura creacion BD](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.1.png)

Y para insertar datos hacemos:

![Captura insertar BD](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.2.png)

Una vez tenemos los preparativos hechos, pasamos a realizar la copia de la base de datos de un servidor a otro.

##Replicar una BD MySQL con mysqldump

En primer lugar, debemos configurar la base de datos para que no se modifique nada en las tablas mientras estamos actualizando el servidor, para ello ejecutamos la orden *FLUSH TABLES WITH READ LOCK* en nuestra base de datos:

![Captura bloquear tablas](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.3.png)

A continuación, en el servidor 1 ejecutamos la siguiente orden:

*mysqldump contactos -u root -p > /root/contactos.sql*

![Captura copia BD](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.4.png)

Con la orden anterior, hemos guardado los datos de la base de datos en el servidor 1. En el próximo paso desbloqueamos la base de datos:

![Captura desbloquear tablas](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.5.png)

Ahora nos pasamos a la máquina 2 y copiamos el arhivo *.sql* con todos los datos que hemos guardado en la máquina 1:

![Captura copia maquina 2](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.6.png)

Una vez en este punto es necesario destacar que, el fichero que hemos copiado es solo un texto plano que contiene los comandos necesarios para recuperar la base de datos que hemos copiado. Para restaurar la base de datos de la máquina 1 en la máquina 2, debemos entrar en la base de datos de la máquina 2 e importar la base de datos de la máquina 1. Para ello hacemos lo que se muestra en la siguiente captura de pantalla:

![Captura restaurar BD](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.7.png)

Por último, en la siguiente captura de pantalla podemos comprobar que todo ha salido bien:

![Captura restaurar BD](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.8.png)

##Replicación de BD mediante una configuración maestro-esclavo

Utilizando lo anteriormente descrito, todo funciona correctamente, pero hay que hacerlo manualmente. Lo interesante sería conseguir hacer este proceso automáticamente, para ello vamos a hacer una configuración maestro esclavo en nuestra base de datos de MySQL.

Para comenzar con la configuración maestro-esclavo, nos dirigimos primero a la máquina 1 (que será el maestro), abrimos el fichero *my.conf* que se encuentra en */etc/mysql* y comentamos la siguiente línea:

![Captura modificacion1 my.conf](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.9.png)

Y añadimos los siguientes campos:

![Captura modificacion2 my.conf](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.10.png)

Reiniciamos el servicio de mysql y si no nos muestra ningún error, quiere decir que todo ha salido correctamente:

![Captura reinicio maestro](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.11.png)

Ahora, nos dirigimos al máquina 2 y modificamos el fichero */etc/mysql/my.conf* y cambiamos la variable server-id a 2 (como el maestro tiene el id 1, al esclavo le damos el id 2)

![Captura modificacion esclavo](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.12.png)

Una vez hemos modificado el fichero, reiniciamos el servicio mysql *sudo service mysql restart*.

Ahora, nos dirigimos a la máquina 1 (maestro) y creamos el usuario esclavo y le damos los permisos necesarios para que pueda realizar la replicación:

![Captura creacion user esclavo](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.13.png)

Por último, nos dirigimos a la máquina 2 (esclavo) y configuramos quién "va a ser su maestro", para ello:

![Captura modificacion maestro](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.14.png)

De este modo, tenemos configurado nuestro servicio maestro esclavo y podremos realizar copias automáticamente de la base de datos de la máquina 1 a la máquina 2. Si todo ha ido bien, escribiendo en la máquina 2 la siguiente orden:

*show slave status\G*

Si la línea Second_Behind_Master tiene el valor 0, quiere decir que todo ha ido bien. Podemos comprobar esto último en la siguiente captura de pantalla:

![Captura todo OK](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.15.png)

En la siguiente captura podemos ver que modificamos la base de datos de la máquina 1 y, automáticamente se actualiza la base de datos de la máquina 2:

![Captura insertar maestro](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.16.png)

![Captura comprobacion esclavo](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%205/imagenes/5.17.png)
 
