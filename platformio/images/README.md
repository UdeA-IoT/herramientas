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

![fig9](platformio10.jpg)

5. Finalmente una vez cargado el programa, se verá el led titilando.