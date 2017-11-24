### Examen 3
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Tema:** Descubrimiento de servicios, Microservicios  
**Correo:** daniel.barragan at correo.icesi.edu.co

**Nombre:** Julián David González Jiménez   
**Código:** A00315288   
**URL:** https://github.com/juliand7gj/so-exam3-1/tree/A00315288/Parcial3/A00315288  

### Objetivos
* Implementar servicios web que puedan ser consumidos por usuarios o aplicaciones
* Conocer y emplear tecnologías de descubrimiento de servicio

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7
* Framework consul, zookeper o etcd

### Descripción
El tercer parcial del curso sistemas operativos trata sobre la creación de servicios web y el uso de tecnologías para el descubrimiento de servicio

![][1]
**Figura 1.** Despliegue básico de microservicios

### Actividades
1. Incluir nombre, código (5%)
2. Ortografía y redacción cuando sea necesario (5%)
3. Despliegue un esquema como el mostrado en la **figura 1**. Empleen un servicio web de su preferencia (puede usar alguno de los ejemplos de clase). No es necesario incluir los componentes para monitoreo (Elasticsearch, Kibana, Logstash) (30%)
4. Adicione un microservicio igual al ya desplegado. Muestre a través de evidencias como las peticiones realizadas al balanceador son dirigidas a la replica del microservicio (30%)
5. Describa los cambios o adiciones necesarias en el diagrama de la **figura 1** para adicionar un microservicio diferente al ya desplegado en el ambiente, tenga en cuenta los siguientes conceptos en su descripción: API Gateway, paradigma reactivo, load balancer, protocolo publicador/suscriptor (interconexión de microservicios) (20%)
6. El informe debe ser entregado en formato pdf a través del moodle y el informe en formato README.md debe ser subido a un repositorio de github. El repositorio de github debe ser un fork de https://github.com/ICESI-Training/so-exam3 y para la entrega deberá hacer un Pull Request (PR) respetando la estructura definida. El código fuente y la url de github deben incluirse en el informe (10%)  

### Desarrollo

3. 

**Discovery service:** Los requerimientos para realizar el montaje del discovery service, son: instalar todas las depencias, instalar consul y ejecutar consul en modo consul server. Los clientes se unen al consul server, por esto, se ven agrupados allí. Es necesario habilitar los puertos para que el consul server funcione. 

![][2]  
**Figura 2.** Consul agent en modo servidor  


![][3]  
**Figura 3.** Consul agent en ejecución  


![][4]  
**Figura 4.** Consul members existentes  


![][5]  
**Figura 5.** Curl balancer  


**Balanceador de carga:** Cada cliente se comunica con el balanceador de carga, este se encarga de registrar los servidores que disponen de cierto servicio y atender las solicitudes de los clientes mediante estos servidores. Los requerimientos para realizar el montaje del balanceador, son: HAProxy y consul-template. Posteriormente, se configura el archivo "/etc/haproxy/haproxy.cfg", aquí se van a configurar los servidores y sus respectivas IP_Addres, esto va a permitir realizar las funciones del balanceador. Para que este proceso se haga de forma automatica se configura el consul-template en el archivo de configuración "/etc/consul-template/haproxy.tpl". 

![][6]  
**Figura 6.** Configuración del balanceador  


![][7]  
**Figura 7.** Balanceador corriendo  


![][8]  
**Figura 8.** Configuración de Consul-Templates  


![][9]  
**Figura 9.** Actualización dinámica del balanceador 


![][11]  
**Figura 10.** HAProxy actualizado    


**Servidores:** Primero se debe instalar los siguiente requerimientos: programar el servicio, iniciar el agente consul, abrir los puertos en el firewall requeridos para acceder al microservicio y los puertos en el firewall del agente consul. Posteriormente, se crea el ambiente a ejecutar, se instala el flask en ese ambiente. Por último se agrega el servidor a consul members.

![][12]  
**Figura 11.** Código del microservicio  


![][13]  
**Figura 12.** Servicio en ejecución  


![][14]  
**Figura 13.** Prueba del microservicio  


Cuando un nuevo servidor es agregado al discovery service, mediante los consul-templates, el balanceador de carga es capaz de escribir automaticamente sobre su archivo de configuraciones y agregar este nuevo servidor a su lista de servidores disponibles.

4. En el punto anterior, se pudo observar que se tienen varios microservicios. El balanceador utiliza el método de balanceo roundrobin, que consiste en que cuando se realiza una solicitud a un servidor, la siguiente solicitud será para el servidor siguiente, así sucesivamente.

![][10]  
**Figura 14.** Peticiones realizadas al balanceador dirigidas a la replica del microservicio  


5. API Gateway se útiliza para no tener que codificar un mismo servicio en cada servidor que exista, ya que esté contiene todos los códigos, así los servidores acceden y adquieren el API del microservicio. Es decir, si se tienen varios servidores que van a conterner el mismo servicio, no se tiene que configarar dicho servicio en cada servidor, sino que, si el servicio está en API Gateway, los servidores pueden adquirirlo de aquí. El balanceador de carga debe ser lo suficientemente bueno cuando se ejecute un servicio con servidores detrás. Por ende, se necesida un nuevo balanceador de carga para soportar el nuevo servicio. El protocolo publicador/suscriptor (interconexión de microservicios) es muy parecido al discory service. 

Lo que se debe realizar es: implementar el servicio en donde ser desee, aquí se puede usar el API Gateway, después se debe registrar el servicio en el consul server, y por último, se creará el nuevo balanceador. 


### Referencias
https://github.com/ICESI/so-microservices-python  
http://microservices.io/patterns/microservices.html  
https://www.upcloud.com/support/haproxy-load-balancer-centos/  
https://github.com/ICESI/so-discovery-service/blob/master/README.md  
http://sirile.github.io/2015/05/18/using-haproxy-and-consul-for-dynamic-service-discovery-on-docker.html  
https://es.wikipedia.org/wiki/Balanceador_de_carga  

[1]: Microservices_Deployment.png
[2]: consul_agent_server.PNG
[3]: consul_logs.PNG
[4]: consul_members.PNG
[5]: curl_balancer.PNG
[6]: configuracionBalanceador.png
[7]: BalanceadorCorriendo.png
[8]: configuracionConsulTemplates.png
[9]: ActualzacionDinamicaBalanceador.png
[10]: DemostracionFuncionBalanceador.png
[11]: HAProxyActualizado.png
[12]: parcial3a.PNG
[13]: parcial3d.PNG
[14]: parcial3e.PNG
