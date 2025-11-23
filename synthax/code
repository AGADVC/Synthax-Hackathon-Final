#include <Wire.h> // library for I2C communication
#include <VL53L0X.h> //Library for ToF lidar

VL53L0X sensor; //Creates and object of VL53L0X

const int I2C_SDA = 21;
const int I2C_SCL = 22;

// 2 vibaration motors used so that the user can easily feel it
const int MOTOR1_PIN = 18; //vibration motor 1 pin to alert user   
const int MOTOR2_PIN = 19; //vibration motor 2 pin to alert user

const int LED_PIN = 4;   //LED is used for testing purposes and video as vibration cant be seen through video

const int TRIGGER_DISTANCE = 2000;  //threshhold distance- if an object comes closer than this distance in millimiters, motors and led will be triggered
const int MIN_DISTANCE = 50; //Time intervals between vibration reaches its minimum at this distance in millimeters

// Pulse timing (ms)
const int MAX_PULSE_DELAY = 1000; //maximum time between pulses in milliseconds
const int MIN_PULSE_DELAY = 50;  //minimum time between pulses in milliseconds

// Motor timing
unsigned long lastPulseTime1 = 0; //sets the variable for the the time of the last pulse of motor 1 as 0
bool motorState1 = false; //sets the state of motor 1 to 0 or false

unsigned long lastPulseTime2 = 0; //sets the variable for the the time of the last pulse of motor 2 as 0
bool motorState2 = false; //sets the state of motor 2 to 0 or false

// LED timing
unsigned long lastLedPulse = 0; //sets the variable for the the time of the last pulse of LED as 0
bool ledState = false; //sets the state of LED to 0 or false

void setup() {
  Serial.begin(115200); //sets up rate of communication of microcontroller with serial monitor
  delay(100); //Delay of 100ms or 0.1 secs

  Wire.begin(I2C_SDA, I2C_SCL); //starts communication of VL53L0X sensor with microcontroller

  pinMode(MOTOR1_PIN, OUTPUT); //Defines the vibration motor 1 as an output device
  pinMode(MOTOR2_PIN, OUTPUT); //Defines the vibration motor 2 as an output device
  pinMode(LED_PIN, OUTPUT);  //Defines the LED as an output device

  digitalWrite(MOTOR1_PIN, LOW); //Sets initial state of vibration motor 1 as LOW or off
  digitalWrite(MOTOR2_PIN, LOW); //Sets initial state of vibration motor 2 as LOW or off
  digitalWrite(LED_PIN, LOW); //Sets initial state of LED as LOW or off

  if (!sensor.init()) {
    Serial.println("VL53L0X init failed. Check wiring."); //if the sensor is nit initialised, it indicates an error in wiring which is printed in the serial monitor
    while (1) delay(1000); //loops back to the beginning of if statement
  }

  sensor.startContinuous(); //keeps taking distance measurements repeatedly in the background
  Serial.println("VL53L0X initialized, starting continuous readings.");// prints the statement that the sensor is about to start reading values in the serial monitor
}

void loop() {

//a function to measure the distance from the sensor and anything in front of it  is called. Its value is stored in integar distance
  int distance = sensor.readRangeContinuousMillimeters(); //get the most recent measured distance in millimeters.


//if the sensor fails, all effectors(motors,LEds) are set to LOW or off
  if (sensor.timeoutOccurred()) {
    Serial.println("Sensor timeout!");
    digitalWrite(MOTOR1_PIN, LOW);
    digitalWrite(MOTOR2_PIN, LOW);
    digitalWrite(LED_PIN, LOW);
    return;
  }

  Serial.print("Distance (mm): "); //prints the words 'Distance (mm) in the serial monitor- It doesnt affect functionality, it only formats the output
  Serial.println(distance); //prints the actual value of the distance between object and sensor in serial monitor

// if object is too far away from sensor, all motors and LEDs(effectors) are LOW or off
  if (distance > TRIGGER_DISTANCE) {
    digitalWrite(MOTOR1_PIN, LOW);
    digitalWrite(MOTOR2_PIN, LOW);
    digitalWrite(LED_PIN, LOW);
    return;
  }

  // Clamp distance
  if (distance < MIN_DISTANCE) distance = MIN_DISTANCE;

// maps the minimum distance and maximum distance to the minimum pulse delay and maximum pulse delay
  int pulseDelay = map(distance, TRIGGER_DISTANCE, MIN_DISTANCE,MAX_PULSE_DELAY, MIN_PULSE_DELAY);

  // if the time interval between pulses becomes too small, it resets the time interval to the integar MIN_PULSE_DELAY as defined before
  if (pulseDelay < MIN_PULSE_DELAY) pulseDelay = MIN_PULSE_DELAY; 

 // if the time interval between pulses becomes too large, it resets the time interval to the integar MAX_PULSE_DELAY as defined before
  if (pulseDelay > MAX_PULSE_DELAY) pulseDelay = MAX_PULSE_DELAY;

  unsigned long now = millis();//stores time passed since ESP32 was started in a variable 'now'

  // Motor 1 pulse
  if (now - lastPulseTime1 >= pulseDelay){// checks if enought time has passed between current time and time of last motor pulse
                                          //if the time passed is greater than or equal to current pulse delay as mapped befor:
    motorState1 = !motorState1; // It sets the state of the motor to the opposite of the previous state. Ex- If motor was LOW, motor becomes HIGH and vice versa
    lastPulseTime1 = now; //resets the time of the current time(now=millis())
    digitalWrite(MOTOR1_PIN, motorState1 ? HIGH : LOW); //implemets the state opposite of previous one
  }

  // Motor 2 pulse-same logic as motor 1 pulse but uses new set of state variables and pulse time variables
  if (now - lastPulseTime2 >= pulseDelay) {
    motorState2 = !motorState2;
    lastPulseTime2 = now;
    digitalWrite(MOTOR2_PIN, motorState2 ? HIGH : LOW);
  }

  // LED pulse-same logic as motor 1 and motor 2 pulse but uses new set of state variables and pulse time variables
  if (now - lastLedPulse >= pulseDelay) {
    ledState = !ledState;
    lastLedPulse = now;
    digitalWrite(LED_PIN, ledState ? HIGH : LOW);
  }
}
