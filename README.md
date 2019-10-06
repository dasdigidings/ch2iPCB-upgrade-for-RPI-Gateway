# TTN LoRaWAN Gateway upgrade 
  
suitable for | RaspberryPi3b+ | ch2i | IMST-iC880a-SPI |  

how to upgrade TTN Gateway with BME280 sensor and digital voltmeter  

this will be a step by step description how to upgrade your ttn lorawan gateway with environmental sensor (BME280) and optional digital voltmeter  
**Parts needed for upgrading the gateway ch2i board with sensor and voltmeter**
•	1x GY-BME280 3.3V precision altimeter atmospheric pressure BME280 sensor module
•	1x Balai 0.28 Zoll 2.5V-30V LED Bildschirm Mini Digital Spannung Circuit Tester Meter Zubehör für elektronische Teile Digitaler Voltmeter
•	Optional 1x 4p female connector for I2C Bus

**Preparation**
•	Shutdown power-supply from gateway, remove power supply and antenna-connections
•	Open your station-box (or the housing from your system)
•	Disconnect any wiring from your device, also the pigtail antenna from the IMST-IC880A board and the LAN-Cable from the RaspberryPi3, power supply from PCB-board
•	Remove the complete gateway-unit out of the housing
•	Seperate the IMST-IC880A from the PCB --- be careful as this concentrator board is EMD sensitive (and expensive, too) – discharge yourself before touching it or use ESD equipment
•	Remove the screws that are fixing the PCB with the RaspberryPi3 to disconnect the two boards

**Gateway upgrade (Hardware)**
After all preparation were done we can start with the upgrade steps

*VOLTMETER*
With hot-glue you can mount the voltmeter display on to the upper-left corner of the PCB board, just see my sample picture. The wiring can be placed below the PCB by using on oft he prepared holes

Next step is to solder the voltmeter-cables to the power-supply connectors from the PCB from backside ! Ist located next tot he DC barrel connector, marked with „DC input“ Red cable is soldered to the DC input VIN (voltage in) and Black cable is soldered to the DC input GND. You will be able to measure the input-voltage now

*BME280 Sensor*
Within the delivery of the sensor is a 6-male connector  - we will only use 4-males of this to solder it from the backside to the sensor. So cut it to four pins . Solder it to 3V3, GND, SCL, SDA connectors from the sensor (CSB, SD0 are not used). Next step is to solder the 4p female connector on the PCB board – use one of the I2C Bus connectors from your choice (keep in mind not to cover some screws when the sensor is installed). I used the first position from the I2C bus on the upper middle part from the board.  Now you can easyly plug in the sensor module on the I2C bus (watch for the correct pin orientation of 3V3, GND, SCL, SDA). 

**Remark: **
it is also possible to use some dupont-cables (jumper cables male/female) to place the sensor somewhere else inside of your station box if needed.  Just plug this extensions between board and sensor if you want to.

After all hardware upgrades are finished you can remount all parts together. Just reverse the description mentioned before

•	Place the PCB board on top of the RapsberryPi3 – fix it with the screws as before
•	Place the concentrator Board carefully on top of the PCB board
•	Remount the Gateway unit inside of the housing
•	Reconnect all wiring including antenna   (never power the device without antenna !!)
•	Power it up again and wait some minutes until it will be online in balena.cloud

**Gateway upgrade (Software)**
After all hardware upgrades are finished we have to do some changes in the balena.cloud to activate the Sensor module BME280. 

Therefor login to your balena account with your personal credentials, open the Application where your gateway is located and then open your gateway to reconfigure. 

Select the „Device Service Variables“  from the left side-bar. Click on the blue colored button to add new variables.  Now you have to configure following variables with values (each time just repeat the step „add new variable“):

Service:	Variable.Name:			Variable.Value: 
collectd		GW_BME280				true
collectd		GW_BME280_ADDR			0x76
collectd		GW_BME280_SMBUS			1

optional (only if your gateway should send data to influx-Database or similar)
collectd		GW_COLLECTD_SERVER	xxx.xxx.xxx.xxx

(this collectd servers credentials must be configured in /ttn-gateway-containers/collectd/collectd.conf.d/network.conf  file  ---- 

Now the configuration is done and the gateway must be restarted to activate changes. Just click on the „restart“ button located in the device summary screen. Some minutes later the gateway should be restarted and active again – in the summary screen you can see that gateway and collectd are marked as „running“

We can test the sensor measuring environmental data directly from balena.cloud very easy:

From the summary screen start a new terminal session by selecting „collectd“ as target

Start terminal session
Some seconds later you will be connecte****d to your device shell
Type `cd collectd-python-plugins` and ENTER to change the folder
Type `python bme280.py` and ENTER again to execute the python command

`Temperature: 34.63°C` 
`Pressure: 1008.74hPa` 
`Humidity: 19.75%`

***Congratulations – your sensor is installed properly and reading data ! 
To finish all the works for this upgrade check all wiring inside of your gateway again and close the box. Install the gateway on its original location and restart power connection.
***

<!--Document Version 1.2 from 06th October 2019-->
