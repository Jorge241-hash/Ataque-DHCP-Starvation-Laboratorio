<div align="center">

# LABORATORIO DHCP STARVATION - JORGE LÓPEZ ARCOS


<img width="555" height="383" alt="dhcp starvation logo" src="https://github.com/user-attachments/assets/93ca7d5a-0f8f-4b18-8034-898ceaa9a883" />

</div>
<br>
<br>

 
## DESCRIPCIÓN DEL ATAQUE

El ataque DHCP Starvation es un ataque de red que consiste en agotar el pool de direcciones que puede asignar el servidor DHCP. El protocolo DHCP utiliza el modelo cliente/servidor que usa el puerto UDP 67 en el servidor y UDP 68 en el cliente.  

Este ataque tiene el fin de inundar el servidor DHCP con cientas de solicitudes DHCP Discover desde un cliente malicioso. Además, utilizamos diferentes direcciones MAC para cada solicitud.  

El propósito que tiene este ataque es denegar el servicio a los clientes para que no puedan conectarse a la red.  

También, a parte de dejar sin IP a la víctima, el atacante puede utilizar un servidor DHCP ilegítimo con el objetivo de emitir direcciones IP correctas a las víctimas.

<br>
<br>

## HERRAMIENTAS UTILIZADAS PARA LA REALIZACIÓN DEL PROYECTO


Para la realización de este ataque van a ser necesarias las siguientes herramientas: 

 - Máquina Atacante: Usaré Kali Linux en Red Interna en el que se haré uso de la herramienta Yersinia.

 - Servidor DHCP Legítimo: En este servidor DHCP tendré configurado un pequeño pool de direcciones. El sistema operativo que usaré será un Ubuntu Server en Red Interna. 

 - Máquina Víctima: Con esta máquina comprobaré que no podré obtener IP debido al ataque DHCP Starvation. Usaré el sistema operativo de Windows en Red Interna.

 - Switch: Utilizaré una máquina Ubuntu que funcionará como un switch para mitigar el ataque DHCP Starvation. Usaré tres interfaces en red interna.

 - Yersina: Esta herramienta es un framework de pentesting que explota vulnerabilidades en protocolos de red como DHCP, STP, etc...


<br>
<br> 


## DESARROLLO DEL PROYECTO

- Primero descargo las dependencias y actualizamos.

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

<img width="505" height="198" alt="serverdhcpbueno" src="https://github.com/user-attachments/assets/7929f360-b1a9-478c-808b-678e5f64f242" />

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


- Ahora procedo mediante el modo interactivo de Yersinia procedo a realizar el ataque. El comando que he usado para abrir el yersinia es el siguiente:


 ```bash
# Abrimos Yersinia
 sudo yersinia -I

```

<img width="349" height="160" alt="yersiniadeverdad" src="https://github.com/user-attachments/assets/217c030e-d983-4fef-a197-af2c747ac284" />

<br>
<br>

- Antes de empezar el ataque, procedo en la víctima a eliminar la asignación de la IP mediante el siguiente comando

 ```bash
# Eliminamos la asignación de la IP
 Ipconfig /release

```

<img width="461" height="298" alt="release1" src="https://github.com/user-attachments/assets/3dc27343-f3ac-4a8b-a95b-5e2e51070568" />

<br>
<br> 

- A continuación, deshabilito el adaptador de red de la víctima.

<img width="501" height="376" alt="adaptadordesactivado1" src="https://github.com/user-attachments/assets/2e89c5cc-6cd2-4d18-88cc-ad3e26ed51eb" />


<br>
<br> 


- Después de deshabilitar el adaptador en la víctima, procedo a iniciar el ataque. Una vez dentro del modo interactivo de yersinia pulso F2 para activar el modo DHCP y después con la tecla "X" nos deja elegir el tipo de ataque DHCP que en mi caso es el 1.

<img width="1075" height="400" alt="yersiniapanel" src="https://github.com/user-attachments/assets/ed1a5bcd-4b93-45d2-be8d-9939b2dddc6a" />


<br>
<br>

- Una vez iniciado se muestra el envío de mensajes Discover que tiene como objetivo saturar el servidor DHCP mandando cientas de direcciones MAC para agotar el pool de direcciones del servidor DHCP y que la víctima no tenga IP.

<img width="1139" height="244" alt="yersiniaempieza" src="https://github.com/user-attachments/assets/0f1f3baf-0e55-4db0-aca0-a9c7cb2dce2d" />

<br>
<br>


- Además, en Wireshark podemos ver el tráfico de paquetes Discover que se ha generado con Yersinia.

<img width="1095" height="322" alt="wiresharkcomprobacion" src="https://github.com/user-attachments/assets/20484baf-4ffb-4ef4-aad5-43a42383972b" />

<br>
<br>

- Ahora en el servidor DHCP enseño como muestra que no quedan leases.

<img width="1003" height="415" alt="noquedanleases" src="https://github.com/user-attachments/assets/05c09dbc-361e-4a6e-82fb-9cf5beddabf7" />

<br>
<br>


- A continuación, vuelvo a activar el adaptador de red de la víctima.

<img width="495" height="220" alt="activaradaptador1" src="https://github.com/user-attachments/assets/fe98ead2-0f8f-4268-ab4a-0d8a7f3887b7" />

<br>
<br>


- Después de habilitar el adaptador, procedo a pedirle al servidor DHCP que le asigne una IP pero como está saturado nunca podrá asignarle una IP.

 ```bash
# Pedimos al servidor DHCP que me asigne una IP

 Ipconfig /release

```

<img width="750" height="137" alt="elrenew" src="https://github.com/user-attachments/assets/932e16d6-5c04-433d-be22-cbd848ba7964" />

<br>
<br>

- Además, en el visor de eventos muestra un aviso de que no se ha podido contactar con el servidor DHCP.


<img width="631" height="498" alt="visordeeventos" src="https://github.com/user-attachments/assets/e093de3b-419b-49f3-9685-151ef2c04c7b" />

<br>
<br>


- Ahora vuelvo a desactivar el adaptador y paro el ataque para que la víctima pueda tener IP.

<img width="306" height="146" alt="pararataque" src="https://github.com/user-attachments/assets/ee8192d0-b13b-4b43-b88f-a1afa592cf28" />

<br>
<br>


- Una vez parado el ataque, vuelvo a activar el adaptador y pido de nuevo al servidor DHCP que me asigne una IP que en este caso me la dará.

<img width="597" height="230" alt="renewcorrecto" src="https://github.com/user-attachments/assets/8d1f0b5d-a274-47e3-aad0-d046f2828207" />

<br>
<br>


## MITIGACIÓN DEL ATAQUE MEDIANTE LA HERRAMIENTA OVS

A continuación, procederé a explicaré la mitigación del ataque DHCP Starvation mediante la herramienta OVS


- Antes de todo, instalamos la herramienta de OVS mediante el siguiente comando.

 ```bash
# Instalación de OVS

 sudo apt install -y openvswitch-switch

```

<img width="616" height="146" alt="instalacion" src="https://github.com/user-attachments/assets/8b425523-0d7e-4b7b-a7b3-1c80c141efb3" />

<br>
<br>


- A continuación, procedo a configurar OVS. Primero, creo el Bridge OVS y después añado las tres interfaces al Bridge.

 ```bash
# Creación del bridge

 sudo ovs-vsctl add-br br0

# Añadir interfaces al bridge

 sudo ovs-vsctl add-port br0 enp0s3
 sudo ovs-vsctl add-port br0 enp0s8
 sudo ovs-vsctl add-port br0 enp0s9

```

<img width="503" height="210" alt="mitigacion1" src="https://github.com/user-attachments/assets/506df5eb-e7b4-452a-ac82-67a3d93f5b08" />

<br>
<br>

- Ahora procedo a realizar generar tráfico desde la víctima mediante los siguientes comandos:

```bash
 # Generamos tráfico

   Ipconfig /release

   Ipconfig /renew

```

<img width="564" height="424" alt="releaseyrenew" src="https://github.com/user-attachments/assets/dca33c04-1a3d-4470-b5f0-cd6b53667e35" />


<br>
<br>


- A continuación, ejecutamos el siguiente comando que nos muestra que ha aprendido la dirección MAC de la víctima.

```bash
# Vemos que ha aprendido la MAC de la víctima

 sudo ovs-appctl fdb/show br0

```

<img width="444" height="226" alt="vermac" src="https://github.com/user-attachments/assets/8054476f-834c-465b-a975-6a75666aaaf1" />

<br>
<br>


- Por último, ejecutamos el siguiente comando que no evitará que se agote el pool pero protege el puerto de la víctima limitando la MAC.

```bash
# Protegemos el puerto de la víctima limitando la MAC

 sudo ovs-vsctl set port enp0s8 other-config:port-security=08:00:27:c3:01:67

```

<img width="863" height="114" alt="mitigacion2" src="https://github.com/user-attachments/assets/6e5b4f49-5619-4d97-bc2f-6829297fae41" />


<br>
<br>

## CONCLUSIONES 

 
En conclusión, me ha parecido un proyecto muy interesado donde he aprendido a realizar un ataque en red como es el DHCP Starvation. Además, he aprendido a usar la herramienta de Yersinia que sirve para poder realizar diferentes ataques STP, Rogue DHCP, MAC Spoofing, etc... Recomiendo utilizar estas herramientas porque sirven para comprender el funcionamiento de muchos ataques. 

<br>
<br>

## BIBLIOGRAFÍA

A continuación, muestro los siguientes enlaces a videos y tutoriales que me ha servido para realizar este proyecto: 

- https://youtu.be/jyo_3JForbA?si=uG4n1hGbBudUiqFz

- https://blog.tiraquelibras.com/?p=491

- https://abcxperts.com/que-es-un-ataque-dhcp-starvation/?srsltid=AfmBOoqokXnVu9ZOV7On0zBjshL6TrpGST1Gl9mqrqM783BjxYsXQdaW





