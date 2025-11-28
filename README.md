<div align="center">

# LABORATORIO DHCP STARVATION - JORGE LÓPEZ ARCOS


<img width="555" height="383" alt="dhcp starvation logo" src="https://github.com/user-attachments/assets/93ca7d5a-0f8f-4b18-8034-898ceaa9a883" />

</div>

## DESCRIPCIÓN DEL ATAQUE

El ataque DHCP Starvation es un ataque de red que consiste en agotar el pool de direcciones que puede asignar el servidor DHCP. El protocolo DHCP utiliza el modelo cliente/servidor que usa el puerto UDP 67 en el servidor y UDP 68 en el cliente.  

Este ataque funciona en inundar el servidor DHCP con muchas solicitudes DHCP Discover desde un cliente malicioso. Utilizamos diferentes direcciones MAC para cada solicitud.  

El propósito que tiene este ataque es denegar el servicio a los clientes para que no puedan conectarse a la red.  

También, una vez el pool de direcciones está agotado, el atacante puede emitir direcciones IP correctas a los clientes.


HERRAMIENTAS UTLIZADAS PARA LA REALIZACIÓN DEL PROYECTO


Para la realización de este ataque van a ser necesarias las siguientes herramientas:  

    Voy a utilizar un total de 3 máquinas virtuales con Red Interna:  

            Máquina Atacante: Usará el sistema operativo Kali en el que se usará Yersinia.  

            Servidor DHCP Legítimo: Este servidor DHCP tendrá configurado un pequeño pool de direcciones. El sistema operativo que usará será un Ubuntu Server. 

            Máquina Víctima: Con esta máquina se comprobará que no podrá obtener IP debido al ataque DHCP Starvation. Usará el sistema operativo de Windows.  
