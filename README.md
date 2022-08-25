# RNSES-Cynthia-P4
## Tabla de contenidos 
* [Información](#Información)
* [Tarea 1. Conexión a red WiFi](#Tarea1)
* [Tarea 2. Poner en hora el módulo mediante un servidor NTP](#setup)
* [Tarea 3. Aplicación software](#set)
* [Tarea 4. Servidor WEB](#sp)
* [Tarea 5. Crear un fichero json](#sp4)
* [Tarea 6. Subir datos a la nube](#sp5)

## Información
Comunicaciones WIFI y stack IP. Manejo de protocolos de alto nivel(HTTP, FTP, NTP, MQTT) y estándares de interoperabilidad (SENML).

## Tarea 1
Conexión a la red wifi del laboratorio o a una creada por el móvil como punto de acceso. Extrae tu IP. Comprueba la conectividad con Google mediante un ping.

## Tarea 2
Poner en hora el módulo mediante un servidor NTP
* https://www.esp32.com/viewtopic.php?t=5978
* https://lastminuteengineers.com/esp32-ntp-server-date-time-tutorial/
 ## Tarea 3
* Montar un chat una aplicación software PC (http://sockettest.sourceforge.net/) o con una aplicación móvil (simple TCP socket tester). A veces el firewall del ordenador no permite las conexiones externas, y es necesario configurarlo correctamente.
* Sustituir uno de los extremos por el módulo hardware siendo cliente y envíar cada segundo la hora local.
* Añadir una capa de control de tal modo que cuando se le mande “start” empiece a mandar la hora hasta que se le mande “stop”.
## Tarea 4 
Montar un servidor WEB (https://randomnerdtutorials.com/esp32-web-server-spiffs-spi-flash-file-system/) que muestre la hora y tenga un botón para resetear la hora a las 0:00
## Tarea 5
Basándose en el estándar SENML (https://tools.ietf.org/html/rfc8428) crear un fichero json (https://arduinojson.org/) que se genere cada 10 segundos, que contenga datos de temperatura inventados, las unidades y la marca temporal. Súbelos a un servidor ftp con un nombre que sea grupoXX_ddmmss.json. (https://platformio.org/lib/show/6543/esp32_ftpclient). Usa el del laboratorio (IP: 155.210.150.77, user: rsense, pass: rsense) o móntate uno con https://filezilla-project.org/download.php?type=server (asegúrate que el firewall permite conexiones entrantes).

## Tarea 6
Subir datos a la nube, en concreto al servicio gratuito proporcionado por Adafruit.
* En primer lugar se debe de crear un usuario en https://io.adafruit.com/, generar una aplicación y obtener un “AIO Key” y crear un feed al que subir datos. Leer info en https://learn.adafruit.com/adafruit-io-basics-feeds
* Subir datos desde navegador (https://www.apirequest.io/) utilizando POST HTTP según documentación (https://io.adafruit.com/api/docs/?shell#create-data)
* Usar librería de Adafruit IO (https://github.com/adafruit/Adafruit_IO_Arduino) para subir datos al feed usando MQTT.
* Usar la librería para suscribirte al feed y comprueba que recibes actualización al escribir desde el navegador.
* https://github.com/plapointe6/EspMQTTClient
