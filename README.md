# Sensors
# Description 
The work here was done practically on site and all the results were shown to the engineer responsible. Three sensors were used: a thermistor, an LDR, and an ultrasonic sensor. 
#  Thermistor 
A thermistor is a resistance thermometer or a resistor whose resistance depends on temperature. The change in resistance depends on the type of material used in the thermistor. 
### Circuit 
The components needed for this circuit are:
- Arduino Uno
- Breadboard and jumper wires
- Thermistor (10kΩ)
- Resistor (10kΩ)

The connection between the thermistor, resistor, and Arduino is shown in the diagram below. 
The thermistor and resistor are connected in series; the node between them is connected to pin A0 in the Arduino. This node is where the voltage division will happen to measure the voltage across the thermistor and based on it the temperature can be calculated. 

![image](https://github.com/user-attachments/assets/fdb2014b-8711-4429-a67f-7b74897614fc)


### Arduino Code
The following code is uploaded to the Arduino using Arduino IDE. This is to program it to show the temperature the thermistor senses in three units. 
```
#define RT0 10000
#define B 3977 

// Our series resistor value = 10 kΩ
#define R 10000

// Variables for calculations
float RT, VR, ln, TX, T0, VRT; 

void setup() {
// Setup serial communication 
Serial.begin(9600); 

// Convert T0 from Celsius to Kelvin 
T0 = 25 + 273.15; 
}

void loop() {
// Read the voltage across the thermistor 
VRT = (5.00 / 1023.00) * analogRead(A0);

// Calculate the voltage across the resistor 
VR = 5.00 - VRT; 

// Calculate resistance of the thermistor 
RT = VRT / (VR / R); 

// Calculate temperature from thermistor resistance 
ln = log(RT / RT0); 
TX = (1 / ((ln / B) + (1 / T0))); 

// Convert to Celsius 
TX = TX - 273.15; 
Serial.print("Temperature: ");

// Display in Celsius Serial.print(TX); 
Serial.print("C\t"); 

// Convert and display in Kelvin 
Serial.print(TX + 273.15); 
Serial.print("K\t"); 

// Convert and display in Fahrenheit 
Serial.print((TX * 1.8) + 32); 
Serial.println("F"); delay(500); 
}
```

# LDR 
The LDR, also known as a photoresistor, is a type of resistor that changes its resistance based on the amount of light that falls on it. 

### Circuit 
The connection between the LDR and the Arduino is shown in the figure below. It is the same connection as the thermistor. The node between the LDR and the resistor is connected to pin A0 in the Arduino. 
The circuit diagram is shown below. 

![image](https://github.com/user-attachments/assets/08067213-f9c9-4465-98cf-391583d7d216)


### Arduino Code
The following code is uploaded to the Arduino using Arduino IDE. This is to program it to show the LDR readings and decision between five light choices: dark, dim, light, bright, very bright.
```
int ldrpin = 0;
int ldreading;

void setup() {
Serial.begin(9600);
}

void loop() {
// reads the input on analog pin A0 
ldreading = analogRead(ldrpin);

Serial.print("Analog reading: ");
Serial.print(ldreading);

// the raw analog reading
  if (ldreading < 10) {
    Serial.println(" - Dark");
  } else if (ldreading < 200) {
    Serial.println(" - Dim");
  } else if (ldreading < 500) {
    Serial.println(" - Light");
  } else if (ldreading < 800) {
    Serial.println(" - Bright");
  } else {
    Serial.println(" - Very bright");
  }

  delay(500);
}
```

# Ultrasonic Sensor (and buzzer)
An ultrasonic sensor is an instrument that measures the distance to an object using ultrasonic sound waves. It uses a transducer to send and receive ultrasonic pulses that give back information about an object’s proximity.

### Circuit 
The connection for this circuit is done as follows: 
-	Sensor Trig to ~6
-	Sensor Echo to ~5 
-	Buzzer leg to 2 
-	Buzzer leg to GND

 ![image](https://github.com/user-attachments/assets/5d355392-9aae-4406-80c0-86a9b652add3)
 

### Arduino Code
The code below is uploaded to the Arduino using Arduino IDE. It programs the circuit to make the buzzers sound louder and faster when the ultrasonic sensor senses closer objects, and a slower sound when the sensor senses a farther object. 
```
#define trigPin 6  
#define echoPin 5
#define buzzer 2
float new_delay; 

void setup()
{
  Serial.begin (9600); 
  pinMode(trigPin, OUTPUT); 
  pinMode(echoPin, INPUT);
  pinMode(buzzer,OUTPUT);
}

void loop() 
{
 long duration, distance;
 digitalWrite(trigPin, LOW);        
 delayMicroseconds(2);              

 digitalWrite(trigPin, HIGH);
 delayMicroseconds(10);           

 digitalWrite(trigPin, LOW);

 duration = pulseIn(echoPin, HIGH);
 distance = (duration/2) / 29.1;
 new_delay= (distance *3) +30;

 Serial.print(distance);
 Serial.println("  cm");

  if (distance < 50)
  {
   digitalWrite(buzzer,HIGH);
   delay(new_delay);
   digitalWrite(buzzer,LOW);
  }
  else
  {
   digitalWrite(buzzer,LOW);
  }

 delay(200);
}
```
