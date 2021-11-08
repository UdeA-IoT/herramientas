# Trabajando con Platformio

## Sobre Platformio

Es una cross-platform, cross-architecture, multiple framework que permite el desarrollo de aplicaciones para sistemas embebidos. La mayor ventaja de esto es que la herramienta permite que el programador pueda trabajar con diferentes plataformas (arduino, esp8266, rPi, etc.) usando el mismo IDE siguiendo el siguiente ciclo de desarrollo:
1.	Seleccionar la board(s) de interés en el archivo platformio.ini (Project Configuration File).
2.	Basado en la lista de boards, PlatformIO descargará las toolchains y las instalará automaticamente.
3.	Programar, compilar, preparar y subir la aplicación a las boards de interés.

## Instalación

Existe muy buena documentación sobre cómo llevar a cabo la instalación de platformio la cual se encuentra en https://platformio.org/install. Por lo tanto esta sección lo único que hicimos fue dedicarnos a copiar y pegar lo que allí se explica.
1.	[Descargar](https://code.visualstudio.com/) el Microsoft Visual Studio Code pues el PlatformIO IDE está construido sobre este.
2.	Abrir el VSCode Package Manager
3.	Buscar la extensión oficial del platformio ide
4.	Instalar el PlatformIO IDE.

![fig1](platformio1.jpg)

## Trabajando con Platformio

En el siguiente [enlace](https://docs.platformio.org/en/latest/integration/ide/vscode.html#quick-start) se muestra una guía rápida para trabajar con platformio. Como esta guia esta enfocada en el arduino, vamos a seguir el tutorial **Quick start with NodeMCU v3 (ESP8266) , Arduino ecosystem and PlatformIO IDE** ([enlace](https://loginov-rocks.medium.com/quick-start-with-nodemcu-v3-esp8266-arduino-ecosystem-and-platformio-ide-b8415bf9a038)) en el cual se explica cómo programar el NodeMCU usando platformio. Veamos los pasos a continuación:
1.	Ubicarse en el home de platformio:

![fig2](platformio2.jpg)

2.	Crear un nuevo proyecto (**New project**) y configurarlo, para el caso:
    * **Name**: blink
    * **Board**: NodeMCU 1.0 (ESP-12E Module)
    * **Framework**: Arduino.

    Luego de esto se da click en **finish**.

![fig3](platformio3.jpg)


**Nota**: Si lo que se desea es usar otra localización diferente a la que viene por defecto, se puede deseleccionar la casilla de Location que define esto y se procede a seleccionar la ruta.

3. Una vez se da **finish**, si todo está bien se descargan las dependencias y se construye la estructura del proyecto. Dentro de los archivos generados se encuentra el archivo de configuración del proyecto **platformio.ini** (para más información sobre puede consultar el siguiente [enlace](https://docs.platformio.org/en/latest/projectconf/)). A continuación se muestra el archivo de configuración, generado para nuestro ejemplo:

![fig4](platformio4.jpg)

Otro archivo de importancia generado es el archivo ```main.cpp``` (esta se encuentra en el directorio ```src```) el cual contiene la plantilla del código fuente que modificaremos para definir la lógica de nuestra aplicación:

![fig5](platformio5.jpg)

Para el caso, lo que deseamos hacer que parpadee el led que viene integrado con la tarjeta del NodeMCU con un periodo de un segundo. A continuación, se muestran los pines del NodeMCU. Para el caso, el pin asociado al led integrado en la tarjeta es el **GPIO2**.

![fig6](platformio6.jpg)

De este modo, el programa a subir a la tarjeta será el siguiente:

```C
#include <Arduino.h>

#define LED 2   // Assign LED to pin GPIO2 (Built-in LED)

void setup() {
  // put your setup code here, to run once:
  pinMode(LED, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(LED, LOW);
  delay(500);
  digitalWrite(LED, HIGH);
  delay(1000);
}
```

Así el código modificado quedará:

![fig7](platformio7.jpg)

4. Finalmente, solo resta hacer el build y el upload empleando los botones destinados para ello. La siguiente figura muestra como hacer el **build** (```CTRL + ALT + b```):

![fig8](platformio8.jpg)

La siguiente figura muestra como hacer el **upload** (```CTRL + ALT + u```):


Si el proceso de upload (carga del programa en la plataforma) está bien, saldrá en la terminal algo como lo siguiente:

![fig10](platformio11.jpg)

5. Finalmente una vez cargado el programa, se verá el led titilando.



## Casos de interes

A continuación, se analizarán dos casos que son muy comunes en muchas de las aplicaciones que se harán a lo largo del curso:
1.	Uso del puerto serial.
2.	Uso del wifi.

### Uso del puerto serial

Para tratar este caso, partamos del siguiente problema. 

**Problema**: Hacer una aplicación en arduino que permita prender y apagar el led integrado a la tarjeta NodeMCU mediante las instrucciones mostradas en la siguiente tabla: 

|Comando|Efecto|
|---|---|
|```h```|Enciende el led|
|```l```|Apaga el led|

La codificación del procedimiento es similar a la realizada en el ejemplo anterior, pero, en este caso el código fuente a codificar se muestra a continuación, se muestra a continuación:

```C
#include <Arduino.h>

#define LED 2   // Assign LED to pin GPIO2 (Built-in LED)

byte comando;

void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
  Serial.write("Configurando puerto serial a 9600bps\n");
  // initialize digital pin LED as an output.
  pinMode(LED, OUTPUT);
}

void loop() {
  // check if data has been sent from the computer
  if (Serial.available()) {
    // read the most recent byte 
    comando = Serial.read();
    if(comando == 'h') {
      digitalWrite(LED, HIGH);
      Serial.write("Led encendido\n");
    }
    else if(comando == 'l') {
      digitalWrite(LED, LOW);
      Serial.write("Led apagado\n");
    }
  } 
}
```

Es buena práctica que una vez codificado el código, este sea construido (```CTRL + ALT + b``` en linux) antes de que sea subido a la plataforma. 

La importancia de este problema radica en que se va a hacer uso del puerto serial, por lo que es necesario agregar las líneas para configurar el puerto serial en el archivo **platformio.ini**. Inicialmente, es bueno verificar cual es el puerto serial al que quedó asignado el NodeMCU una vez que este fue conectado lo cual depende del sistema operativo ([link](https://la.mathworks.com/help/supportpkg/arduinoio/ug/find-arduino-port-on-windows-mac-and-linux.html)). 

El puerto USB que aparezca al realizar la verificación, es aquel que está conectado el NodeMCU (supongamos que el resultado dio ttyUSB0). 

Inicialmente si abrimos el archivo platformio.ini tendremos algo como lo siguiente:

```ini
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:nodemcuv2]
platform = espressif8266
board = nodemcuv2
framework = arduino
```

Teniendo en cuenta que el puerto de trabajo será el ```ttyUSB0``` (para este ejemplo) y la velocidad serial será de 9800 bps. Se agregaron las siguientes líneas en el archivo de configuración (para más información de como configurar el monitor serial puede consultar el enlace [Monitor Options](https://docs.platformio.org/en/latest/projectconf/section_env_monitor.html)):


```ini
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:nodemcuv2]
platform = espressif8266
board = nodemcuv2
framework = arduino

; Custom Serial Monitor port
monitor_port = /dev/ttyUSB0

; Custom Serial Monitor speed (baud rate)
monitor_speed = 9600
```

De este modo el archivo platformio.ini queda así:

![fig11](platformio12.jpg)

Una vez hecho esto, el paso siguiente consistirá en subir el código al NodeMCU (```CTRL + ALT + u``` en linux). 

De este modo solo resta probar en el monitor serial, para ello se da click en el botón resaltado (o se ejecuta la combinación de teclas ```CTRL + ALT + s```) en la siguiente figura:

![fig12](platformio13.jpg)

Finalmente, una vez llamado el monitor serial, se hace un reset del NodeMCU y se ejecuta la aplicación de acuerdo a la lógica programada:

![fig13](platformio14.jpg)

 Cuando culmine la prueba con la combinación de letras ```CTRL + C``` puede salir del monitor serial. 

 ### Uso del wifi

 El otro caso que es necesario analizar es la conexión por Wifi, esto debido al gran número de aplicaciones de IoT que se conectan inalámbricamente usando esta tecnología. Como en muchos casos, Wifi es una librería externa por lo que la clave, está en instalar y usar esta y otras librerías en el proyecto. Para más información puede consultar el enlace [Library Manager](https://docs.platformio.org/en/latest/librarymanager/index.html). 

Para nuestro caso vamos a adaptar el ejemplo descrito en la segunda parte del tutorial: [Quick start with NodeMCU v3 (ESP8266), Arduino ecosystem and PlatformIO IDE](https://loginov-rocks.medium.com/quick-start-with-nodemcu-v3-esp8266-arduino-ecosystem-and-platformio-ide-b8415bf9a038). 

Inicialmente, tal y como se ha realizado anteriormente, procedemos a crear un proyecto. El cual para el caso llamaremos **wifi-test**. Tal y como sucede inicialmente, el archivo **platformio.ini** tiene la siguiente configuración basica:

![fig14](platformio15.jpg)

Como nuestro proyecto va a hacer uso de Wifi, es necesario agregar esta librería, de modo que lo primero que se debe hacer es ir al gestor de librerías y descargarla. Para ello primero cargamos el home del platformio:

![fig15](platformio16.jpg)

Una vez aparezca el **home** se presiona el botón del gestor de librerías:

![fig16](platformio17.jpg)

Para buscar la librería vamos al gestor de librerías y en el campo para la búsqueda digitamos la librería o palabra clave de nuestro interés (cuadro amarillo). Como resultado aparecerá una lista de librerías, donde para nuestro caso procedimos a instalar la librería **WifiManager** tal y como se muestra en la siguiente figura:

![fig17](platformio18.jpg)

Para instalarla damos click en esta librería (cuadro naranja) lo que hace que como resultado aparezca la siguiente pestaña. De modo que para instalar la librería procedemos a dar click en el botón **Install**:

![fig18](platformio19.jpg)

Luego podemos verificar que la libreria está instalada dando click en la pestaña **Installed** tal y como se muestra a continuación: 

![fig19](platformio20.jpg)

El siguiente paso consiste en agregar la librería recién instalada en el archivo de configuración. Damos click en el nombre de la librería (**WifiManager**):

![fig20](platformio21.jpg)

Como se puede notar, aparecerá información relevante relacionada con esta librería la cual está desplegada a lo largo de las pestañas (examples, installation, etc). Como lo que deseamos es instalar esta librería para poder usar sus funciones en nuestro proyecto damos click en la pestaña **installation**:

![fig21](platformio22.jpg)

Una vez allí, se procede a copiar la parte relacionada con lib_deps en el archivo de configuración (**platformio.ini**):

![fig22](platformio23.jpg)

El archivo **platformio.ini** queda de la siguiente manera hasta el momento:

```ini
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:nodemcuv2]
platform = espressif8266
board = nodemcuv2
framework = arduino

lib_deps =
  # Using a library name
  WifiManager
    
  # ... or using library Id
  567
      
  # ... or depend on a specific version
  WifiManager@0.15.0
    
  # Semantic Versioning Rules
  # http://docs.platformio.org/page/userguide/lib/cmd_install.html#description
  WifiManager@^0.15.0
  WifiManager@~0.15.0
  WifiManager@>=0.15.0
```

Con lo anterior ya es posible usar las funciones propias de esta libreria, asi que procedemos a codificar nuestro código fuente ejemplo en el archivo ```main.cpp``` (no olvidar adaptar el **ssid** y el **password** de su red local): 

```C
#include <Arduino.h>
#include <ESP8266WiFi.h>        // Include the Wi-Fi library
#include <ESP8266WebServer.h>
#include <DNSServer.h>

const char* ssid     = "SSID";         // The SSID (name) of the Wi-Fi network you want to connect to
const char* password = "PASSWORD";     // The password of the Wi-Fi network

void setup() {
  Serial.begin(115200);         // Start the Serial communication to send messages to the computer
  delay(10);
  Serial.println('\n');

  WiFi.begin(ssid, password);             // Connect to the network
  Serial.print("Connecting to ");
  Serial.print(ssid); Serial.println(" ...");

  int i = 0;
  while (WiFi.status() != WL_CONNECTED) { // Wait for the Wi-Fi to connect
    delay(1000);
    Serial.print(++i); Serial.print(' ');
  }
  Serial.println('\n');
  Serial.println("Connection established!");  
  Serial.print("IP address:\t");
  Serial.println(WiFi.localIP());         // Send the IP address of the ESP8266 to the computer
}

void loop() {
  // put your main code here, to run repeatedly:
  
}
```

Como se podrá notar, se está haciendo uso del monitor serial por lo que es necesario agregar en el archivo de configuración (**platformio.ini**) las líneas necesarias para configurar el monitor serial:

```ini

; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:nodemcuv2]
platform = espressif8266
board = nodemcuv2
framework = arduino

lib_deps =
  # Using a library name
  WifiManager
    
  # ... or using library Id
  567
      
  # ... or depend on a specific version
  WifiManager@0.15.0
    
  # Semantic Versioning Rules
  # http://docs.platformio.org/page/userguide/lib/cmd_install.html#description
  WifiManager@^0.15.0
  WifiManager@~0.15.0
  WifiManager@>=0.15.0

; Custom Serial Monitor port
monitor_port = /dev/ttyUSB0

; Custom Serial Monitor speed (baud rate)
monitor_speed = 115200
```

Una vez hecho esto ya es posible verificar (```CTRL +ALT + b```) y subir el codigo (```CTRL +ALT + u```) de modo que si todo está bien aparecera algo como lo que se muestra en la siguiente figura:

![fig23](platformio24.jpg)

Finalmente se llama al monitor serial (```CTRL + ALT + s```) y se procede a dar un reset a la placa NodeMCU:

![fig24](platformio25.jpg)

Ya lo que resta es cacharrear y adaptar esto a su proyecto en cuestión.

## Referencias

1.	https://platformio.org/install
2.	https://medium.com/@loginov_rocks/quick-start-with-nodemcu-v3-esp8266-arduino-ecosystem-and-platformio-ide-b8415bf9a038
3.	https://openhomeautomation.net/getting-started-esp32
4.	https://awesomeopensource.com/projects/esp32
5.	https://www.youtube.com/watch?v=tQJiClU4Z3o
6.	https://www.instructables.com/id/Quick-Start-to-Nodemcu-ESP8266-on-Arduino-IDE/ 


