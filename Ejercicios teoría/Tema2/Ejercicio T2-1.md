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

#Buscar frameworks y librerías para diferentes lenguajes que permitan hacer
#aplicaciones altamente disponibles con relativa facilidad.

*Kendo UI:* es una librería de **javascript** para el desarrollo de aplicaciones web
enriquecidas del lado del cliente.

*AngularJS:* es un framework de **javascript**, que permite crear aplicaciones SPA.

*CppCMS:* es un framework de **C++** cuyo objetivo es el de construir aplicaciones web
de alto rendimiento.

*Grails:* framework de **java** que ha sido diseñado para desarrollar aplicaciones web. Intenta
establecer un equilibrio entre consistencia y funcionalidad.

*Mason:* framework de **perl** para hacer aplicaciones web que sirvan contenido dinámico.

*Agavi:* framework de **php** que permite realizar aplicaciones web. Permite una alta escalabilidad.
