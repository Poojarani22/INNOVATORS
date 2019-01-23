

#include <TinyGPS++.h>
#include <SoftwareSerial.h>
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

static const int RXPin = 4, TXPin = 5;  
static const uint32_t GPSBaud = 9600; 

TinyGPSPlus gps; 
WidgetMap myMap(V4); 

SoftwareSerial ss(RXPin, TXPin); 

BlynkTimer timer;

float speeds;       
float satts;     
String rings;  

char authi[] = "cc465ead5603481fa1ce9bb89503f0c5";             
char ssids[] = "MSKL";                                       
char password[] = "12345678";                                     




      
unsigned int move_index = 1;      
  

void setup()
{
  Serial.begin(115200);
  Serial.println();
  ss.begin(GPSBaud);
  Blynk.begin(authi, ssids, password);
  timer.setInterval(5000L, checkGPS);
}

void checkGPS(){
  if (gps.charsProcessed() < 10)
  {
    Serial.println(F(" NO DETECTION"));
      Blynk.virtualWrite(V4, " ERROR"); 
  }
}

void loop()
{
 
    while (ss.available() > 0) 
    {
     
      if (gps.encode(ss.read()))
        displayInfo();
  }
  Blynk.run();
  timer.run();
}

void displayInfo()
{ 

  if (gps.location.isValid() ) 
  {
    
    float la = (gps.location.lat());    
    float lo = (gps.location.lng()); 
    
    Serial.print("LAT:  ");
    Serial.println(la, 6);  
    Serial.print("LONG: ");
    Serial.println(lo, 6);
    Blynk.virtualWrite(V1, String(la, 6));   
    Blynk.virtualWrite(V2, String(lo, 6));  
    myMap.location(move_index, la, lo, "GPS_Location");
    speeds = gps.speed.kmph();             
       Blynk.virtualWrite(V3, spd);
       
     

       rings = TinyGPSPlus::cardinal(gps.course.value());
       Blynk.virtualWrite(V5, rings);               
      
    
  }
  

  Serial.println();
}

Libraries used:
1) TinyGPS++ is a new arduino library for parsing NMEA data streams provided by GPS modules.
2) SoftwareSerial is developed to allow serial communication on other digital pins of the arduino.
3) ESP8266WiFi is used to provide wifi to Node MCU ESP8266.
4) BlynkSimpleEsp8266 is used to integrate NodeMCU with the Blynk app.

The code written above is for running the GPS module, to track the location of the visually impaired.
