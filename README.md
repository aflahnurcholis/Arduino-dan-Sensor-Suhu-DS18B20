# Pemrograman Arduino dan Sensor Suhu DS18b20

Pertama, dalam membuat program Arduino dan Sensor Suhu DS18b20 diperlukan beberapa komponen dasar, diantarannya :
- Arduino Uno dan USB
- Sensor Suhu DS18b20
- Resistor 4.7K (opsional)
- Kabel Jumper
- Project Board
- LCD atau OLED (Opsional)
- Buzzer (Opsional)
- LED (Opsional)

## Instalasi Program Arduino IDE dan Driver USB CH340

Download Arduino IDE versi 2 melalui link berikut [Arduino IDE](https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-downloading-and-installing/). Pastikan Arduino yang dipilih sesuai spesifikasi PC atau Laptop yang kalian miliki.
Instal Arduino IDE yang telah terdownload.
Nb : Pastikan terkoneksi dengan internet atau WiFi, karena pada saat penginstalan Arduino IDE akan melakukan pengunduhan beberapa library dan Board.

## Instalasi driver CH340

Berikut adalah langkah-langkah untuk melakukan instalasi driver tersebut :

1. Download File Driver CH340/CH341 >>> [Download DISINI.](https://drive.google.com/file/d/1mjJGaasvh0iiqljNrpCc17mMRMeVlDaC/view)
2. Hubungkan Hardware Arduino Uno/Nano/Mega ke Laptop/PC/Komputer anda melalui USB.
3. Ekstrak File yang sudah Ter-download tersebut, kemudian jalankan File tersebut dengan klik kiri 2x.
4. Kemudian Klik INSTALL seperti tampak pada gambar dibawah ini:

![Image](https://github.com/user-attachments/assets/f8a4260a-19a2-4e56-8d93-006ff4c8717d)

6. Driver CH340 atau CH341 untuk Arduino sudah Ter-Install dan siap digunakan.
7. Silahkan digunakan untuk Upload Program dari Arduino IDE ke Board Arduino yang kalian miliki.

## Rangkaian Arduino Dan Sensor Suhu DS18b20

Setelah Arduino IDE dan Driver siap, maka langkah selanjutnya membuat rangkaian sesuai gambar dibawah :
![Image](https://github.com/user-attachments/assets/856b9f2f-40fc-4108-95c9-c6828490cc5a)
Sesuaikan rangkaian berdasarkan kebutuhan yang dimiliki.

## Coding Program Arduino IDE dan Upload ke Arduino UNO

Arduino IDE menggunakan bahasa pemrograman C++ sehingga mudah dipahami. Perlu diperhatikan dalam pemrograman Arduino sesuai rangkaian diatas perlu menambahkan beberapa library. Library tersebut dapat di unduh melalui library manager yang terdapat pada window tool disebelah kiri. Seperti contoh dibawah ini :

![Image](https://github.com/user-attachments/assets/3f6731e4-b4eb-4263-8fe7-87f7e2f54eeb)

```
#include <SPI.h>
#include <Wire.h>
#include <OneWire.h>
#include <DallasTemperature.h> //Library dapat diunduh melalui library manager
#include <LiquidCrystal_I2C.h> //Library dapat diunduh melalui library manager

LiquidCrystal_I2C LCD = LiquidCrystal_I2C(0x27, 16, 2); 

#define SENSOR_PIN A1   // hubungkan pin sensor DS18B20 ke pin A1 arduino
#define BUZZER_PIN 12   // hubungkan pin buzzer ke pin 12 arduino, jika tidak menggunakan buzzer bisa di"comment" saja

OneWire oneWire(SENSOR_PIN);         // setup a oneWire 
DallasTemperature tempSensor(&oneWire); // pass oneWire 

unsigned long previousMillis = 0;

String tempString,tempString1;
float batas = 35;     // batas suhu biar alarm
float tempCelsius;

void setup() {
  Serial.begin(9600);
  LCD.init();
  LCD.backlight();
  pinMode(BUZZER_PIN,OUTPUT);
  digitalWrite(BUZZER_PIN,LOW);

  LCD.clear(); // clear display
  LCD.setCursor(0,0);
  LCD.print("Aflah Nurcholis");
  LCD.setCursor(0,1);
  LCD.print("*085747272461*");

  tempSensor.begin();     // inisialisasi sensor
  tempString.reserve(10); // to avoid fragmenting memory when using String
  delay(5000);
}

void loop() {
  tempSensor.requestTemperatures();             // send the command to get temperatures
  tempCelsius = tempSensor.getTempCByIndex(0);  // read temperature in Celsius
  Serial.println(tempCelsius); // print the temperature in Celsius to Serial Monitor

  LCD.clear();
  LCD.setCursor(0,0);
  LCD.print("Suhu: ");
  LCD.setCursor(0,1);
  LCD.print(tempCelsius);
  LCD.setCursor(6,1);
  LCD.print("Celcius");
  delay(500);

  if( tempCelsius >= batas)  //cek alarm vs buzzer
   {
    digitalWrite(BUZZER_PIN,HIGH);
    delay(200);
    digitalWrite(BUZZER_PIN,LOW);
    delay(50);
   }
  
  else digitalWrite(BUZZER_PIN,LOW);
}
```

Copy atau salin sketch program tersebut ke Arduino IDE.
