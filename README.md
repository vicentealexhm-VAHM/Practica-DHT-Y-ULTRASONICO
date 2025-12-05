# Practica-DHT-Y-ULTRASONICO
## Descripción
En esta practica agregaremos una pantalla ```LCD 16x2 I2C``` un ```ultrasonico``` y un sensor ```DHT``` que hará que visualicemos los datos del ultrasonico y del sensor de temperatura, utilizando el simulador de la ESP32 de WOKWI
## Material
- Tarjeta ```ESP32```
- Pantalla ```LCD 16x2 I2C```
- El sensor ```HC-SR04 ULTRASONIC DISTANCE SENSOR``` y ```DHT22```
- Las librerias ```LiquidCrystal I2C``` y ```DHT sensor library for ESPx```
## Instrucciones
1. Abrir WOKWI en el navegar y escogemos el simulador ESP32
![](https://github.com/vicentealexhm-VAHM/Practica-DHT11/blob/main/1.png?raw=true)
2. Bajar hasta ``Starter Templates`` y elegir la ``ESP32``
![](https://github.com/vicentealexhm-VAHM/Practica-DHT11/blob/main/2.png?raw=true)
3. Una vez adentro nos vamos a la pestaña que dice ``Library Manager`` y hay buscamos las librerias antes mencionada presionando en el circulo con el simbolo de mas
![]()
4. Una vez agregadas regresamos a la pestaña de sketch donde agregaremos y conectaremos el ultrasonico, el DHT22 y la lcd a la tarjeta y para agregarlo hacemos click en el circulo azul con el simbolo de mas y lo buscamos como ```DHT22```, ``HC-SR04 Ultrasonic Distance Sensor`` y ``lcd 16x2(l2c)``

![](https://github.com/vicentealexhm-VAHM/Practica-ULTRASONICO-con-LCD/blob/main/ultrasonico.png?raw=true)
![](https://github.com/vicentealexhm-VAHM/Practica-ULTRASONICO-con-LCD/blob/main/lcd.png?raw=true)

5. Una vez que lo agregamo realizamos la conexion haciendo click sobre los pines o sobre los puntos amarrillos y lo llebamos a donde lo deceamos conectar

![]()

6. En el sketch colocamos el siguiente codigo:

```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

const int DHT_PIN = 17;
const int Trigger = 4;
const int Echo = 15;

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0

}

void loop() {

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms
  
  lcd.clear();
  lcd.setCursor(3, 0);
  lcd.print("Bienvenidos");
  lcd.setCursor(4,1);
  lcd.print("Modulo 5");
  delay(2000);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Vicente H.");
  lcd.setCursor(0,1);
  lcd.print("Ing. Mecanica");
  delay(2000);

  lcd.clear();
  lcd.setCursor(1, 0);
  lcd.print("  Distancia: ");
  lcd.setCursor(5, 1);
  lcd.print(d);
  lcd.print("cm");
  delay(2000);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  delay(2000);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("  Fecha: ");
  lcd.setCursor(3, 1);
  lcd.print("22/11/2025");
  delay(2000);

}
```
7. Ya que lo tengamos damos click al boton de play para que se inicie el simulador y se podra visualizar las lecturas del ultrsonico y del sensor de temperatura en la pantalla LCD.

![]()

## Resultados
Como resultado aprendimos a como visualizar los datos en una pantalla ya sea de cualquier sensor y con el codigo podemos escoger lo que queremos que aparezca en la pantalla y en el orede que deseamos
## Créditos
Desarrollado por Vicente Alexander Hernandez Maldonado
