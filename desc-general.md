##Componentes##

* Raspberry Pi Model A+

* Raspberry Pi Camera Board

* Sensor de temperatura (2 unidades)

* Adafruit Ultimate GPS

* xBee Pro 900 HP

* Shield GSM

* Micro SD clase 10 (U1) 32 GB

##Tren de vuelo##

El tren de vuelo es donde se ubican todos los instrumentos necesarios del globo. Transportará una Raspberry Pi modelo A+ sin overclockear. Tendrá acceso a todos los sensores. Tendrá una batería propia.

El tren de vuelo llevará también un xBee Pro para la comunicación con el globo durante el vuelo. Hay que tener en cuenta que la distancia máxima es de 45 km, además de las interferencias y bloqueos que puedan ocurrir (montañas, edificios...).

Por último, llevará una Shield GSM para la comunicación con el tren de vuelo una vez aterrizado, es necesario porque con el xBee Pro no se puede garantizar la cobertura a baja altura, ya que hay muchos obstáculos.

##Comandos de comunicación##


###Encendido###

Es el encendido de todos los sensores y protocolos, la carga del programa principal. Este comando requiere una contraseña. Ejemplos de uso:

		ON openstratos | 301 OK System ready

		ON balloon     | 501 ERR Incorrect password 

		ON openstratos | 502 ERR System was already on

###Estado componentes###

Se comprueba el estado de todos los componentes del tren de vuelo (Raspberry, GPS, xBee, camaras, sensores...). Si alguno de los componentes falla, se envía un array con el estado de todos los componentes. Ejemplos de uso:

		CHECK_ALL      | 302 OK Components work

		CHECK_ALL      | 503 ERR [<component1>:<work1>, <component1>:<work2>, ...]


###Estado componentes individuales###

Se comprueba el estado de cada componente individualmente. En el caso de que el componente GPS falle, el mensaje de error debe incluir la última posición GPS almacenada. 

Ejemplos de uso:

		CHECK Component | 303 OK Component working normally

		CHECK Component | 504 ERR Component Component does not exist

		CHECK Component | 505 ERR Component error: Error

###Localización###

Se utiliza para obtener la localización GPS del globo en ese momento. Se deberá ejecutar después de comprobar que funciona correctamente. Si el Ejemplos de uso:

		GPS | 305 OK Latitude, Longitude, Altitude.

		GPS | 506 ERR GPS not initialized

		GPS | 507 ERR There was an error when retrieving GPS status

###Información de los sensores###

Se utiliza para obtener la información de los sensores. En caso de que falle, debe devolver el error y además el último valor registrado. Ejemplos de uso:

		GET Sensor | 306 OK value

		GET Sensor | 508 ERR Sensor error: Error, last value: value

###Apagado###

Es el apagado de todos los sensores y protocolos, se interrumpe el programa principal. Al igual que el encendido, requiere una contraseña. Ejemplos de uso:

		OFF openstratos | 307 OK System stopped

		OFF balloon     | 509 ERR Incorrect password

		OFF openstratos | 510 ERR System was already stopped

###Funciones###

####char [] checkComponent(char[])####

Función para comprobar el estado de cualquier componente (Raspberry, GPS, xBee, GSM). La función devuelve el mensaje del apartado anterior (comandos de comunicación). Por ejemplo, si queremos comprobar el estado del GPS, utilizamos el método pasándole como parámetro: GPS. El método nos devolverá la cadena: 303 OK Component work, si funciona correctamente y 505 ERR GPS error: Error si falla alguna de las pruebas. (En el caso de comprobar el estado del GPS, si este falla, nos devolverá las últimas coordenadas almacenadas junto al error).

####int checkBattery(char[])####

Función para comprobar el porcentaje de batería que le queda a una de las baterías.

###Usuarios y contraseñas###

Se usará una contraseña distinta para cada lanzamiento. Generadas en http://randomkeygen.com/. 

(redacted)