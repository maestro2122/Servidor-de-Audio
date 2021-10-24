# PROYECTO INSTALACIÓN Y CONFIGURACIÓN DE UN SERVIDOR DE AUDIO

# INTRODUCCIÓN

Los servicios de audio se adaptan a nuevas necesidades de los usuarios, lo que permite que las radios de cualquier lugar sean escuchas como si 
fueran radios locales.

En este proyecto se realizará la instalación, configuración de un servidor de audio para lo cual se requiere trabajar con icecast2 y ices2.

Icecast2 es un servidor de streaming que permite la creación de una estación de radio en internet. Soporta archivos Ogg y Mp3.
Cualquier usuario a nivel mundial puede conectarse a nuestra emisora y escuchar las canciones o programas que se emitan en tiempo real.

Icecast2 requiere de un cliente a quien entregará los contenidos para que funcione. Estos pueden ser los archivos de música, un micrófono conectado
a la tarjeta de sonido, el cliente es Ices2.


# DESARROLLO

Para el desarrollo de este proyecto a continuación se detalla los pasos a seguir:

# PARTE 1 INSTALACIÓN

1. Primeramente, debemos instalar icecast2(streaming), ice2(enviar los contenidos) y vorbis-tools. 
Para esto el comando a utilizar: sudo apt-get icecast2 ices2 vorbis-tools.

Ahora verificaremos la versión del servicio que hemos instalado 

<img src="/Servisor-de-Audio/img/imagen1.jpg"/>
![](https://github.com/maestro2122/Servidor-de-Audio/img/imagen1.jpg)
Imagen 1. Versión del servicio instalado 

2. ¿Qué son las vorbis-tools?
Son herramientas de línea de comando para ficheros de sonido con extensión Ogg, siendo utilizados para reproducir o editar estos ficheros.

Vorbis Tools contiene: 
Oggdec: Decodificador de archivos ogg
Oggenc: Codificador (convierte archivos .wav en ficheros Ogg Vorbis)
ogg123: Herramienta de reproducción
vcut: Divide ficheros en ogg
vorbiscomment: Editor de comentarios de ogg

3. ¿Qué otros formatos, además de ogg y mp3 soporta icecast2?
Soporta formatos como: Ogg Vorgis, MP3, Ogg Speex, OggFLAC, Ogg Theora.

4. ¿Cómo es el tipo MIME de los ficheros ogg?
Los MIME Types es la manera estándar de enviar contenido por medio de la red. Especificando los tipos de datos como son: audio, video, imagen, texto.
Un MiME de los ficheros de ogg es: audio/ogg Que es un archivo de audio en el formato de contenidos Ogg. Vorbis es el códec de audio utilizado en este contenedor
audio representa a cualquier tipo de archivos de audio.

5. ¿Cuál es la diferencia entre una transmisión unicast y una multicast? Averigüemos si icecast2 tiene soporte para ambas formas de transmisión.
Unicast es el tráfico dirigido hacia un equipo de la red. 
Una rama (frame) es enviada desde una interfaz de salida a una interfaz de destino, se la conoce como uno-a-uno.

Multicast es el tráfico dirigido hacia un grupo de equipos de la red.
Envía una interfaz de salida a un grupo específico de interfaces de destino, lo que permite ahorrar el ancho de banda.

icecast2 puede transmitir mediante multicast, unicast es muy limitante.


# PARTE 2 CONFIGURACIÓN DEL SERVIDOR

1. Para proceder con la configuración de servidor debemos ingresar mediante el comando sudo gedit /etc/icecast2/icecast.xml en donde debemos modificar en el fichero icecast2 
los siguientes parámetros: 

Nombre usuario del administrador y contraseña
Clave del transmisor 
Clave del enviador
Usuario y contraseña de la administración mediante la página web. 
2. Cambiar el nombre uoc en hostname.

Imagen 2. Configuración en el fichero icecast2


# PARTE 3 HABILITACIÓN DEL SERVICIO 

1. En este paso se debe configurar el puerto en nuestro caso colocamos el puerto 8080 como se puede observar en la imagen 2

2. Debemos habilitar el servicio icecast2.mxl, editar el siguiente comando sudo gedit /etc/default/icecast2 para editar el fichero icecast, en donde se debe cambiar la última 
línea ENABLE=true

Imagen 3. Cambio en la última línea por ENABLE=true

A continuación se debe arrancar el servicio mediante el comando service icecast2 start


# PARTE 4 HABILITACIÓN DEL SERVICIO

1. ¿Si quisiéramos parar el servicio qué comando se usaría?
El comando a utilizar es stop

2. Con el comando netsat-ntl verificamos si nuestro puerto está escuchándose.

Imagen 4. Verificación puerto escuchándose.

Pero que ¿significa la opción -ntl?
-NTL es una opción del comando NETSTAT que permite visualizar los puertos y direcciones en formato numérico 

4. Hagamos una prueba, conectémonos a la página de administración del service icecast2, coloquemos en el navegador http://ip del servidor:puerto.

Imagen 5. Conexión a la página de administración

# CONFIGURACIÓN DEL EMISOR (ices2)

Vamos a configurar el emisor ices2 es la fuente de audio que enviara los datos al servidor para que los retrasmita a los usuarios conectados. Comando a utilizar 
sudo mkdir /etc/ices2

1. Copiamos la lista de música playlist.xml que fue creado anteriormente. Comando sudo cp /usr/share/doc/ices2/examples/ices-playlist.xml /etc/ices2/ 

2. A continuación creamos un directorio para almacenar los log. Comando sudo mkdir /var/log/ices/

3. Crear el directorio donde se almacenará el contenido. Sentencia sudo mkdir /etc/ices2/music

4. Ahora debemos editar el fichero de configuración que anteriormente lo copiamos, esto con el fin de adaptarlo a los parámetros de nuestro servidor icecast. Para esto utilizamos el comando sudo gedit /etc/ices2/ices-playlist.xml

Modificamos La sección stream; input

Imagen 6. Configuración playlist.xml (stream e input)

También debemos configurar la sección instance en donde se configura la conexión par que el ices2 se pueda conectar al servidor icecast.

Imagen 7. Configuración playlist.xml (instance)

5. Adicionalmente se debe generar el archivo milista.txt dentro de la siguiente ruta /etc/ices2/milista.txt

El archivo contendrá la lista de música que se reproducirá en la emisora, añadiendo la ruta de estos ficheros para esto se puede utilizar la sentencia 
sudo find / -iname "*.ogg" >> /etc/ices2/milista.txt


# PARTE 5 TRANSMISIÓN

1. En el fichero playlist.xml podemos configurar la calidad a la cual se transmitirá, este valor es importante debido a que indica la velocidad a la que el audio se reproduce, es decir, que a mayor velocidad de la canción mucho mejor la calidad del sonido, pero el archivo será más grande.  

Imagen 8. Calidad en la transmisión.

2. A continuación presentamos el contenido de nuestro fichero con las rutas de las canciones a producirse.

Imagen 9. Contenido fichero milista.txt

3. Ahora debemos poner en funcionamiento al emisor y arrancar el servicio, esto lo realizamos con la sentencia ices2/etc/ices-playlist.xml &

Imagen 10. Puntos de montaje activos desde la página de administración

4. En la siguiente imagen se observará la calidad con la que está enviando el codec al servidor. 

El modo de transmisión VBR consta desde la carga del archivo de música hasta que se retransmite, la calidad se determina pro el tamaño en bits del valor nominal, en nuestro caso
64000 hace referencia a los 64Kb/s

Imagen 11. Calidad con la que está enviando el codec al servidor


# CONEXIÓN CON CLIENTES DE STREAMING (VLC) Y ANÁLISIS DE TRÁFICO

1. Primero debemos descargar e instalar VLC en nuestro servidor. Sentencia a utilizar sudo apt-get install vlc

2 y 3. Se debe abrir el VLC ir al menú y escoger la opción Abrir Ubicación de Red, en donde se va a colocar la dirección ip de nuestro servidor, puerto y punto de montaje, en nuestro caso  http://localhost:8080/endirecto, pulsar reproducir

Imagen 12. Conexión VLC


# PARTE 6 VLC

1. Observemos en la siguiente imagen información sobre el codec, esto lo encontramos en el menú herramientas opción Información del codec

Imagen 13. Codec

2. Adicional vamos a mirar en la página de administración de icecast, en la opción Mountpoint List el VLC conectado

Imagen 14. VLC conectado

3. Ahora vamos analizar el tráfico mediante Wireshark, como sabemos existen tres categorías UDP streaming, HTTP streaming y adaptive HTTP streaming. Mediante VLC se procede a 
analizar los paquetes mostrando lo siguiente:

UDP = No se originaron paquetes

Imagen 15. Analizando el tráfico

4. ¿Qué es Adaptive Bit Rade?
También conocido como streaming de video adaptivo, permite enviar vídeo mediante el internet de una forma eficiente, mediante la calidad de la imagen de acuerdo a los recursos 
de cada usuario. Se basando se en entregar el vídeo con la misma calidad a todos usuarios, sin tomar en cuenta su dispositivo o ancho de banda. 

Entre VBR (variable bit rate) y ABR (Adaptive bir rate) existe diferencias como:
VBR = Se esfuerza por mantener la calidad constante del archivo
ABR = Se esfuerza por mantener el constante tamaño del archivo 

La calidad resultante de los archivos codificados ABR es desconocida, mientras que el tamaño del archivo resultante de los archivos codificados VBR es desconocido.
VBR produce una mejor relación de calidad sobre el tamaño en comparación con ABR
VBR producirá archivos pequeños con configuración alta en comparación con ABR
ABR es una forma de codificar archivos de audio VBR

Icecas2 usa VBR, se debe modificar manualmente la calidad del audio, es decir, que no modifica el audio según las características del cliente. Icecast2 no tiene implementado ABR 
pero si VBR.

Imagen 16. Archivos codificados con VBR

5. Un servidor icecast puede tener varios broadcast o también llamados puntos de montaje y cada uno con un contenido diferente, observar imagen 17.

Imagen 17. Dos puntos broadcast

En la imagen 18 se puede observar in bitrate con el mismo contenido pero diferente calidad.

Imagen 18. Bitrate

