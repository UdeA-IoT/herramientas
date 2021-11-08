


Siga las instrucciones de https://github.com/esp8266/Arduino

1. Enter https://arduino.esp8266.com/stable/package_esp8266com_index.json into the File>Preferences>Additional Boards Manager URLs field of the Arduino IDE. You can add multiple URLs,

https://arduino.esp8266.com/stable/package_esp8266com_index.json


* instalacion1
* instalacion2
* instalacion3
* targeta1
* targeta2


Ejemplo:

* ejemplo1
* ejemplo2
  


Configuración en el IDE:

Tools > Board: NodeMCU1.0(ESP-12E Module)
Upload Speed > 115200
CPU Frequency > 80 MHz
Flash Size: 4M (no SPIFFS)


Centrándonos en los experimentos que emplean pines digitales tenemos la siguiente figura:

* ejemplo3

Tengase en cuenta que el nombre de los pines de NodeMCU tiene una nomenclatura diferente de la que utiliza el ESP8266. Por ejemplo los pines D0, D1, y D2 en la NodeMCU correponden con GPIO16, 5 y 4 en el ESP8266. El framework para Arduino permite utilizar ambas para referirse a los pines de entrada salida. Estas dos sentencias son equivalentes.

> **Acceso a los pines con la nomenclatura de NodeMCU**
> ```C
> pinMode(D2, OUTPUT);
> digitalWrite(D2,HIGH);
> ```



> **Acceso a los pines con la nomenclatura de ESP8266**
> ```C
> pinMode(4, OUTPUT);
> digitalWrite(4,HIGH);
> ```

## Ejemplos

### Ejemplo 1


* **Nota**: Este ejemplo se tomo del siguiente [enlace](https://programarfacil.com/esp8266/como-programar-nodemcu-ide-arduino/)





## Referencias

1. https://www.arduino.cc/en/guide/libraries
2. https://arduino-esp8266.readthedocs.io/en/3.0.2/
3. https://github.com/esp8266/Arduino
4. https://naylampmechatronics.com/blog/56_usando-esp8266-con-el-ide-de-arduino.html
5. https://www.luisllamas.es/programar-esp8266-con-el-ide-de-arduino/
6. https://www.prometec.net/esp8266-pluggin-arduino-ide/
7. http://robots-argentina.com.ar/didactica/programar-esp8266-desde-el-ide-arduino-y-con-sus-librerias/
