
##Inicializacion##
This document explains the OpenStratos initialization protocol.

1. The Raspberry Pi A+ flight computer is turned on.
2. Root executes GPIO initialization program.
3. OpenStratos starts and waits for root to acknowledge that the OS time was properly configured.
4. Root tests the OpenStratos process and gives it maximum priority.
5. Root starts the GPS module and waits for the time to be received via GPS. When the time is received, system time and hardware clock are set and OpenStratos is acknowledged. While initialization is not complete, the process will sleep for periods of ten seconds.
6. Root enters Failback mode. In said mode, root will sleep for 30 seconds and then send a query message to the OpenStratos process to check that is working correctly. If everything is working correctly, root will sleep again (step 6). If something is wrong root will regain control of the balloon and enter failsafe mode (see end of the document).
7. OpenStratos tests all the components in parallel:
	7.1.Test ability to read and write files 

 	7.2. xBee tests:
 
	- Test xBee connection

	- Send SYN. If an ACK is received within 10 seconds, the test is passed. If not, the thread stays blocked and waits for the client.
   
	7.3 GPS test: While the GPS module is not initialized, wait in periods of 15 seconds. When initialization is detected, wait for 15 more seconds and check system time, position, altitude and stability. (need to be careful with sleep calls after system time is changed)

	7.4 GSM module test 

	7.5 Battery charge test

	7.6 Camera test: A short recording will be made and a single picture will be taken. The correctness of the write operation will be tested and the new files will be deleted. 
 	
	7.7 Temperature sensors test

8. When all seven threads finish, continue initializating. From now on, a message will be sent to the client each time a step is completed, starting with INITIALISED.

9. Send status report to the client and wait for ACK

10. If everything is OK, execute main program.

##Failsafe mode##

When one of the Raspberry Pi computers enters failsafe mode, its only goal will be the recovery of the OpenStratos probe. It will probe the other Raspberry Pi for its status, and if everything is correct, it will enter waiting failsafe mode (where it will check whether the other flight computer is working correctly every defined period of time and acknowledge that it has entered failsafe mode). If the OpenStratos process is still running, it will be killed. If the mission can be aborted by triggering landing, so will happen.

The GPS position of the probe will be transmitted via xBee and GSM (if height is below 3km) even if the modules are failing.
N. de T.: Original: Enviará tanto por xBee como por GSM, si se está por debajo de los 3km de altura (aunque fallen), en todo momento los datos del GPS (solo posición y altura), con sleeps de 10 segundos. Una vez haya aterrizado, se pondrá en modo ahorro de energía. Se apagarán todos los componentes no necesarios. El GPS se activará una vez cada 1h para comprobar si se ha cambiado la posición. Se enviará cada 5 minutos, tanto por GSM como por xBee la posición actual de la sonda.
