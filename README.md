ELECTRICAL AND ELECTRONICS ENGINEERING DEPARTMENT
Lab Report
EEE 308 (1) Microprocessors
Spring 2026-2027
THIS PROJECT PREPARED BY MOHAMED OMAR OSMAN
ID / 210202987

SAQR ALFAKIH ID/210202995

	Smart Home Security System 

•	 Introduction
This project is a smart home security system that was created using many sensors and an Arduino. The system keeps an eye on temperature, humidity, gas and smoke levels, movements, and darkness. It uses a buzzer, LED, LCD screen, and Bluetooth mobile application to notify the user.
By identifying hazardous situations including gas leaks, smoke, high temperatures, and movements in the dark, the initiative increases house safety.

 
•	Project goals
o	The primary goals of this initiative are:
o	To detect motion with a PIR motion sensor.
o	To detect darkness with an LDR sensor.
o	To activate a lamp/LED when darkness is sensed.
o	To detect gas or smoke with a MQ2 sensor.
o	Use a DHT11 sensor to detect temperature and humidity.
o	To sound a buzzer in perilous situations.
o	To show sensor readings on an LCD panel.
o	Use Bluetooth to communicate sensor data to a mobile phone.
o	To operate the system using the Serial Bluetooth Terminal app.

•	 Components Used

•	
Component	Function
Arduino	Main controller of the system
PIR Motion Sensor	Detects movement
LDR Sensor	Detects light/darkness
MQ2 Gas Sensor	Detects gas and smoke
DHT11 Sensor	Measures temperature and humidity
Buzzer	Gives alarm sound
LED / Lamp	Indicates danger or darkness
LCD I2C
Screen	Displays system data
Bluetooth Module	Sends data to phone and receives commands
Jumper Wires	Connections
Breadboard	Circuit testing


•	 System Description.
o	The smart security system continually reads data from the linked sensors. The Arduino evaluates the readings and determines if the condition is safe,
 
warning, or dangerous.
o	The system contains three major states:
1.	SAFE State.
o	The system is normal. There is no threat identified. The LCD screen shows temperature, humidity, gas level, light level, and motion status.
2.	Warning State
o	The gas level or temperature is near to the danger zone. The LED goes on, but the buzzer stays off.
3.	ALERT State.
o	A serious situation has been found. The buzzer sounds unless it is silenced in the Bluetooth app.
o	Alerts occur when:
o	The gas level is high.
o	The temperature is high.
o	Motion is sensed in the dark.


•	. Sensors and Functions
PIR motion sensor
The PIR sensor detects human movement. In this project, motion becomes harmful when it occurs in the darkness.
LDR darkness sensor
The LDR sensor senses the light level. If the light value is less than the threshold, the system considers the environment dark.
MQ2 Gas and Smoke Sensor
The MQ2 sensor can detect gas or smoke. If the reading equals or exceeds the gas limit, the system enters alarm mode.
The DHT11 temperature and humidity sensor
Every 2 seconds, the DHT11 sensor measures the temperature and humidity. If the temperature rises above the set point, the buzzer sounds.
Threshold values used. Gas Limit: 350
The temperature limit is 30°C. Light Limit: 400
 
•	Bluetooth Application Control
o	The system is linked to the Serial Bluetooth Terminal app.
o	The user can operate the system with numbers:
Number	Function
0	Turns off the entire
system
1	Restarts the system
2	Turns off / mutes the
buzzer
3	Turns on the buzzer
again


•	LCD Display.

o	Every two seconds, the LCD panel displays a different page.
o	Page 1 shows the temperature and humidity.
o	Page 2 shows gas and light readings.
o	Page 3 shows motion status and safety mode.
o	In alert mode, the screen indicates the danger kind.




•	Conclusion.
This project effectively installs an Arduino-based smart home security and monitoring system. It can detect motion, darkness, gas/smoke, temperature, and humidity. The system notifies the user via a buzzer, LED, LCD screen, and Bluetooth mobile app.

The Bluetooth control makes the system more interactive by allowing the user to switch it off, restart it, silence the alarm, or activate it again. Overall, this project is helpful, practical, and applicable to home safety applications.

