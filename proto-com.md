### SYN ### 
		$SYN*checksum<LF>

####Ex:###
		 $SYN*44<LF>

###ACK:### 
		$ACK*checksum<LF>

####Ex: ####
		$ACK*49<LF>

// For report ACK

###ACKR: ### 		
		$ACKR*checksum<LF>

####Ex: ####
		$ACKR*1B<LF>


// Must test if there is external power (0 = Internal, 1 = external)

###PWR: ###
		$PWR,PowerStatus(0/1)*checksum<LF>

####Ex: #####
		$PWR,0*49<LF>


###OFF: ###
		$OFF,password*checksum<LF>

####Ex: ####
		$OFF,(redacted)*16<LF>

###ERR: ###
		$ERR,error*checksum<LF>

####Ex: ####
		$ERR,1*58<LF>

###REPORT: ###		

		$REPORT,SafeMode(0/1),CPULoad(%%),FreeRAM(bbbbbbbbb),RASPTime(unixSeconds),GPSActive(0/1),GPSLat(+/-ddmm.mmmm),GPSLon(+/-ddmm.mmmm),BatRasp(%%),Bat1(%%),Bat2(%%)*checksum<LF>

####Ex: ####
		$REPORT,0,23,185213184,1421782514,1,4140.7276,-0404.8853,73,52,43*1E<LF>

###CREPORT:### 
		$CREPORT,SafeMode(0/1),CPULoad(%%.%%),CPULoad1m(x.xx),CPULoad5m(x.xx),CPULoad15m(x.xx),FreeRAM(bbbbbbbbb),RASPTime(unixSeconds.sss),GPSTime(unixSeconds.sss),GPSActive(0/1),GPSSat(0-22),GPSLat(+/-ddmm.mmmm),GPSLon(+/-ddmm.mmmm),Alt(mmmmm.m),HDOP(x.xx),VDOP(x.xx),Cam(0/1),Logs(0/1),GSM(0/1),Temp1(+/-cc.cc),Temp2(+/-cc.cc),BatRasp(%%.%%),Bat1(%%.%%),Bat2(%%.%%)*checksum<LF>

####Ex:###
		$CREPORT,0,23.13,185213184,0.23,0.21,0.22,1421782514.138,1421782514.072,1,6,4140.7276,-0404.8853,17894.3,1.12,1.98,1,1,0,-40.23,-12.49,73.24,51.92,43.42*42<LF>

###Tramas GPS:###
- GGA (Hora, Sat, Lat, Lon, HDOP, Alt)
- GSA (HDOP, VDOP)
- RMC (Hora, GPSActive, Lat, Lon, Speed, Course, Date)

Errores:

- 1: Checksum not valid
- 2: Internal error
- 3: Incorrect password
