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

 - Máquina Atacante: Usará el sistema operativo Kali en Adaptador Puente en el que se haremos uso de la herramienta Yersinia.  

 - Servidor DHCP Legítimo: Este servidor DHCP tendrá configurado un pequeño pool de direcciones. El sistema operativo que usará será un Ubuntu Server en Adaptador Puente. 

 - Máquina Víctima: Con esta máquina se comprobará que no podrá obtener IP debido al ataque DHCP Starvation. Usará el sistema operativo de Windows en Adaptador Puente.

 - Yersina: Esta herramienta es un framework de pentesting que explota vulnerabilidades en protocolos de red como DHCP, STP, etc...


<br>
<br> 


## DESARROLLO DEL PROYECTO

- Primero descargamos el servidor DHCP en mi Ubuntu Server.

 ```bash
# Actualizar sistema e instalamos el servidor DHCP
 sudo apt update

 sudo apt install isc-dhcp-server -y

```

<img width="542" height="147" alt="instalardhcp" src="https://github.com/user-attachments/assets/ae5bc1c9-15a2-49d2-b5e3-2be08bf1e513" />


<br> 
<br>

- Ahora procedemos a configurar el Servidor DHCP Legítimo. El archivo de configuracion es **/etc/dhcp/dhcpd.conf**:

 ```bash
# Abrimos el fichero de configuración
 sudo nano /etc/dhcp/dhcpd.conf

```
<img width="496" height="199" alt="configuraciondhcp" src="https://github.com/user-attachments/assets/27b88ae8-08a8-4ee5-b210-3e755e1aa862" />


<br> 
<br>






