<div align="center">

# LABORATORIO DHCP STARVATION - JORGE LÓPEZ ARCOS


<img width="555" height="383" alt="dhcp starvation logo" src="https://github.com/user-attachments/assets/93ca7d5a-0f8f-4b18-8034-898ceaa9a883" />

</div>
<br>
<br>

## DESCRIPCIÓN DEL ATAQUE

El ataque DHCP Starvation es un ataque de red que consiste en agotar el pool de direcciones que puede asignar el servidor DHCP. El protocolo DHCP utiliza el modelo cliente/servidor que usa el puerto UDP 67 en el servidor y UDP 68 en el cliente.  

Este ataque funciona en inundar el servidor DHCP con muchas solicitudes DHCP Discover desde un cliente malicioso. Utilizamos diferentes direcciones MAC para cada solicitud.  

El propósito que tiene este ataque es denegar el servicio a los clientes para que no puedan conectarse a la red.  

También, una vez el pool de direcciones está agotado, el atacante puede emitir direcciones IP correctas a los clientes.

<br>
<br>

## HERRAMIENTAS UTLIZADAS PARA LA REALIZACIÓN DEL PROYECTO


Para la realización de este ataque van a ser necesarias las siguientes herramientas: 

 - Máquina Atacante: Usaré el sistema operativo Ubuntu en Adaptador Puente en el que se haré uso de la herramienta Yersinia. Ha sido imposible utilizarlo con Kali por problemas de depndencias y versiones.  

 - Servidor DHCP Legítimo: En este servidor DHCP tendré configurado un pequeño pool de direcciones. El sistema operativo que usaré será un Ubuntu Server en Adaptador Puente. 

 - Máquina Víctima: Con esta máquina comprobaré que no podré obtener IP debido al ataque DHCP Starvation. Usaré el sistema operativo de Windows en Adaptador Puente.

 - Yersina: Esta herramienta es un framework de pentesting que explota vulnerabilidades en protocolos de red como DHCP, STP, etc...


<br>
<br> 


## DESARROLLO DEL PROYECTO

- Primero descargo el servidor DHCP en mi Ubuntu Server.

 ```bash
# Actualizo sistema e instalo el servidor DHCP
 sudo apt update

 sudo apt install isc-dhcp-server -y

```

<img width="542" height="147" alt="instalardhcp" src="https://github.com/user-attachments/assets/ae5bc1c9-15a2-49d2-b5e3-2be08bf1e513" />


<br> 
<br>

- Ahora procedo a configurar el Servidor DHCP Legítimo. El archivo de configuracion es **/etc/dhcp/dhcpd.conf**:

 ```bash
# Abro el fichero de configuración principal
 sudo nano /etc/dhcp/dhcpd.conf

```
<img width="496" height="199" alt="configuraciondhcp" src="https://github.com/user-attachments/assets/27b88ae8-08a8-4ee5-b210-3e755e1aa862" />


<br> 
<br>

- A continuación, procedo a indicar la interfaz que voy a utlizar en el fichero **/etc/default/isc-dhcp-server**:


 ```bash
# Abro el fichero de configuración de las interfaces
 sudo nano /etc/default/isc-dhcp-server

```

<img width="619" height="151" alt="interfazserver" src="https://github.com/user-attachments/assets/713fe665-06b1-4132-9807-7955d286d5a9" />

<br> 
<br>

- Una vez terminado de configurar el servidor DHCP, procedo a configurar el atacante que será en mi máquina de Ubuntu. Primero, instalaré la herramienta de Yersinia que sirve para hacer pruebas de penetración controladas, sobre todo en redes locales.


 ```bash
# Instalo la herramienta Yersinia

 sudo apt update

 sudo apt install yersinia

```

<img width="579" height="138" alt="sifunciona" src="https://github.com/user-attachments/assets/7dec5c10-308a-4e55-8ffb-089275cd75b9" />


<br>
<br>


- Ahora procedo a abrir el modo gráfico de Yersinia mediante el siguiente comando:


 ```bash
# Abrimos Yersinia
 sudo yersinia -G

```

<img width="1091" height="141" alt="abriryersinia" src="https://github.com/user-attachments/assets/19efadd7-ab32-457e-8568-010c06563c04" />


<br>
<br>

- Ahora procedo a seleccionar la interfaz en la que realizaremos el ataque. En mi caso es enp0s3.


<img width="441" height="266" alt="interfazyersinia" src="https://github.com/user-attachments/assets/7331b1f7-a479-489d-bd75-c79c1acc2436" />


<br>
<br> 

- A continuación, selecciono el tipo de ataque que voy a producir. En mi caso elegiré el Sending Discover Packet, indicando que es un ataque de denegación de servicios ya que dejaré sin poder conectarse a la red a la víctima mandando paquetes de forma constante hasta saturarlo.

<img width="532" height="378" alt="elegirataque" src="https://github.com/user-attachments/assets/27f5544c-0c0e-4db2-a86a-345a5604db0f" />


<br>
<br> 

- Después de iniciar el ataque, compruebo en Wireshark el tráfico inundado del servidor debido a las peticiones de DHCP Discover.


<img width="1095" height="322" alt="wiresharkcomprobacion" src="https://github.com/user-attachments/assets/2e922357-de5b-48fc-9acb-5841a0095c6a" />

<br>
<br>

- Además, también vemos desde Yersinia como se están enviando los paquetes que inundarán el servidor imposibilitando de entregar IPs a los clientes debido a que se ha agotado el pool mandando direcciones MAC falsas.


<img width="573" height="284" alt="yersiniacomprobacion" src="https://github.com/user-attachments/assets/9688693e-e6cb-4fdf-aefb-652f7c2ccd9f" />


<br>
<br>

- Ahora en el servidor DHCP enseño como muestra que no puede entregar direcciones IP.


<img width="1001" height="270" alt="correcto" src="https://github.com/user-attachments/assets/9f39727b-a918-4f01-bb40-5c071a4cd08d" />
