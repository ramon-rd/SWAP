#PRÁCTICA 4: COMPROBAR EL RENDIMIENTO DE SERVIDORES WEB

En esta práctica haremos uso de tres herramientas diferentes para comprobar el rendimiento de nuestro
servidor web, estas herramientas son:

-Apache Benchmark

-Httperf

-Openload

Para cada una de estas herramientas realizaremos las siguientes pruebas:

Prueba de rendimiento a una sola máquina (la primera, por ejemplo)
Prueba de rendimiento a nuestra granja web (con el balanceador nginx)
Prueba de rendimiento a nuestra granja web (con el balanceador haproxy)

Para cada una de las pruebas, realizaremos 10 test, y solicitaremos un pequeño archivo html que devuelve el siguiente mensaje:

*Esto es una prueba*

El objetivo, calcular la media y la desviación estándar de los resultados obtenidos, para conocer cual de los tres servidores resuelve un mayor número de peticiones con el menor número de errores posible.

##Apache Benchmark

En primer lugar, debemos instalar esta herramienta en una máquina virtual que hallamos creado previamente,
para ello:

*sudo apt-get install apache2-utils*

ab -n 100000 -c 100 http://192.168.54.130

El comando anterior quiere decir que solicitaremos 100000 veces la página que se encuentra en la IP que se muestra al
final. Además, realizaremos las peticiones concurrentemente de 100 en 100.

**Prueba de rendimiento a la primera máquina:**

ab -n 100000 -c 100 http://192.168.54.130/index.html


|	Test		| Duración de la prueba |Solicitudes fallidas|Solicitudes por segundo| 
|:---------:|:---------------------:|:------------------:|:---------------------:|
|		1	   |		78.865				|			0				|			1267.99			|
|		2		|  	80.49					|			0				|			1243.02			|
|		3		|  	80.344				|			0				|			1244.65			|
|		4		|  	80.484				|			0				|			1242.49			|
|		5		|		80.709				|			0				|			1239.02			|
|		6		|		80.237				|			0				|			1246.31			|
|		7		|		81.1					|			0				|			1233.05			|
|		8		|		81.149				|			0				|			1232.3			|
|		9		|		81.835				|			0				|			1221.98			|
|		10		|		80.502				|			0				|			1242.2			|



**Prueba de rendimiento a la granja web (con el balanceador nginx):**

ab -n 100000 -c 100 http://192.168.54.135/index.html


|	Test		| Duración de la prueba	|Solicitudes fallidas|Solicitudes por segundo| 
|:---------:|:---------------------:|:------------------:|:---------------------:|
|		1	   |			125.950			|			0				|			793.97			|
|		2		|  		127.025			|			0				|			787.25			|
|		3		|  		122.191			|			0				|			819.39			|
|		4		|  		128.810			|			0				|			776.34			|
|		5		|			162.339			|			0				|			615.99			|
|		6		|			132.429			|			0				|			755.12			|
|		7		|			130.668			|			0				|			765.3				|
|		8		|			128.075			|			0				|			780.79			|
|		9		|			124.996			|			0				|			800.03			|
|		10		|			125.308			|			0				|			798.03			|

**Prueba de rendimiento a la granja web (con el balanceador haproxy):**

ab -n 100000 -c 100 http://192.168.54.135/index.html

|	Test		| Duración de la prueba	|Solicitudes fallidas|Solicitudes por segundo|
|:---------:|:---------------------:|:------------------:|:---------------------:|
|		1	   |		105.243				|			0				|			950.18			|
|		2		|  	106.49				|			0				|			939.06			|
|		3		|  	109.284				|			0				|			915.05			|
|		4		|  	102.104				|			0				|			979.39			|
|		5		|		99.809				|			0				|			1001.91			|
|		6		|		100.484				|			0				|			995.19			|
|		7		|		100.856				|			0				|			991.51			|
|		8		|		102.45				|			0				|			976.09			|
|		9		|		101.141				|			0				|			988.72			|
|		10		|		104.339				|			0				|			958.42			|

![Captura Media Duración Prueba](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/AB-media-tiempo.png)
![Captura Desviación Duración Prueba](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/AB-desviacion-duracionprueba.png)

Como se puede observar, cuando las pruebas son lanzadas a un servidor único (en este caso el servidor 1) el tiempo de respuesta es
mucho menor que utilizando haproxy o nginx en la granja web. Además, como se puede observar en los siguientes gráfico, el servidor 1 atiende
en media a un mayor número de solicitudes. 
En cuanto a la desviación estandar, tanto en el tiempo de respuesta como en la tasa de solicitudes completadas del servidor 1 es el más fiable, ya que no tiene *picos* y su desviación es mucho más baja respecto a las otras dos herramientes.

![Captura Media Solicitudes/segundo](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/AB-media-solicitudes-seg.png)
![Captura Desviación Solicitudes/segundo](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/AB-desviacion-solicitudes-seg.png)

##Httperf

Lo siguiente que haremos será instalar la herramienta **httperf** y ejecutaremos las mismas pruebas que en el 
apartado anterior. Para ello:

*sudo apt-get install httperf*

Una vez que lo hemos instalado, lanzamos los test hacia la primera máquina, a la granja web usando como balanceador
**nginx** y utilizando **haproxy**. Para ello:

*httperf --server 192.168.54.130 --port 80 --uri /index.html --rate 100 --num-conn 10000 --num-call 100 --timeout 5*

La orden anterior realiza una serie de peticiones a la dirección ip indicada con puerto 80. Esta orden llamará a la página index.html
hasta alcanzar un total de 10000 peticiones, realizando 100 peticiones por segundo. El último parámetro (timeout) indica el tiempo en segundos
que el cliente esperará al servidor. Si este tiempo se cumple, httperf nos lanza un error, indicando que la llamada habrá fallado.

**Prueba de rendimiento a la primera máquina**

*httperf --server 192.168.54.130 --port 80 --uri /index.html --rate 100 --num-conn 10000 --num-call 100 --timeout 5*


|	Test		|	Duración de la prueba |		Total errores  |    	Tasa solicitud		|	
|				|			(segundos)	    |				      	|				 				|
|:---------:|:----------------------:|:-----------------:|:---------------------:|
|		1	   |			100.024		    |			0				|			999.8				|
|		2		|  		100.01			 |			0				|			999.9				|
|		3		|  		100.045			 |			0				|			999.5				|	
|		4		|  		100.012			 |			0				|			999.9				|	
|		5		|			100.011			 |			0				|			999.9				|
|		6		|			100.011			 |			0				|			999.9				|	
|		7		|			100.016			 |			0				|			999.8				|
|		8		|			100.022			 |			0				|			999.8				|
|		9		|			100.013			 |			0				|			999.9				|
|		10		|			100.012			 |			0				|			999.9				|


**Prueba de rendimiento a la granaja web (usando balanceador nginx)**

*httperf --server 192.168.54.135 --port 80 --uri /index.html --rate 100 --num-conn 10000 --num-call 100 --timeout 5*


|	Test		|	Duración de la prueba |		Total errores  |	   Tasa solicitud		|	
|				|			(segundos)	    |				      	|				 				|
|:---------:|:----------------------:|:-----------------:|:---------------------:|
|		1	   |			104.16		    |			3219			|			655.3				|
|		2		|  		104.88			 |			3649			|			610.6				|
|		3		|  		105.033			 |			3660			|			608.3				|	
|		4		|  		104.594			 |		   3733			|			604.2				|	
|		5		|			104.542			 |			3437			|			632.7				|
|		6		|			105.039			 |			2957			|			676.7				|	
|		7		|			104.615			 |			3225			|			651.9				|
|		8		|			104.756			 |			3021			|			669.6				|
|		9		|			104.578			 |			2657			|			704.2				|
|		10		|			104.228			 |			3506			|			628.2				|


**Prueba de rendimiento a la granja web (usando balanceador haproxy)**

*httperf --server 192.168.54.135 --port 80 --uri /index.html --rate 100 --num-conn 10000 --num-call 100 --timeout 5*

|	Test		|	Duración de la prueba |		Total errores 	|	   Tasa solicitud		|	
|				|			(segundos)	    |				      	|				 				|
|:---------:|:----------------------:|:-----------------:|:---------------------:|
|		1	   |			100.054	       |			0				|			999.5				|
|		2		|  		100.076			 |			0				|			999.2				|
|		3		|  		100.065			 |			0				|			999.3				|	
|		4		|  		100.059			 |		   0				|			999.4				|	
|		5		|			100.043			 |			0				|			999.6				|
|		6		|			100.046			 |			0				|			999.5				|	
|		7		|			100.08			 |			0				|			999.2				|
|		8		|			100.112			 |			0				|			998.9				|
|		9		|			100.024			 |			0				|			999.8				|
|		10		|			100.024			 |			0				|			999.5				|


![Captura Media Duración Prueba](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/HTTP-mediaduracionprueba.png)
![Captura Desviación Duración Prueba](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/HTTP-desviacionduracionprueba.png)

Si prestamos atención a la duración de la prueba en los diferentes tests que hemos realizado, podremos comprobar que la granja web usando el balanceador *nginx* es el que más tarda, mientras que *haproxy* y el servidor 1 tienen, en media, prácticamente la misma duración de la prueba.
Además, el balanceador que utiliza *nginx* es el que nos da un mayor número de errores, por lo que atendiendo al número de errores, a la tasa 
de solicitud y la duración de la prueba, el balanceador que atiende un mayor número de peticiones es *haproxy*, sin embargo, el servidor 1 resuelve un mayor número de peticiones que *nginx*.

![Captura Media tasa solicitud](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/HTPP-mediatasasolicitud.png)
![Captura Desviación tasa solicitud](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/HTTP-desviaciontasasolicitud.png)

##OpenWebLoad

Por último, instalaremos la herramienta **OpenWebLoad** y ejecutaremos las mismas pruebas que en los apartados anteriores. Para
instalar esta herramienta haremos lo siguiente:

*sudo apt-get install openload*

Una vez instalada la herramienta, la ejecutaremos escribiendo la siguiente orden (para la máquina 1):

*openload 192.168.54.130/index.html 10*

Esta herramienta es más sencilla de utilizar, solo recibe dos parámetros:

URL de la página del servidor a la que queremos realizar el test.
El número de clientes que se simularán, en nuestro caso, 10 clientes.

**Prueba de rendimiento a la primera máquina**

*openload 192.168.54.130/index.html 10*

| Test		| Peticiones completadas|Promedio tiempo resp|
|				|		(segundos)			|							|
|:---------:|:---------------------:|:------------------:|
|		1		|			1152.04			|		0.008				|
|		2		|			1164.86			|		0.008				|
|		3		|			1184.03			|		0.008				|
|		4		|			1117.95			|		0.008				|
|		5		|			1207.57			|		0.008				|
|		6		|			1196.88			|		0.008				|
|		7		|			1217.54			|		0.008				|
|		8		|			1204.75			|		0.008				|
|		9		|			1147.63			|		0.009				|
|		10		|			1141.5			|		0.008				|


**Prueba de rendimiento a la granja web (con el balanceador nginx):**

*openload 192.168.54.135/index.html 10*

| Test		| Peticiones completadas|Promedio tiempo resp|
|				|		(segundos)			|							|
|:---------:|:---------------------:|:------------------:|
|		1		|			496.45			|		0.002				|
|		2		|			213.34			|		0.046				|
|		3		|			399.35			|		0.024				|
|		4		|			380.34			|		0.025				|
|		5		|			348.52			|		0.028				|
|		6		|			379.72			|		0.026				|
|		7		|			489.78			|		0.02				|
|		8		|			515.63			|		0.019				|
|		9		|			537.83			|		0.018				|
|		10		|			541.38			|		0.018				|

**Prueba de rendimiento a la granja web (con el balanceador haproxy):**

*openload 192.168.54.135/index.html 10*

| Test		| Peticiones completadas|Promedio tiempo rep |
|				|		(segundos)			|							|
|:---------:|:---------------------:|:------------------:|
|		1		|			413.67			|		0.023				|
|		2		|			556.01			|		0.017				|
|		3		|			563.96			|		0.018				|
|		4		|			529.63			|		0.019				|
|		5		|			538.69			|		0.018				|
|		6		|			372.91			|		0.026				|
|		7		|			540.48			|		0.018				|
|		8		|			566.91			|		0.017				|
|		9		|			576.54			|		0.017				|
|		10		|			285.92			|		0.035

![Captura Media peticiones completadas](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/OPEN-mediatotaltps.png)
![Captura Desviación peticiones completadas](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/OPEN-desviaciontotaltps.png)

Al igual que en el primer caso, el servidor 1 es el que ofrece mejores resultados, frente a la granja web utilizando los balanceadores
*nginx* y *haproxy*. En media, es el servidor 1 quien resuelve un mayor número de peticiones, seguido de *haproxy* y *nginx*. En cuanto a la duración de la prueba, en media, el servidor 1 vuelve a superar a las otras dos herramientas, siendo el más rápido.

Por último, el menor valor en la desviación de las peticiones completadas, vuelve a ser para el servidor 1, con lo que podemos corroborar que el servidor 1 está ofrenciendo mejores resultados que *haproxy* y *nginx*. 

![Captura Media tiempo respuesta](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/OPEN-mediatiempores.png)
![Captura Desviación tiempo respuesta](https://github.com/ramon-rd/SWAP/blob/master/Practicas/Pr%C3%A1ctica%204/imagenes/OPEN-desviaciontiempores.png)

