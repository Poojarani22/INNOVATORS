1st Part of the project

// 2 Ultrasonic sensors and a buzzer


int ultra01trig=2;
int ultra01echo=3;

int ultra02trig=4;
int ultra02echo=5;

int ultrabuzzer=6;

void setup() {
  Serial.begin (9600);
  pinMode(ultra01trig, OUTPUT);
  pinMode(ultra01echo, INPUT);
   pinMode(ultra02trig, OUTPUT);
  pinMode(ultra02echo, INPUT);
  pinMode(ultrabuzzer, OUTPUT);
   
}

void loop() {
  long durations1, distances1;

  digitalWrite(ultrabuzzer,LOW);
  
  
  digitalWrite(ultra01trig, LOW);  // Added this line
  delayMicroseconds(2); // Added this line
  digitalWrite(ultra01trig, HIGH);
  delayMicroseconds(10); // Added this line
  digitalWrite(ultra01trig, LOW);
  duration1 = pulseIn(ultra01echo, HIGH);
  distance1 = (duration1/2) / 29.1;

   
   Serial.print ( "Sensor1  ");
   Serial.print ( distances1);
   Serial.println("cm");

   if(distance1 < 50)
    { 
        digitalWrite(ultrabuzzer, HIGH);
        delay(100);
     }
  
  digitalWrite(echoPin1, LOW);
  delay(250);

  
  
long durations2, distances2;
  digitalWrite(ultra02trig, LOW);  // Added this line
  delayMicroseconds(2); // Added this line
  digitalWrite(ultra02trig, HIGH);
  delayMicroseconds(10); // Added this line
  digitalWrite(ultra02trig, LOW);
  durations2 = pulseIn(ultra02echo, HIGH);
  distances2= (durations2/2) / 29.1;

   
   Serial.print("Sensor2  ");
    Serial.print(distances2);
    Serial.println("cm");
  
   if(distances2 < 40)
     { 
        digitalWrite(ultrabuzzer, HIGH);
        delay(1000);
     }
  
  digitalWrite(ultra02echo, LOW);
  delay(2500);

  
  
}

The code1.docx shows the code we have written for the ultrasonic sensors to detect any object encountered on the path, a buzzer attached with the arduino board along with the two ultrasonic sensors to give a sound alert to the visually impaired.
