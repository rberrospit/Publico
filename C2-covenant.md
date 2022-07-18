
# Command and Control: Covenant
Covenant es un C2 desarrollado en C# y que posee una interfaz web, la cual posee un aspecto sencillo que resulta intuitivo al momento de interactuar. A continuación se presentará paso a paso la instalación del **Covenant** y una breve demostración de su uso.

## Descarga e Instalación
La instalación del **Covenant** se puede realizar por dos métodos:
1. Instalación de DotNET
2. Contenedor Docker

Para esta demostración se realizará la instalación mediante el uso de DotNET
### Instalación del repositorio Covenant y DotNET
Clonar el repositorio:
```
git clone --recurse-submodules https://github.com/cobbr/Covenant
```
Instalación de repositorios de Microsoft:
```
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget -q https://packages.microsoft.com/config/debian/10/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
```
Actualización e instalación de apt-transport-https y dotnet-sdk:
```
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt install dotnet-sdk-3.1
```
Correr el Covenant:
```
cd Covenant/Covenant
sudo dotnet build
sudo dotnet run
```
Con ello obtendremos el siguiente resultado:

![Inicio Covenant](https://user-images.githubusercontent.com/36284620/179444477-2664b2be-3fb3-44eb-b8b8-9df2dc957dfa.PNG)

## Exploración de la interfaz web
Se podrá ingresar a la interfaz web del Covenant mediante la dirección **https://127.0.0.1:7443**. En la primera pantalla que se observará será para registrar el usuario y contraseña; sin embargo, como ya se ha creado con anterioridad lo solicitado, la pantalla que se mostrará será el login.

![Login](https://user-images.githubusercontent.com/36284620/179445063-f8ff7280-d462-4c93-8ab6-7a79fdb7afb6.PNG)

A continuación, se observa el **Dashboard** del Covenant con todas las acciones que se pueden realizar dentro del C2. Entre las principales podemos encontrar: Listeners, Launchers, Grunts, Templates, etc.

![DashBoardCovenant](https://user-images.githubusercontent.com/36284620/179449560-3355fe0d-018d-4127-a68d-0266117f90d6.PNG)

Dentro de la opción de **Listeners** se listarán todos los listeners que se encuentren activos y también te brindan la opción de crear uno. El **listener** es un servicio del C2 que permite escuchar las conexiones provenientes de la PC objetivo. Entre los parámetros que se pueden modificar se encuentran: Nombre del listener, BindAddress, BindPort, ConnectPort, ConnectAdresses.

![CrearListener](https://user-images.githubusercontent.com/36284620/179450160-99d8826b-ce61-4ecb-af98-6ff3dd403999.PNG)

Dentro de la opción **Launchers** se encuentran los métodos que se utilizarán para establecer la conexión entre la máquina del atacante y el objetivo; se podría considerar como el análogo de los payloads en Metasploit. Entre ellos se cuentran: Binary, ShellCode, PowerShell, etc.

![Launchers](https://user-images.githubusercontent.com/36284620/179450862-59466254-9fe4-4948-a669-a60257e30664.PNG)

En **Grunts** podemos observar las sesiones de las máquinas objetivo vinculadas a nuestro Covenant. Podemos interactuar con cada una de estas sesiones; sin embargo, se observará en el siguiente artículo donde se utilizará este C2 en el **Metasploiteable 3**.

![Grunts](https://user-images.githubusercontent.com/36284620/179451333-d56abfdf-0348-458a-b6e3-8d834d2a4401.PNG)

# Acceso a Metasploiteable3 mediante Covenant
En el presente artículo se explicará paso a paso el acceso a la máquina virtual **Metasploiteable3** mediante el uso del C2 Covenant.
## Creación del Listener
Para iniciar, se creará el listener que se utilizará para establecer la comunicación con la máquina víctima. Es importante que tanto la **IP** como el **puerto** definidos entre los parámetros sean accesibles para la víctima; es decir, que se encuentren en la misma red. En el caso de una red distinta, se pueden utilizar otros métodos (ej. **ngrok**) para poder establecer la conexión. 

La IP a utilizar será 192.168.213.128 y el puerto será el 80.

![CrearListener](https://user-images.githubusercontent.com/36284620/179459642-bbafb35e-30b7-485c-b9f8-0cd9be0e7e5a.PNG)

## PowerShell Launcher
En la sección de **Launcher** se presentan diversos métodos para poderse ejecutar en la PC comprometida, de manera que se pueda entablar la conexión. Para esta demostración se utilizará el **PowerShell**. 

Dentro de esta opción observamos ciertos parámetros para poder generar el comando PowerShell que se utilizará. Entre éstas, debemos seleccionar el listener que creamos anteriormente. Al darle click en generar, podremos observar el comando plano y codificado.

![PowerShellGenerado](https://user-images.githubusercontent.com/36284620/179460821-07fb60c2-95c9-4479-a664-4264449dc804.PNG)

Otra información adicional que podemos observar en esta ventana son las opciones **Host** y **Code**. Dentro de Host podemos ver una **Url** donde se puede alojar de manera comprimida el comando PowerShell; este Url puede ser modificado para poder hostear el archivo generado. Por otro lado, en Code se observa el código fuente del launcher.

![CodeLauncher](https://user-images.githubusercontent.com/36284620/179461503-6e14051e-eb81-48c0-bc0f-871048428fde.PNG)

## Generación de los Grunt
El código generado en el **Launcher** se copiará y pegará en el PowerShell de la máquina víctima. Una vez se ingresa el comando, se cerrará la pantalla del PowerShell y en nuestra interfaz se mostrará una alerta de que se obtuvo un nuevo **grunt** o sesión.

![PowerShellVíctima](https://user-images.githubusercontent.com/36284620/179462516-aaaf1dd8-30fe-42da-9e6b-cabb5e6ac41d.PNG)

![GruntAlarma](https://user-images.githubusercontent.com/36284620/179462621-14a25fed-138e-4feb-bf66-0d82bb2788d7.PNG)

## Interacción con el Grunt
Cada sesión generada por un objetivo comprometido se le llama **grunt** y mediante el Covenant podemos interactuar con cada uno de éstos. Al ingresar a la sección Grunt, podemos visualizar una lista de todas las sesiones (tantos las activas como las que se perdieron) vinculadas al listener y cada una posee un ID único para poder identificarlos.

Al acceder a uno de los Grunt, podemos visualizar 4 pestañas diferentes:
1. Info
2. Interact
3. Task
4. Taskings

### Info
Dentro de **Info** se encuentra toda la información relacionada al grunt; por ejemplo: Username, UserDomainName, IPAddress, Hostname, etc.

![Grunt](https://user-images.githubusercontent.com/36284620/179464605-ebeff29e-d0b2-4cf5-90df-14101a4937e8.PNG)

### Interact
En esta pestaña podemos interactuar con la sesión. La misma interfaz nos brinda opciones de comandos que podemos ejecutar en la víctima, obteniendo de esta manera información importante.

![Interacción](https://user-images.githubusercontent.com/36284620/179465231-a4416eb2-18ee-4c2b-8c14-9560b265732b.PNG)

### Task
En **Task** podemos ver una lista de tareas (o conjunto de comandos) que se pueden ejectuar en la PC comprometida. Esta opción facilita el hecho de ir ingresando comandos en la pestaña de Interact, ya que son tareas predefinidas. Se pueden crear tasks en la opción **Tasks** de la interfaz web, además de poder visualizar la descripción de cada una que se encuentra creada por defecto.

![Task](https://user-images.githubusercontent.com/36284620/179466044-929a2b83-8269-424a-873a-96c1b4ac60df.PNG)

### Tasking
Por último, en la pestaña de **Tasking** se visualiza un listado de todos los comandos que fueron efectutados en el grunt seleccionado. La diferencia de esta pestaña con la opción **Tasking** de la interfaz es que la segunda muestra la lista de comandos utilizadas en todos los grunts.

![Tasking](https://user-images.githubusercontent.com/36284620/179466444-cbcba66e-4ac9-40a7-8857-47fceb045794.PNG)




