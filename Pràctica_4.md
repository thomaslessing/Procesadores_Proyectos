# Pràctica 3 - WIFI i BLUETOOTH

## 3a - WIFI

En aquest codi es realitza la configuració d'un servidor web utilitzant el mòdul WiFi de l'ESP32 per crear una pàgina web senzilla. Aquí s'explica el funcionament del codi:
S'inclouen les llibreries necessàries, com ara "WiFi.h" per connectar-se a la xarxa WiFi i "WebServer.h" per crear i gestionar el servidor web.

```cpp
#include <WiFi.h>
#include <WebServer.h>
```

Es defineixen el nom de la xarxa WiFi (SSID) i la contrasenya associada.

```cpp
const char* ssid = "Redmi Note 8 Pro"; // Enter your SSID here
const char* password = "bhvgdgrxsfrh583"; //Enter your Password here
```

Es crea un objecte del servidor web amb el número de port 80 (port HTTP per defecte).

```cpp
WebServer server(80);
```
En la funció setup(), s'inicialitzen les comunicacions sèrie, es connecta l'ESP32 a la xarxa WiFi especificada i es comprova si s'ha establert la connexió WiFi amb èxit. A continuació, s'inicia el servidor web i s'espera un breu retard.
```cpp 
void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);

// Connect to your wi-fi modem
WiFi.begin(ssid, password);
// Check wi-fi is connected to wi-fi network
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}
```


En la funció loop(), es gestiona la comunicació amb els clients del servidor web.

```cpp
void loop() {
server.handleClient();
}
```






A continuació, es defineix el contingut de la pàgina web com una cadena de caràcters en format HTML. En aquest cas, es mostra un encapçalament amb el text "Pràctica 3"

```cpp
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1> Pràctica 3. ;</h1>\
</body>\
</html>";
```

Finalment, es defineix una funció anomenada handle_root() que s'executa quan es demana la ruta de l'arrel ("/") al servidor web. Aquesta funció envia una resposta HTTP amb l'encodat "text/html" i el contingut de la pàgina web.
```cpp 
void handle_root() {
server.send(200, "text/html", HTML);
}
```
## Conclusions Pràctica 3a

Amb aquest codi, es crea un servidor web bàsic en l'ESP32 que mostra una pàgina web senzilla quan es fa una petició a l'adreça IP de l'ESP32. El contingut de la pàgina web es defineix amb l'ús de codi HTML i es pot personalitzar segons les necessitats de l'usuari.















# 3b - BLUETOOTH

En aquest codi es mostra com establir una connexió Bluetooth mitjançant l'ús de la llibreria "BluetoothSerial.h" a l'ESP32. A continuació s'explica com funciona el codi:
S'inclou la llibreria "BluetoothSerial.h" que permet l'ús de les funcionalitats Bluetooth a l'ESP32.
```cpp 
#include <BluetoothSerial.h>
```
Es crea un objecte de tipus BluetoothSerial anomenat SerialBT, que serà utilitzat per interactuar amb el dispositiu Bluetooth.
```cpp 
BluetoothSerial SerialBT;
```
A la funció setup(), s'inicialitza la comunicació sèrie i es comença el funcionament del Bluetooth amb el nom "ESP32test". A continuació, s'imprimeix un missatge indicant que el dispositiu està llest per a l'emparellament Bluetooth.
```cpp 
void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32test"); //Bluetooth device name
  Serial.println("The device started, now you can pair it with bluetooth!");
}
```


A la funció loop(), es comprova si hi ha dades disponibles en el port sèrie. Si hi ha dades disponibles des de l'entrada sèrie (Serial.available()), aquestes dades són enviades a través de la connexió Bluetooth (SerialBT.write()). I si hi ha dades disponibles des de la connexió Bluetooth (SerialBT.available()), aquestes dades són enviades a través del port sèrie (Serial.write()). Es realitza un petit retard de 20 ms amb delay(20) per evitar un consum excessiu de recursos de la CPU.
```cpp 
void loop() {
  if (Serial.available()) {
    SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
    Serial.write(SerialBT.read());
  }
  delay(20);
}
```
## Conclusions Pràctica 3b

Amb aquest codi, es permet establir una connexió Bluetooth amb altres dispositius i intercanviar missatges entre ells. Les dades rebudes pel port sèrie són enviades a través de la connexió Bluetooth i les dades rebudes a través del Bluetooth es mostren pel port sèrie. Això permet una comunicació bidireccional entre l'ESP32 i altres dispositius amb connexió Bluetooth.

