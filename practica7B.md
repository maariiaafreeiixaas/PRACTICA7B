# Pràctica 7

## Practica 7B: Reproduir un Fitxer WAVE en ESP32 des d'una Targeta SD Externa

### Objectiu

L'objectiu d'aquest exercici és reproduir un fitxer WAVE des d'una targeta SD utilitzant l'ESP32 i enviar la senyal d'àudio digital al descodificador MAX98357A per a la seva conversió a senyal analògica que es pot escoltar a través d'un altaveu.

### Codi

```cpp
#include "Audio.h"  //llibreria per gestionar l'àudio
#include "SD.h"     //llibreria per gestionar la targeta SD
#include "FS.h"     //llibreria per gestionar el sistema de fitxers

// Pins digitals utilitzats
#define SD_CS 5         
#define SPI_MOSI 23     
#define SPI_MISO 19     
#define SPI_SCK 18      
#define I2S_DOUT 25     
#define I2S_BCLK 27     
#define I2S_LRC 26      

Audio audio;  //objecte de tipus Audio per gestionar la reproducció

void setup() { 
    pinMode(SD_CS, OUTPUT);            //Configura el pin SD_CS com a sortida
    digitalWrite(SD_CS, HIGH);         //Estableix el pin SD_CS a nivell alt (desactiva la targeta SD)
    //Inicialitzacions
    SPI.begin(SPI_SCK, SPI_MISO, SPI_MOSI); 
    Serial.begin(115200);            
    SD.begin(SD_CS);                  
    audio.setPinout(I2S_BCLK, I2S_LRC, I2S_DOUT); //Configura els pins per a la sortida I2S
    audio.setVolume(10); //Configura el volum de la sortida d'àudio
    audio.connecttoFS(SD, "Ensoniq-ZR-76-01-Dope-77.wav"); //Connecta el sistema de fitxers SD i el fitxer WAVE
} 

void loop() { 
    audio.loop();  //manté la reproducció d'àudio en funcionament
} 

// Funcions opcionals per obtenir informació d'àudio
void audio_info(const char *info) { 
    Serial.print("info        "); 
    Serial.println(info); 
}

void audio_id3data(const char *info) { 
    Serial.print("id3data     ");
    Serial.println(info); 
}

void audio_eof_mp3(const char *info) {  //Fi del fitxer
    Serial.print("eof_mp3     ");
    Serial.println(info); 
}

void audio_showstation(const char *info) { 
    Serial.print("station     ");
    Serial.println(info); 
}

void audio_showstreaminfo(const char *info) { 
    Serial.print("streaminfo  ");
    Serial.println(info); 
}

void audio_showstreamtitle(const char *info) { 
    Serial.print("streamtitle ");
    Serial.println(info); 
}

void audio_bitrate(const char *info) { 
    Serial.print("bitrate     ");
    Serial.println(info); 
}

void audio_commercial(const char *info) {  //Durada en segons
    Serial.print("commercial  ");
    Serial.println(info); 
}

void audio_icyurl(const char *info) {  //Pàgina d'inici
    Serial.print("icyurl      ");
    Serial.println(info); 
}

void audio_lasthost(const char *info) {  //URL de l'stream reproduït
    Serial.print("lasthost    ");
    Serial.println(info); 
}

void audio_eof_speech(const char *info) { 
    Serial.print("eof_speech  ");
    Serial.println(info); 
}
```

### Sortida pel port sèrie

Durant la inicialització, si la targeta SD s'inicialitza correctament, no es mostrarà cap missatge específic relacionat amb això al port sèrie, però en cas d'error, es podria afegir un missatge d'error.

No es generen missatges específics mentre es reprodueix l'àudio, però si es criden les funcions opcionals (audio_info, audio_id3data, etc.), es mostraran els missatges corresponents.

### Funcionament del programa

Aquest codi proporciona una base per a la reproducció d'àudio des d'una targeta SD utilitzant l'ESP32 i el descodificador I2S MAX98357A, facilitant la creació de projectes d'àudio.