
##Inicializacion##
Aquí se pretende explicar la inicialización de OpenStratos.

1. Encendido de la Raspberry Pi A+
2. Root ejecuta la configuración de GPIO
3. OpenStratos arranca, y se queda a la espera del mensaje de que Root ha configurado correctamente la hora del SO.
4. Root comprueba el proceso de OpenStratos y le asigna la prioridad máxima.
5. Root arranca el GPS y se queda a la espera de una correcta configuración de la hora. Cuando la reciba, configura la fecha del sistema y el reloj de hardware a esa hora, y enviará el mensaje a OpenStratos. Mientras se inicializa, se harán sleeps de 10 segundos.
6. Root se pone en modo Failback, tendrá sleeps de 30 segundos, tras los que comprobará que OpenStratos está en funcionamiento, mediante el envío de mensajes. Si todo está correcto, hará un nuevo sleep. Si algo falla, recuperará el control de la sonda, y se pondrá en modo seguro (al final del documento).
7. OpenStratos comprueba todos los componentes en paralelo:
	7.1.Comprobación de la capacidad de escritura de ficheros.

 	7.2. Comprobación de xBee:
 
	- Comprobar conexión con xBee
   
	- Enviar SYN. Si se recibe ACK en menos de 10 segundos, pasa el test, si no el hilo queda bloqueado a la espera del cliente
 
	7.3 Comprobación del GPS: Mientras el GPS no esté inicializado, se espera 15 segundos. Una vez se detecte la inicialización, se esperarán 15 segundos más, y se comprobará la fecha y hora del sistema, la posición, altura y estabilidad de la sonda. (Cuidado con los sleeps al cambiar la hora)

	7.4 Comprobación del sistema GSM
 	
	7.5 Comprobación de la carga de todas las baterías.
 	
	7.6. Comprobación de la cámara. Se hará una pequeña grabación con una foto entre medias, se comprobará que los ficheros han sido escritos, y se borrarán.
 	
	7.7 Comprobación de los sensores de temperatura.

8. A la espera de los 7 threads, cuando todos los tests acaban se continúa con la inicialización. A partir de aquí, se enviará por cada paso un mensaje al cliente. Comenzando por un “INITIALISED”.

9. Se envía el status report al cliente, y se espera al ACK.

10. Si todo es correcto, comienza el programa principal.

##Modo seguro##

Cuando una de las Raspberry Pi entre en modo seguro, su único objetivo será la recuperación de la sonda, sin importar nada más. Comprobará el estado de la otra Raspberry Pi. Si es correcto, se pondrá en modo seguro a la espera, que es un estado en el que comprobará cada cierto tiempo si la otra Raspberry Pi sigue correctamente. No obstante, avisará a esta de que se encuentra en modo seguro. Si el proceso de OpenStratos sigue vivo, se le matará. En el caso de existir la posibilidad de abortar la misión y de aterrizar, aterrizará.

Enviará tanto por xBee como por GSM, si se está por debajo de los 3km de altura (aunque fallen), en todo momento los datos del GPS (solo posición y altura), con sleeps de 10 segundos. Una vez haya aterrizado, se pondrá en modo ahorro de energía. Se apagarán todos los componentes no necesarios. El GPS se activará una vez cada 1h para comprobar si se ha cambiado la posición. Se enviará cada 5 minutos, tanto por GSM como por xBee la posición actual de la sonda.