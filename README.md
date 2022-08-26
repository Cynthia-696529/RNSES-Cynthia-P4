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
* Socket: https://www.keil.com/pack/doc/mw/Network/html/group__net_sockets.html


## Tarea 1
Conexión a la red WiFi creada por el móvil como punto de acceso. Extrae tu IP. Comprueba la conectividad con Google mediante un ping.
* Código - 1
```
/*Conexion red Wifi creada por el movil como punto de acceso
  Extraer IP
*/
#include <WiFi.h>

const char* ssID     = "ESP32_wifi"; // Nombre WiFi
const char* password = "patata13"; // Contraseña 

void setup()
{
    Serial.begin(115200);
    delay(10);

    Serial.println();
    Serial.print("Conectando a: ");
    Serial.println(ssID);

    WiFi.begin(ssID, password);

    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }

    Serial.println("");
    Serial.println("Conectado a WiFi");
    Serial.println("Dirección IP: ");
    Serial.println(WiFi.localIP());
}


void loop() {
  // put your main code here, to run repeatedly:

}
```
* Proceso

Encender Mobile Hotspot, ir a la configuración, poner el mismo nombre de la red Wifi que se ha puesto en el código "ESP32_wifi"  y la misma contraseña "patata13"

![mobile hotspot](https://github.com/Cynthia-696529/Imagenes/blob/6ea7d953868d23abd81e8e88ee98747437c5280e/mobileHotspot.jpeg)

![red-contraseña](https://github.com/Cynthia-696529/Imagenes/blob/6ea7d953868d23abd81e8e88ee98747437c5280e/esp32_wifi.jpeg)

![IP](https://github.com/Cynthia-696529/Imagenes/blob/4f14861baf29ce4eee8301509c123de7ff6c5865/Captura%20de%20pantalla%202022-08-25%20a%20las%2011.51.58.png)

*  Codigo - 2

  Es necesario descargarse la librería: https://github.com/marian-craciunescu/ESP32Ping y añadir la librería al entorno IDE         (https://programarfacil.com/blog/arduino-blog/instalar-una-libreria-de-arduino/)

  ```
#include <ESP32Ping.h>
#include <ping.h>

/*Comprobacion conectividad Google-ping
*/
#include <WiFi.h>

const char* ssID     = "ESP32_wifi"; // Nombre WiFi
const char* password = "patata13"; // Contraseña 

void setup() {
  Serial.begin(115200);
 
  WiFi.begin(ssID, password);
   
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.println("Connecting to WiFi...");
  }
 
  bool success = Ping.ping("www.google.com", 3);
 
  if(!success){
    Serial.println("Ping failed");
    return;
  }
 
  Serial.println("Ping succesful."); 
}
 
void loop() {
  // put your main code here, to run repeatedly:

}
  ```
 
![google-ping](https://github.com/Cynthia-696529/Imagenes/blob/46271ab1ab0b79d1a8319dcc5c5b4412e0c2f572/google-ping.png)

## Tarea 2
Poner en hora el módulo mediante un servidor NTP
* https://www.esp32.com/viewtopic.php?t=5978
* https://lastminuteengineers.com/esp32-ntp-server-date-time-tutorial/
* Código
```
/*TAREA 2 :Poner en hora el modulo mediante servidor NTP
*/
#include <WiFi.h>
#include "time.h"

const char* ssID     = "ESP32_wifi"; // Nombre WiFi
const char* password = "patata13"; // Contraseña 

const char* ntpServer = "pool.ntp.org";   // Servidor horario
const long  gmtOffset_sec = 3600;         // Compensar UTC para mi zona horaria en ms
const int   daylightOffset_sec = 3600;    // Compensar la luz del día 

void setup() {
 Serial.begin(115200);
  //connect to WiFi
  Serial.printf("Connecting to %s ", ssID);
  WiFi.begin(ssID, password);
  while (WiFi.status() != WL_CONNECTED) {
      delay(500);
      Serial.print(".");
  }
  Serial.println("CONNECTED");
  
  //init and get the time
  configTime(gmtOffset_sec, daylightOffset_sec, ntpServer); // inicializar el cliente NTP 
                                                            // obtener la fecha y la hora de un servidor NTP
  printLocalTime();                                         // imprimir hora actual.
  //disconnect WiFi as it's no longer needed
  WiFi.disconnect(true);
  WiFi.mode(WIFI_OFF);
}

void loop() {
  delay(1000);
  printLocalTime();   //hora local
}

/*Funciones auxiliares*/
void printLocalTime(){
  struct tm timeinfo;
  if(!getLocalTime(&timeinfo)){
    Serial.println("Failed to obtain time");
    return;
  }
  Serial.println(&timeinfo, " %H:%M:%S");  // imprimir hora  
}  
```
![hora](https://github.com/Cynthia-696529/Imagenes/blob/11d776754df9ee966c83f65dd62f13ceec31786b/hora.png)


 ## Tarea 3
* Montar un chat una aplicación software PC (http://sockettest.sourceforge.net) o con una aplicación móvil (simple TCP socket tester). A veces el firewall del ordenador no permite las conexiones externas, y es necesario configurarlo correctamente.
    * Paso 1: actualizar o descargar Java si es necesario.
    * Paso 2: descargar Sockettest (http://sockettest.sourceforge.net), descarga la carpeta de github. Abrir "SocketTest-master" y en la carpeta dist ejecutar socketTest.jar. Si hay problemas de seguridad abrir **Preferencias del sistema** -> Java-> Seguridad-> Lista de excepciones-> editar lista de sitios. Hacer doble clic en alguna de las líneas y añadir la url de http://sockettest.sourceforge.net, clic en agregar y aceptar. Ejecutar de nuevo socketTest.jar. 
    * Paso 3: abrir Terminal y ejecuar el comando: sudo service ssh status. Elegir uno de los puertos udp que aparece para realizar el test.
    ![udp_port](https://github.com/Cynthia-696529/Imagenes/blob/e66b434f177d22f054c9609d6431d163b5e56d08/sudo.png)
    * Paso 4: en la pestaña **Udp**, escribir el puerto elegido y conectar
    ![udp_socket](https://github.com/Cynthia-696529/Imagenes/blob/afa033e5bc3f0abe88ca5a8147b6090b29a06534/udp.png)
    * Paso 5: Testeo. Escribir el mismo puerto en **Server** y **Cliente** y conectar. Enviar mensajes para comprobar el correcto funcionamiento
    ![testeo](https://github.com/Cynthia-696529/Imagenes/blob/d7a6cb2cfa274544d633dd9333c904696c9e435d/testsock.png)
* Sustituir uno de los extremos por el módulo hardware siendo cliente y envíar cada segundo la hora local.
      * Paso 1: 
      https://techtutorialsx.com/2018/05/17/esp32-arduino-sending-data-with-socket-client/
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
