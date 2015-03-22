##Ejercicio 1

Inicialmente teníamos los siguientes datos:

|Component	|	Availability|
|:---------:|:------------:|
|Web			|	85%			|
|Aplication |  90%			|
|DataBase   |  99.9%			|
|DNS			|  98%			|
|Firewall	|	85%			|
|Switch		|	99%			|
|DataCenter	|	99.99%		|
|ISP			|	95%			|

Después de realizar una primera réplica sobre todo el sistema, hemos obtenido:

|Component	|	Availability|
|:---------:|:------------:|
|Web			|	97.75%		|
|Aplication |  99%			|
|DataBase   |  99.9999%		|
|DNS			|  99.96%		|
|Firewall	|	97.75%		|
|Switch		|	99.99%		|
|DataCenter	|	99.99%		|
|ISP			|	99.75%		|


#Si realizamos una segunda réplica al sistema tenemos que:


disponibilidad_web3: 97.75%+(1-97.75%)*85% = 99.6625%

disponibilidad_app3: 99%+(1-99%)*90% = 99.9%

disponibilidad_db3: 99.9999%+(1-99.9999%)*99.9% = 99.9999% 

disponibilidad_dns3: 99.96%+(1-99.96%)*98% = 99.9992%

disponibilidad_firewall3: 97.75%+(1-97.75%)*85% = 99.6625%

disponibilidad_switch3: 99.99%+(1-99.99%)*99% = 99.9999%

disponibilidad_datacenter3: 99.99%+(1-99.99%)*99.99% = 99.9999%

disponibilidad_isp3: 99.75%+(1-99.75%)*95% = 99.6625%

Por lo que, la disponibilidad del servidor con dos réplicas de cada elemento es:

As = 99.6625% * 99.9% * 99.9999% * 99.9992% * 99.6625% * 99.9999% * 99.9999% 99.6625% = **98.89%**

##Ejercicio 2

#Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad.

*Kendo UI:* es una librería de **javascript** para el desarrollo de aplicaciones web
enriquecidas del lado del cliente.

*AngularJS:* es un framework de **javascript**, que permite crear aplicaciones SPA.

*CppCMS:* es un framework de **C++** cuyo objetivo es el de construir aplicaciones web
de alto rendimiento.

*Grails:* framework de **java** que ha sido diseñado para desarrollar aplicaciones web. Intenta
establecer un equilibrio entre consistencia y funcionalidad.

*Mason:* framework de **perl** para hacer aplicaciones web que sirvan contenido dinámico.

*Agavi:* framework de **php** que permite realizar aplicaciones web. Permite una alta escalabilidad.

##Ejercicio 3

#¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor? Buscar herramientas y aprender a usarlas.

Herramientas de prueba de carga HTTP:

**ApacheBench.**

**Siege:** herramienta utilizada por desarrolladores para realización de pruebas de carga.

**Nsasoft Network Traffic Emulator:** puede generar tráfico IP/ICMP/TCP/UDP. Aunque su uso más
indicado es para routers y firewalls, también puede ser usado para testear servidores.

**WebServer Stress Tool:** es una herramienta para simular altas cargas de trabajo en un servidor web.

**Hammerhead2:** es una herramienta de prueba de carga que puede generar múltiples conexiones y usuarios
a una hora programada.

**Hammerora:** es una herramienta de generación de carga para la base de datos de Oracle.

**JMeter:** con esta herramienta podemos realizar pruebas de carga en los servidores de correo.


##Ejercicio 4

#Buscar ejemplos de balanceadores software y hardware (productos comerciales).

**Balanceadores hardware**

*LM-2600:* es un balanceador de carga diseñado para cargas de trabajo en crecimiento.

*F5 Networks Big/IP:* es uno de los productos de balanceo de carga más conocidos del mercado.

**Balanceadores software:**

*Kemp:* permite balanceo de carga virtual, hardware y nube.

*HAProxy:* software libre que actúa como balanceador de carga ofreciendo alta disponibilidad y balanceo
de carga para comunicaciones TCP y HTTP.

#Buscar productos comerciales para servidores de aplicaciones.

BEA WebLogic

IBM WebSphere

Sun-Netscape IPlanet

Sun One

Oracle IAS

Borland AppServer

HP Bluestone

#Buscar productos comerciales para servidores de almacenamiento.

ThinkServer RS140 - lenovo

Qnap Servidor Nas TS-453 Pro

Servidores PowerEdge - dell

IBM System Storage 

##Bibliografía

http://en.wikipedia.org/wiki/Comparison_of_web_application_frameworks

http://codehero.co/como-hacer-pruebas-de-carga-servidores-web/

http://www.pymesyautonomos.com/tecnologia/en-los-servidores-debemos-provocar-estres

http://www.gurudelainformatica.es/2008/04/stress-test-servicios-de-red.html

http://unadocenade.com/una-docena-de-herramientas-para-probar-y-mejorar-el-rendimiento-de-nuestra-web/

http://kemptechnologies.com/es/

https://unpocodejava.wordpress.com/2013/07/03/que-es-un-balanceador-de-carga-que-es-haproxy/

http://www.ecured.cu/index.php/Servidor_de_Aplicaciones

http://shop.lenovo.com/es/es/servers/

http://www.macnificos.com/product.aspx?p=9528&gclid=CLjkwvr6vMQCFYLItAodHl8ARA

http://www-03.ibm.com/systems/es/storage/
