# Instalación y ejecución del programa

> [!WARNING]
> Procure seguir las instrucciones paso a paso.

## Programas usados:
* GNS3
* Oracle VirtualBox

> [!IMPORTANT]
> El programa fue desarrollado en C, tanto el servidor como el cliente.

### Instalación GNS3

Primero, debemos dirigirnos a [la página de descarga de GNS3](https://www.gns3.com). Hacemos clic en el botón que dice 'Free Download' y descargamos la versión correspondiente a nuestro sistema operativo.

<p align="center">
  <img src="images/image.png" alt="Página de descarga GNS3">
</p>


Para la instalación, dejamos todo por defecto, y cuando el programa pregunte si queremos comprar una licencia, elegimos la opción 'No'.

<p align="center">
  <img src="images/image-1.png" alt="Opción de licencia">
</p>

Además, debemos descargar una imagen para poder hacer algunas configuraciones del programa más adelante. Es para la configuración de un router Cisco 3660 (c3660).

> [!NOTE]
> El archivo se puede encontrar en la carpeta Documentos-Utiles.

Para no tener problemas, el archivo debe guardarse en la siguiente dirección:
```
C:
└── Usuarios
    └── <su_usuario>
        └── GNS3
            └── imagenes
                └── IOS
```

Aquí finaliza la instalación de GNS3.

### Instalacón de Oracle VirtualBox

Nos dirigimos a [la página de descarga de Oracle VirtualBox](https://www.virtualbox.org). Hacemos clic en el botón que dice 'Download' y seleccionamos el sistema operativo correspondiente a nuestra instalación.

<p align="center">
  <img src="images/image-2.png" alt="Página de descarga VB">
</p>

Dejamos toda la instalación por defecto y tendríamos VirtualBox instalado.

## Instalación de la máquinas virtuales

Debemos descargar una imagen, que sería el archivo ISO de nuestro sistema operativo. En este caso, usaremos Ubuntu. Nos dirigimos [la página página principal de Ubuntu](https://ubuntu.com/download/desktop) y seleccionamos el botón que dice 'Download 24.04.1 LTS.'

<p align="center">
  <img src="images/image-3.png" alt="Página de descarga Ubuntu">
</p>

Para crear las máquinas virtuales, debemos ingresar a VirtualBox y hacer clic en el botón 'Nueva'.

En la pestaña 'Nombre y Sistema Operativo', debemos configurarlo de la siguiente forma: el nombre que queremos asignar a la máquina, la carpeta donde la queremos almacenar y seleccionar la imagen del sistema operativo. Lo demás lo dejamos por defecto.

<p align="center">
  <img src="images/image-4.png" alt="Crear máquina virtual">
</p>

> [!IMPORTANT]
> - En la pestaña 'Instalación desatendida' se recomienda configurar solo el usuario y contraseña de la maquina.
> - En 'Hardware' se recomienda usar 4 CPUs.
> - En 'Disco duro' se puede dejar por defecto.

Cuando hagamos clic en 'Terminar', se abrirá la máquina y comenzará la instalación del sistema operativo. Cuando finalice, reinicia la máquina.

> [!NOTE]
> Debe hacer este proceso dos veces, ya que necesitamos una máquina para el 'Cliente' y otra para el 'Servidor'.

<p align="center">
  <img src="images/image-5.png" alt="Crear máquina virtual">
</p>
 
## Configuración del programa en GNS3

Dentro del programa creamos un proyecto y para la configuración, hay que seguir tres pasos:

* Crear un router.
* Añadir las máquinas virtuales.
* Programar el proyecto.

### Crear un router

Creamos un nuevo 'Template' y seleccionamos la opción que dice 'Manually create new template'. En la barra de la izquierda, seleccionamos la opción 'IOS routers'. Hacemos clic en 'New' y dejamos todo por defecto.

<p align="center">
  <img src="images/image-6.png" alt="Creación c3660">
</p>

Cuando ya esté creadO , hacemos clic en 'Apply' y luego en 'OK'.

### Añadir máquinas virtuales

Creamos un nuevo 'Template' y seleccionamos la opción que dice 'Manually create new template'. En la barra de la izquierda, seleccionamos la opción 'VirtualBox VMs'. Hacemos clic en 'New' y el programa reconocerá automáticamente las máquinas virtuales. Seleccionamos el servidor y el cliente.

<p align="center">
  <img src="images/image-7.png" alt="Añadir máquinas">
</p>

Cuando ya estén creadas las máquinas, hacemos clic en 'Apply' y luego en 'OK'.

### Programar el proyecto

Primero, debemos entrar a GNS3 para configurar el adaptador de red de ambas máquinas. Seleccionamos la máquina, entramos en configuración, ponemos el modo 'Expert' y nos dirigimos a Red. Ahí, en 'Conectado a', seleccionamos NAT.

<p align="center">
  <img src="images/image-8.png" alt="Configuración NAT">
</p>

> [!IMPORTANT]
> Hacer esto con ambas maquinas antes de los siguientes pasos.

Después de configurar la red de las máquinas, nos dirigimos a GNS3. En la barra lateral izquierda, seleccionamos el tercer ícono que contiene todos los dispositivos. Arrastramos a la plantilla el router creado, las máquinas virtuales, un switch y un VPCS. Los conectamos de la siguiente forma:

<p align="center">
  <img src="images/image-9.png" alt="Plantilla del proyecto en GNS3">
</p>

Para hacer la conexión, debemos seleccionar el último ícono y hacer clic desde el dispositivo hacia el switch, y luego seleccionar el puerto.

**Ahora debemos configurar el router**

Hacemos clic derecho en el router y seleccionamos la opción 'Start'. Después de que inicie, volvemos a hacer clic derecho y seleccionamos la opción de 'Consola'.

Dentro de la consola damos enter para iniciar.

<p align="center">
  <img src="images/image-10.png" alt="Consola del router">
</p>

Ahora vamos a configurar una interfaz de red del router, en este caso, la primera. Para eso, debemos usar las siguientes líneas.

```
show ip interface brief
configure terminal
interface FastEthernet0/0
no shutdown
ip address 192.168.1.1 255.255.255.0
end
show ip interface brief
wr
```

Con esas líneas de comando, estamos:

* Mostrando las interfaces de red del router.
* Accediendo a la configuración de la primera interfaz.
* La encendemos.
* Le asignamos una IP con su máscara.
* Verificamos que se haya configurado correctamente.
* Guardamos.

Ahora volvemos a GNS3, hacemos doble clic en el equipo PC1, y nos dirigirá a la consola del equipo.