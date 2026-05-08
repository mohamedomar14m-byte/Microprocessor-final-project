# Microprocessor-final-project


#include <Wire.h>
#include <LiquidCrystal_I2C.h> #include <SoftwareSerial.h> #include <DHT.h>
// ===== LCD =====
LiquidCrystal_I2C lcd(0x27, 16, 2);
// ===== Bluetooth ===== SoftwareSerial BT(10, 11);
// ===== DHT =====
#define DHTPIN 4
#define DHTTYPE DHT11 DHT dht(DHTPIN, DHTTYPE);
// ===== Pins ===== const int LDR = A0; const int GAS = A1; const int PIR = 2; const int BUZZER = 9; const int LED = 13;
// ===== State =====
enum State { SAFE, WARNING, ALERT };
State currentState = SAFE;
// ===== Variables ===== float temp = 0;
float hum = 0;
unsigned long lastDHT = 0; unsigned long lastScreen = 0;
int screenPage = 0;
// ===== Thresholds ===== int GAS_LIMIT = 350;
int TEMP_LIMIT = 30; int LIGHT_LIMIT = 400;
// ===== Bluetooth Control ===== bool systemEnabled = true;
bool alarmMuted = false;
// ===== Setup ===== void setup() { Serial.begin(9600); BT.begin(9600);
lcd.init(); lcd.backlight(); dht.begin();
pinMode(PIR, INPUT); pinMode(LED, OUTPUT); pinMode(BUZZER, OUTPUT);
lcd.print("SMART SYSTEM"); delay(1500);
lcd.clear();
}
// ===== Loop ===== void loop() {
// ===== Bluetooth Commands ===== if (BT.available()) {
char cmd = BT.read();
if (cmd == '0') { systemEnabled = false; BT.println("SYSTEM OFF");
}
else if (cmd == '1') { BT.println("RESET..."); delay(200);
void(* resetFunc) (void) = 0; resetFunc();
}
else if (cmd == '2') { alarmMuted = true; BT.println("ALARM MUTED");
}
else if (cmd == '3') { alarmMuted = false; BT.println("ALARM ON");
}
}
// ===== System OFF ===== if (!systemEnabled) { lcd.clear(); lcd.setCursor(0,0); lcd.print("SYSTEM OFF");
digitalWrite(LED, LOW); digitalWrite(BUZZER, LOW);
delay(500); return;
}
unsigned long now = millis();
// ===== DHT =====
if (now - lastDHT >= 2000) { lastDHT = now;
float t = dht.readTemperature(); float h = dht.readHumidity();
if (!isnan(t) && !isnan(h)) { temp = t;
hum = h;
}
}
// ===== Sensors =====
int gas = analogRead(GAS); int light = analogRead(LDR); int motion = digitalRead(PIR);
// ===== Conditions =====
bool gasAlert = gas >= GAS_LIMIT; bool tempAlert = temp >= TEMP_LIMIT; bool lightAlert = light < LIGHT_LIMIT; bool motionAlert = motion == HIGH;
// ===== State Logic =====
if (gasAlert || tempAlert || (motionAlert && lightAlert)) { currentState = ALERT;
}
else if (gas >= GAS_LIMIT - 50 || temp >= TEMP_LIMIT - 3) { currentState = WARNING;
}
else {
currentState = SAFE;
}
// ===== Screen Pages ===== if (now - lastScreen >= 2000) { screenPage++;
if (screenPage > 2) screenPage = 0; lastScreen = now;
}
lcd.clear();
// ===== ALERT =====
if (currentState == ALERT) { lcd.setCursor(0, 0); lcd.print("!!! ALERT !!!");
lcd.setCursor(0, 1);
if (gasAlert && tempAlert) { lcd.print("Gas+Temp High");
}
else if (gasAlert) { lcd.print("Gas High");
}
else if (tempAlert) { lcd.print("Temp High");
}
else { lcd.print("Motion+Dark");
}
digitalWrite(LED, HIGH); if (!alarmMuted) {
digitalWrite(BUZZER, HIGH);
} else {
digitalWrite(BUZZER, LOW);
}
}
// ===== WARNING =====
else if (currentState == WARNING) { lcd.setCursor(0, 0); lcd.print("WARNING");
lcd.setCursor(0, 1);
if (gas >= GAS_LIMIT - 50 && temp >= TEMP_LIMIT - 3) { lcd.print("Gas+Temp Rise");
}
else if (gas >= GAS_LIMIT - 50) { lcd.print("Gas Rising");
}
else {
lcd.print("Temp Rising");
}
digitalWrite(LED, HIGH); digitalWrite(BUZZER, LOW);
}
// ===== SAFE =====
else {
digitalWrite(LED, LOW); digitalWrite(BUZZER, LOW);
if (screenPage == 0) { lcd.setCursor(0, 0); lcd.print("Temp:"); lcd.print(temp, 1);
lcd.setCursor(0, 1); lcd.print("Hum:"); lcd.print(hum, 0);
}
else if (screenPage == 1) { lcd.setCursor(0, 0); lcd.print("Gas:"); lcd.print(gas);
lcd.setCursor(0, 1); lcd.print("Light:"); lcd.print(light);
}
else { lcd.setCursor(0, 0); lcd.print("Motion:"); lcd.print(motion);
lcd.setCursor(0, 1); lcd.print("SAFE MODE");
}
}
// ===== Bluetooth Data ===== BT.print("State:"); BT.print(currentState); BT.print(",Gas:"); BT.print(gas); BT.print(",Temp:"); BT.print(temp); BT.print(",Light:"); BT.print(light); BT.print(",Motion:"); BT.println(motion);
delay(3000);
}#include <Wire.h>
#include <LiquidCrystal_I2C.h> #include <SoftwareSerial.h> #include <DHT.h>
// ===== LCD =====
LiquidCrystal_I2C lcd(0x27, 16, 2);
// ===== Bluetooth ===== SoftwareSerial BT(10, 11);
// ===== DHT =====
#define DHTPIN 4
#define DHTTYPE DHT11 DHT dht(DHTPIN, DHTTYPE);
// ===== Pins ===== const int LDR = A0; const int GAS = A1; const int PIR = 2; const int BUZZER = 9; const int LED = 13;
// ===== State =====
enum State { SAFE, WARNING, ALERT };
State currentState = SAFE;
// ===== Variables ===== float temp = 0;
float hum = 0;
unsigned long lastDHT = 0; unsigned long lastScreen = 0;
int screenPage = 0;
// ===== Thresholds ===== int GAS_LIMIT = 350;
int TEMP_LIMIT = 30; int LIGHT_LIMIT = 400;
// ===== Bluetooth Control ===== bool systemEnabled = true;
bool alarmMuted = false;
// ===== Setup ===== void setup() { Serial.begin(9600); BT.begin(9600);
lcd.init(); lcd.backlight(); dht.begin();
pinMode(PIR, INPUT); pinMode(LED, OUTPUT); pinMode(BUZZER, OUTPUT);
lcd.print("SMART SYSTEM"); delay(1500);
lcd.clear();
}
// ===== Loop ===== void loop() {
// ===== Bluetooth Commands ===== if (BT.available()) {
char cmd = BT.read();
if (cmd == '0') { systemEnabled = false; BT.println("SYSTEM OFF");
}
else if (cmd == '1') { BT.println("RESET..."); delay(200);
void(* resetFunc) (void) = 0; resetFunc();
}
else if (cmd == '2') { alarmMuted = true; BT.println("ALARM MUTED");
}
else if (cmd == '3') { alarmMuted = false; BT.println("ALARM ON");
}
}
// ===== System OFF ===== if (!systemEnabled) { lcd.clear(); lcd.setCursor(0,0); lcd.print("SYSTEM OFF");
digitalWrite(LED, LOW); digitalWrite(BUZZER, LOW);
delay(500); return;
}
unsigned long now = millis();
// ===== DHT =====
if (now - lastDHT >= 2000) { lastDHT = now;
float t = dht.readTemperature(); float h = dht.readHumidity();
if (!isnan(t) && !isnan(h)) { temp = t;
hum = h;
}
}
// ===== Sensors =====
int gas = analogRead(GAS); int light = analogRead(LDR); int motion = digitalRead(PIR);
// ===== Conditions =====
bool gasAlert = gas >= GAS_LIMIT; bool tempAlert = temp >= TEMP_LIMIT; bool lightAlert = light < LIGHT_LIMIT; bool motionAlert = motion == HIGH;
// ===== State Logic =====
if (gasAlert || tempAlert || (motionAlert && lightAlert)) { currentState = ALERT;
}
else if (gas >= GAS_LIMIT - 50 || temp >= TEMP_LIMIT - 3) { currentState = WARNING;
}
else {
currentState = SAFE;
}
// ===== Screen Pages ===== if (now - lastScreen >= 2000) { screenPage++;
if (screenPage > 2) screenPage = 0; lastScreen = now;
}
lcd.clear();
// ===== ALERT =====
if (currentState == ALERT) { lcd.setCursor(0, 0); lcd.print("!!! ALERT !!!");
lcd.setCursor(0, 1);
if (gasAlert && tempAlert) { lcd.print("Gas+Temp High");
}
else if (gasAlert) { lcd.print("Gas High");
}
else if (tempAlert) { lcd.print("Temp High");
}
else { lcd.print("Motion+Dark");
}
digitalWrite(LED, HIGH); if (!alarmMuted) {
digitalWrite(BUZZER, HIGH);
} else {
digitalWrite(BUZZER, LOW);
}
}
// ===== WARNING =====
else if (currentState == WARNING) { lcd.setCursor(0, 0); lcd.print("WARNING");
lcd.setCursor(0, 1);
if (gas >= GAS_LIMIT - 50 && temp >= TEMP_LIMIT - 3) { lcd.print("Gas+Temp Rise");
}
else if (gas >= GAS_LIMIT - 50) { lcd.print("Gas Rising");
}
else {
lcd.print("Temp Rising");
}
digitalWrite(LED, HIGH); digitalWrite(BUZZER, LOW);
}
// ===== SAFE =====
else {
digitalWrite(LED, LOW); digitalWrite(BUZZER, LOW);
if (screenPage == 0) { lcd.setCursor(0, 0); lcd.print("Temp:"); lcd.print(temp, 1);
lcd.setCursor(0, 1); lcd.print("Hum:"); lcd.print(hum, 0);
}
else if (screenPage == 1) { lcd.setCursor(0, 0); lcd.print("Gas:"); lcd.print(gas);
lcd.setCursor(0, 1); lcd.print("Light:"); lcd.print(light);
}
else { lcd.setCursor(0, 0); lcd.print("Motion:"); lcd.print(motion);
lcd.setCursor(0, 1); lcd.print("SAFE MODE");
}
}
// ===== Bluetooth Data ===== BT.print("State:"); BT.print(currentState); BT.print(",Gas:"); BT.print(gas); BT.print(",Temp:"); BT.print(temp); BT.print(",Light:"); BT.print(light); BT.print(",Motion:"); BT.println(motion);
delay(3000);
}
