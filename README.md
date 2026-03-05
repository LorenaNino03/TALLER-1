# TALLER-1

# Primer Punto Instalando y entendiendo el alcance de Docker.

## ¿Qué ven?
docker run --rm -it wernight/funbox cvlc --no-audio
-V caca /examples/countdown.mp4
Aquí pudimos observar que para que el comando funcione y veas la animación, necesitas un servidor X en Windows por lo que se requiere instalar el WSL y así poder ver la cuenta regresiva, se instala el WSL y se observo. 
<img width="1324" height="616" alt="Captura de pantalla 2026-03-05 100323" src="https://github.com/user-attachments/assets/9fe1b750-77e2-4865-a2d3-c8ead9760a95" />

# Docker + ROS + Gazebo: Simulación de un Carrito Seguidor de Línea
## Paso 1 Descargar imagen de ROS y GAZEBO
<img width="1064" height="327" alt="Captura de pantalla 2026-03-05 100412" src="https://github.com/user-attachments/assets/b59ed105-5dcb-402b-9e87-327d18d0da26" />

## Paso 2 Crear Dockerfile (se debe llamar Dockerfile-ros)
<img width="1364" height="636" alt="image" src="https://github.com/user-attachments/assets/8f8b5f59-5f40-4ca0-8f53-69943eb524b9" />

## Paso 3 Construir y ejecutar (Se recomienda correr todo a la vez)
<img width="996" height="121" alt="image" src="https://github.com/user-attachments/assets/e4754a8a-d35b-4b61-8061-bbc18e469357" />
Se puso el comando para construir la imagen.

## Paso 4 Se debe abrir otra terminal para correr y ejecutar el robot
Se abre la ventana del robot
<img width="1910" height="983" alt="Captura de pantalla 2026-03-05 124908" src="https://github.com/user-attachments/assets/5dd778e9-04e1-4def-899a-6a70134eebb2" />
<img width="1054" height="414" alt="image" src="https://github.com/user-attachments/assets/f130629e-310f-404e-9b99-4d74dbdd9235" />

# Simulación de un robot con LIDAR y SLAM en Docker.

# Paso 1 Crear el DockerFile
Identificamos el contenedor 
<img width="1447" height="70" alt="image" src="https://github.com/user-attachments/assets/7337851a-564b-4d40-864d-89c683ba9753" />
Ponemos el nombre del contenedor para que así nos indique como podemos mover el robot. 
<img width="1467" height="377" alt="image" src="https://github.com/user-attachments/assets/7232b9eb-ce6c-4fa2-84f7-641df2538eb0" />

# Paso 2 Ejecutar el Sistema
En la siguiente imagen podemos ver que se movio
<img width="1306" height="827" alt="image" src="https://github.com/user-attachments/assets/e3b2a2b5-0a87-4b42-b3fc-f5066fb3375f" />

# Paso 3 Visualizar el mapa
Para visualizarlo debemos colocar el siguiente comando en una nueva interface de Ubuntu
<img width="1448" height="302" alt="image" src="https://github.com/user-attachments/assets/3259618d-58f4-4767-916b-9cf1bdf4a959" />
<img width="1617" height="819" alt="image" src="https://github.com/user-attachments/assets/d31ff4c9-b854-4aad-9098-6259e3b477c8" />
<img width="1721" height="733" alt="image" src="https://github.com/user-attachments/assets/d96fd0fc-fbe1-44da-80b4-9189ae101e86" />

# Segundo Punto Descubriendo el protocolo ARP

# Escenario Docker
Crear una red bridge personalizada:
<img width="877" height="97" alt="image" src="https://github.com/user-attachments/assets/23cd0062-f6bd-42e8-be57-217187f7307b" />
# Lanzar dos contenedores interactivos (con bash)conectados a esa red:
<img width="694" height="345" alt="image" src="https://github.com/user-attachments/assets/d4a85386-aa4b-4e72-8cc1-9da83cc186ca" />

# Obtener IP del destino contenedor 2
<img width="1003" height="39" alt="image" src="https://github.com/user-attachments/assets/3eb89135-f4aa-4328-838c-32d5ad19b072" />
# Escuchar el tráfico ARP: En una terminal aparte, ejecutamos:
<img width="800" height="271" alt="image" src="https://github.com/user-attachments/assets/cb2e8b83-b7d0-41a6-b3e9-cae1cffe1480" />

# Obtener IP del destino Contenedor 1
<img width="1011" height="36" alt="image" src="https://github.com/user-attachments/assets/82976513-e044-44fc-95d3-054343593935" />

# Desde el contenedor1, hacer ping a contenedor2: “ping –c 172.17.0.3” (usar la IP de contenedor2).
<img width="735" height="43" alt="image" src="https://github.com/user-attachments/assets/4b55c4ba-fe64-4490-95d6-2c430879cec2" />
# Iniciar la captura en Wireshark con el filtro arp.
<img width="1250" height="420" alt="image" src="https://github.com/user-attachments/assets/d29b0bcb-cac4-4394-b079-582c42ed2455" />

# Análisis y Preguntas
## ¿Quién envía la trama ARP? (Dirección MAC de origen y destino).
La envía el Contenedor 1 (el emisor del ping). En la captura se observa como un paquete . La dirección MAC de origen es la del contenedor emisor y la de destino inicial es la de difusión (broadcast: )

## ¿A qué dirección MAC de destino va dirigido?
Va dirigido a la dirección de Broadcast. Esto es necesario porque el emisor conoce la IP del destino

## Identificar el paquete de respuesta ARP: "172.17.0.3 is at <MAC_del_contenedor2>". ¿Es broadcast o unicast?
Es Unicast.
## Explicar: ¿Por qué es necesario el ARP? ¿Qué información se almacena en la caché ARP después de este intercambio?
Porque los switches y tarjetas de red trabajan en la Capa 2 (Enlace de datos) y solo entienden direcciones MAC. El ARP traduce las IPs (Capa 3) a direcciones físicas.
Caché ARP: Después del intercambio, ambos contenedores guardan esta relación en una tabla temporal (caché) para no tener que repetir el proceso de "preguntar a todos" en el próximo envío.

## Tercer Punto Midiendo latencia y jitter con ping
Calcular la latencia (RTT) y observar la variabilidad (jitter) en las respuestas ICMP.
<img width="999" height="438" alt="image" src="https://github.com/user-attachments/assets/69b8f53b-c3e2-4470-b7d6-0cc4def0ea5a" />
# Escenario Docker
Procedimiento
# Sin perturbación: Inicia la captura en Wireshark. Desde contenedor1, hacer ping a contenedor2 con 10 paquetes: ping -c 10 172.17.0.3. Detener la captura.
<img width="999" height="438" alt="image" src="https://github.com/user-attachments/assets/dc7028ce-96a6-49c8-a305-f066cb931e50" />
RTT Mínimo: 0,093 ms
RTT Promedio (promedio): 0,370 ms
RTT Máximo: 3.496 ms
Jitter (mdev): 0,836 ms
# Analisis
# ¿Qué es la latencia?
Es el tiempo exacto que tarda un paquete de datos en viajar desde el origen al destino y regresar

# ¿Qué factores pueden causarla?
La distancia física entre nodos, la congestión en los routers, el procesamiento de los protocolos en cada salto y, en el caso de Docker, la sobrecarga del switch virtual

# ¿Qué es el jitter y por qué es importante para aplicaciones en tiempo real (VoIP,gaming)?

VoIP (llamadas): Un jitter alto causa que la voz se escuche entrecortada o robótica.

Videojuegos: Provoca los famosos "tirones" o lag spikes





 






















































