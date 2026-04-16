# Ejercicio-en-Clase-
Esta actividad consistio en analizar el rastro digital del modelo de visión por computadora YOLOv8. El objetivo es contrastar el comportamiento de los protocolos TCP (durante la descarga de componentes) y UDP (durante la inferencia de video en tiempo real).

## Preparación del Entorno

<img width="921" height="58" alt="image" src="https://github.com/user-attachments/assets/f82c6785-0151-40ae-b289-b0ba282ed9c8" />
<img width="921" height="346" alt="image" src="https://github.com/user-attachments/assets/f322ea1d-771f-4719-9fc0-5017431403b0" />

Se utilizó Google Colab como servidor en la nube basado en Linux, configurando el entorno con GPU para optimizar el rendimiento de YOLOv8. Se instalaron las librerías necesarias y la herramienta de captura de paquetes .ultralyticstcpdump

## Análisis de la Descarga (Tráfico TCP)

<img width="921" height="164" alt="image" src="https://github.com/user-attachments/assets/ed2a775f-e48b-4ce9-b5cc-c031b2b6b9cd" />
Para la descarga del modelo pre-entrenado, ejecutamos el siguiente comando en segundo plano:!sudo tcpdump -i eth0 -s 1500 -w descarga_tcp.pcap &
<img width="921" height="486" alt="image" src="https://github.com/user-attachments/assets/dcc8e71a-e3fd-4140-a866-de6c71941642" />
Se utilizó el protocolo TCP porque es orientado a conexión y fiable. Esto garantiza que el modelo se descargue sin errores, ya que cualquier pérdida de datos corrompería el archivo.yolov8n.pt
<img width="921" height="156" alt="image" src="https://github.com/user-attachments/assets/978f52cf-84e1-40cb-a1c3-6e2653c2b5d5" />
<img width="921" height="136" alt="image" src="https://github.com/user-attachments/assets/99527dd2-925d-4175-8d0a-a18701774f4b" />

## Transmisión de Video (Tráfico UDP)

<img width="744" height="131" alt="image" src="https://github.com/user-attachments/assets/d64bcb85-282b-4ec6-8be5-fb578acc7048" />
Simulamos una transmisión de video enviando paquetes desde un script de Python hacia el puerto . Análisis: Para esta tarea usamos UDP. A diferencia de TCP, UDP no garantiza la entrega pero minimiza la latencia, lo cual es vital para la transmisión de vídeo en tiempo real donde un retraso por retransmisión es más perjudicial que la pérdida de un fotograma.12345

## Análisis Forense con Wireshark

<img width="531" height="102" alt="image" src="https://github.com/user-attachments/assets/184182db-41e1-4e10-bb11-6a05db093efc" />
<img width="1906" height="995" alt="image" src="https://github.com/user-attachments/assets/82adca0c-1ab1-4093-95d9-7ce33170aac7" />
Al abrir los archivos en Wireshark, aplicamos filtros específicos para entender el diálogo de red:.pcap
<img width="921" height="486" alt="image" src="https://github.com/user-attachments/assets/da433ded-0328-4689-a63e-c9f633f5daea" />

tcp.flags.syn == 1
<img width="1862" height="989" alt="Captura de pantalla 2026-04-11 222206" src="https://github.com/user-attachments/assets/e247e662-12ad-475f-bf75-fbc0d3021ff9" />

tcp.analysis.retransmission
<img width="1748" height="1000" alt="image" src="https://github.com/user-attachments/assets/dbae95d0-1ccb-4fc3-a9dc-51e2238ba68a" />

tcp.port == 443:
<img width="1324" height="692" alt="image" src="https://github.com/user-attachments/assets/dfd9259b-977e-436a-8d4f-d82a3292df7b" />

# Preguntas:

### ¿Qué es YOLO y su arquitectura?
YOLO (You Only Look Once) es un modelo de detección de objetos que utiliza una sola red neuronal convolucional para predecir cuadros y clases directamente de imágenes completas en una sola pasada.

### Protocolo vs. Aplicación (¿Por qué TCP para descarga y UDP para video?):
La descarga requiere integridad (TCP asegura que el archivo sea idéntico al original). El video requiere fluidez (UDP evita el "lag" al no retransmitir paquetes perdidos que ya serían obsoletos).

### Fiabilidad vs. Velocidad (Retransmisiones): 
Si en vemos , significa que el protocolo está recuperando datos perdidos. En un video en vivo, esto causaría que la imagen se congele, por lo que es mejor ignorar la pérdida (UDP).descarga_tcp.pcaptcp.analysis.retransmission

### Identificando el Origen: 
Para aislar el tráfico de los servidores de Google Cloud o Ultralytics, se utilizan filtros de dirección IP como o .ip.src == [Dirección_IP]ip.addr









