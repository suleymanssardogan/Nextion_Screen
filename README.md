# Nextion_Screen
All_Nextion_Screen_Project

**1-Turn_On_LED_Turn_Off_LED**
#include <SoftwareSerial.h>
SoftwareSerial screen(2,3);
void setup() {
  // put your setup code here, to run once:

    screen.begin(9600);
    Serial.begin(9600);
    pinMode(13,OUTPUT);

}
int sayac=0;
byte data;
bool toggle=0;

void loop() {
  
  // put your main code here, to run repeatedly:
  while(screen.available()>0){
     data = screen.read();
      if(sayac==2)
      {
          if(data==1)
          {
            if(toggle==0)
            {
              
              digitalWrite(13,1);
              Serial.println("lamba yandı");
              toggle=1;
            }
            else
            {
              Serial.println("lamba sondu");
              digitalWrite(13,0);
              toggle=0;
            }
          }

      }
      sayac++;
      Serial.print("Sayac: ");
      Serial.println(sayac);
      if(screen.available()==0)
      {
        Serial.println("sayac sifir");
       sayac=0;
      }
  } 
}


**2-Gauge_Control**

//Library for Nextion Screen
#include <Nextion.h>
//You can use SoftwareSerial but I got mistakes for Arduino Mega and Uno
#include <SoftwareSerial.h>

#define USE_SOFTWARE_SERIAL
#define DEBUG_SERIAL_ENABLE
//Assigment Analog1 pin for potentiometer
#define pot1 A1


//First Number is page number where we made component,Second Number is Component ID,Third is a component name 
NexGauge gauge = NexGauge(1, 1, "z0");
NexGauge gauge2 = NexGauge(1,2,"z1");
//SoftwareSerial nextion(3,2);//First pin RX Second pin TX  


void setup() {
  Serial.begin(9600);
 
  nexInit();

}

void loop() {
  // Read value of  potentiometer and from 0-1023 to  0-359 scale to range
  int potValue = analogRead(pot1);
  int gauge_value = map(potValue, 0, 1023, 0, 359);
  int gauge2_value = map(potValue,0,1023,0,359);
  
  
  //Update Nextion Gauge's component 
  gauge.setValue(gauge_value);
  gauge2.setValue(gauge2_value);

  /*Serial.print("Pot Value: ");
  Serial.println(potValue);
  Serial.print("Gauge Value: ");
  Serial.println(gauge_value);
  Serial.println("Gauge2 Value");
  Serial.println(gauge2_value);*/
 // delay(50);
}



**3-Write_Screen**
#include <SoftwareSerial.h>

SoftwareSerial nextion(2, 3); // Nextion TX to pin 2, RX to pin 3

void setup() {
  nextion.begin(9600); // Nextion Baud Rate
  Serial.begin(9600); // Arduino Baud Rate

  // We send first message to Nextion Screen 
  sendToNextion("Hello World!");
}

void loop() {
  // Eğer ekrana tekrar bir mesaj göndermek istiyorsanız, buraya kod ekleyebiliriz
  //If you want to send message to screen  again,you can add code here 
}

void sendToNextion(String message) {
  //Create command as String
  String cmd = "t0.txt=\"" + message + "\"";
  nextion.print(cmd);
  nextion.write(0xff);
  nextion.write(0xff);
  nextion.write(0xff);
}


  


