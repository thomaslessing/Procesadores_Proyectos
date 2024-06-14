# Pràctica 6 - BUSES DE COMUNICACIÓN II

Aquest codi té com a objectiu demostrar el funcionament del bus SPI i la lectura d'un fitxer des de una targeta SD utilitzant aquest bus.

Es necessiten dues llibreries: "SPI.h" per gestionar el bus SPI i "SD.h" per interactuar amb la targeta SD. S'importen aquestes llibreries al principi del codi.
```cpp
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
```

Es declara un objecte de tipus "File" anomenat "myFile". Aquest objecte s'utilitzarà per accedir i manipular el fitxer de la targeta SD.
```cpp
File myFile;
```


A la funció setup(), s'inicialitza la comunicació sèrie (Serial) per mostrar informació per la terminal. A continuació, es prova d'inicialitzar la targeta SD amb SD.begin(5). Si la inicialització és exitosa, es mostra el missatge "Inici exitós". Si no es pot inicialitzar la targeta SD, es mostra el missatge "No s'ha pogut iniciar" i es surt de la funció setup().
```cpp
void setup()
{
  Serial.begin(115200);
  Serial.print("Iniciant SD");
  if (!SD.begin(5)) {
    Serial.println("No s'ha pogut iniciar");
    return;
  }
  Serial.println("Inici exitós");
```

Després de la inicialització de la targeta SD, es crida a SD.open("/arxiu.txt") per obrir el fitxer de text "archivo.txt" de la targeta SD i es guarda a l'objecte "myFile". A continuació, es verifica si s'ha pogut obrir correctament el fitxer amb una condició if.
Si el fitxer s'ha pogut obrir amb èxit, es mostra el missatge "arxiu.txt:" i es llegeix el contingut del fitxer utilitzant un bucle while. El contingut es llegeix byte a byte amb myFile.read() i es mostra per la comunicació sèrie amb Serial.write(). Finalment, es tanca el fitxer amb myFile.close().
Si no es pot obrir el fitxer, es mostra el missatge "Error en obrir l'arxiu".
```cpp
myFile = SD.open("/arxiu.txt");  if (myFile) {
    Serial.println("arxiu.txt:");
    while (myFile.available()) {
    	Serial.write(myFile.read());
    }
    myFile.close();   } else {
    Serial.println("Error en obrir l'arxiu");
  }
}
```

A la funció loop(), no hi ha cap codi ja que no es realitza cap acció repetitiva.
Aquest codi demostra com utilitzar el bus SPI per interactuar amb una targeta SD i llegir un fitxer de text des de la mateixa. És una introducció al funcionament del bus SPI i la lectura de fitxers amb una targeta SD

