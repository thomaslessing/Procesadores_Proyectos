# Pràctica 5 - BUSES DE COMUNICACIÓN I

## 5a

En aquest codi es presenta un exemple d'ús del bus I2C per identificar els dispositius connectats a aquest bus. A continuació s'explica com funciona el codi:

S'inclou la llibreria "Wire.h", que proporciona les funcions necessàries per interactuar amb el bus I2C.
```cpp
#include <Arduino.h>
#include <Wire.h>
``` 
A la funció setup(), es comença la comunicació I2C mitjançant la crida a Wire.begin(). A més, s'inicialitza la comunicació sèrie per a la sortida de resultats.
```cpp 
void setup()
{
  Wire.begin();
 
  Serial.begin(115200);
  while (!Serial);
  Serial.println("\nI2C Scanner");
}
```

A la funció loop(), s'escaneja el bus I2C per identificar els dispositius connectats. Es realitza un bucle des de l'adreça 1 fins a l'adreça 127 (que són les adreces possibles dels dispositius I2C). Per a cada adreça, s'intenta establir una comunicació amb el dispositiu mitjançant la crida a Wire.beginTransmission(address) i Wire.endTransmission(). Si no hi ha hagut cap error en la comunicació, es considera que hi ha un dispositiu connectat i es mostra l'adreça per la comunicació sèrie.
```cpp
void loop()
{
  byte error, address;
  int nDevices;
 
  Serial.println("Scanning...");
 
  nDevices = 0;
  for(address = 1; address < 127; address++ )
  { Wire.beginTransmission(address);
    error = Wire.endTransmission();
 
    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
 
      nDevices++;
    }
    else if (error==4)
    {
      Serial.print("Unknown error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }    
  }
  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");
 
  delay(5000); // wait 5 seconds for next scan
  ```
## Conclusions Pràctica 5a

Amb aquest codi, s'escaneja el bus I2C per identificar els dispositius connectats i es mostra la seva adreça per la comunicació sèrie. Això permet conèixer quins dispositius estan connectats al bus I2C i facilita el desenvolupament de sistemes que interactuen amb aquests dispositius. Aquesta és només una part de la pràctica que explora el bus I2C, però esmenta que hi ha altres busos sistemes de comunicació com SPI, I2S i USART, que també es poden utilitzar per connectar perifèrics interns o externs al processador.



# 5b

Aquest codi és una extensió del codi anterior que utilitza la llibreria "Adafruit_SSD1306" per controlar una pantalla OLED (SSD1306) mitjançant el bus I2C.

S'incorporen dues noves llibreries al codi: "Adafruit_GFX.h" i "Adafruit_SSD1306.h". Aquestes llibreries permeten la comunicació i el control de la pantalla OLED.
```cpp
#include <Arduino.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
```

Es defineixen les dimensions de la pantalla OLED utilitzant les constants "ancho" (ample) i "alto" (alt). En aquest cas, la pantalla té una resolució de 128x64 píxels.
```cpp
#define ancho 128
#define alto 64
```

Es crea un objecte "display" de la classe "Adafruit_SSD1306" utilitzant les dimensions definides anteriorment i especificant el bus I2C utilitzant l'objecte Wire.
```cpp
Adafruit_SSD1306 display (ancho, alto, &Wire, -1)
```

A la funció setup(), es comença la comunicació I2C i s'inicialitza la comunicació sèrie, igual que en el codi anterior. A més, s'inicialitza la pantalla OLED cridant a display.begin(SSD1306_SWITCHCAPVCC, 0x3C). Els paràmetres indiquen el tipus de connexió de la pantalla i l'adreça I2C de la pantalla.
```cpp
void setup()
{
  Wire.begin();
 
  Serial.begin(115200);
  while (!Serial); 
  Serial.println("\nI2C Scanner");

  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
}
```

A la funció loop(), s'escaneja el bus I2C com en el codi anterior per identificar els dispositius connectats. Després d'això, es borra el contingut de la pantalla OLED mitjançant display.clearDisplay() i s'escriu un text a la pantalla amb display.println(). Finalment, es fa una crida a display.display() per actualitzar la pantalla i mostrar el text.
  

```cpp
void loop()
{
  // ...
display.clearDisplay();
display.setTextSize(1);
  // Color del text
  display.setTextColor(SSD1306_WHITE);
  // Posició del text
  display.setCursor(10, 32);
  // Escriure text
  display.println("GERARD I FERRAN PRACTICA 5");
  // Enviar a pantalla
  display.display();
}
```

## Conclusions Pràctica 5b

Amb aquesta extensió del codi, s'afegeix la funcionalitat per controlar una pantalla OLED utilitzant el bus I2C. El text "GERARD I FERRAN PRACTICA 5" s'escriu a la pantalla OLED i s'actualitza cada vegada que es repeteix el bucle. Aquest exemple demostra com utilitzar el bus I2C per interactuar amb perifèrics com una pantalla OLED.

