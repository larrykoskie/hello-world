Hello, I am Larry and I am 67 years old and this is like teaching an old dog a new trick.   This all seems too much for me, but I am willing to give it a real try.  I am having real difficulty trying to understand code, it is a whole new language to me.  Some of it is beginning to make some sense.  But I know that I have a long way to go.  I am working on a project.  This project is a type of robot that has 4 dc motors and 2 steppers.  I have 2 adafruit motor shields to stack onto a arduino uno.  I want to control the motors speed and direction with two joysticks using a pair of nrf24lo1 transceivers.  I have no idea how to write the code for this project.
While I am waiting for me new adafruit motor shields to arrive I am experimenting with an older L293D motor shield.  I know that these two boards take a different code to operate.  But I am learning on this one for now.  I took the example Motor Test and rewrote it to use all 4 motor outputs and the code looking like this:
// Adafruit Motor shield library
// copyright Adafruit Industries LLC, 2009
// this code is public domain, enjoy!

#include <AFMotor.h>

AF_DCMotor motor1(1);
AF_DCMotor motor2(2);
AF_DCMotor motor3(3);
AF_DCMotor motor4(4);
void setup() {
  Serial.begin(9600);           // set up Serial library at 9600 bps
  Serial.println("Motor test!");

  // turn on motor1
  motor1.setSpeed(200);
  motor2.setSpeed(200);
  motor3.setSpeed(200);
  motor4.setSpeed(200);
 
  motor1.run(RELEASE);
  motor2.run(RELEASE);
  motor3.run(RELEASE);
  motor4.run(RELEASE);
}

void loop() {
  uint8_t i;
  
  Serial.print("tick");
  
  motor1.run(FORWARD);
  motor2.run(FORWARD);
  motor3.run(FORWARD);
  motor4.run(FORWARD);
  for (i=0; i<255; i++) {
    motor1.setSpeed(i);
    motor2.setSpeed(i);
    motor3.setSpeed(i);
    motor4.setSpeed(i);  
    delay(10);
 }
 
  for (i=255; i!=0; i--) {
    motor1.setSpeed(i);
    motor2.setSpeed(i);
    motor3.setSpeed(i);
    motor4.setSpeed(i);  
    delay(10);
 }
  
  Serial.print("tock");

  motor1.run(BACKWARD);
  motor2.run(BACKWARD);
  motor3.run(BACKWARD);
  motor4.run(BACKWARD);
  for (i=0; i<255; i++) {
    motor1.setSpeed(i);
    motor2.setSpeed(i);
    motor3.setSpeed(i);
    motor4.setSpeed(i);  
    delay(10);
  }
 
  for (i=255; i!=0; i--) {
    motor1.setSpeed(i);
    motor2.setSpeed(i);
    motor3.setSpeed(i);
    motor4.setSpeed(i);  
    delay(10);
 }
  

  Serial.print("tech");
  motor1.run(RELEASE);
  motor2.run(RELEASE);
  motor3.run(RELEASE);
  motor4.run(RELEASE);
  delay(1000);
}
Then I changed it again to run just one motor at a time and the code looked like this:
// Adafruit Motor shield library
// copyright Adafruit Industries LLC, 2009
// this code is public domain, enjoy!

#include <AFMotor.h>

AF_DCMotor motor1(1);
AF_DCMotor motor2(2);
AF_DCMotor motor3(3);
AF_DCMotor motor4(4);
void setup() {
  Serial.begin(9600);           // set up Serial library at 9600 bps
  motor1.setSpeed(200);
  motor2.setSpeed(200);
  motor3.setSpeed(200);
  motor4.setSpeed(200); 
  motor1.run(RELEASE);
  motor2.run(RELEASE);
  motor3.run(RELEASE);
  motor4.run(RELEASE);
}
void loop() {
  uint8_t i;
  motor1.run(FORWARD);
  for (i=0; i<255; i++) {
    motor1.setSpeed(i);
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor1.setSpeed(i);
    delay(10);
}
  motor2.run(FORWARD);
  for (i=0; i<255; i++) {
    motor2.setSpeed(i);
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor2.setSpeed(i);
    delay(10);
}
  motor3.run(FORWARD);
  for (i=0; i<255; i++) {
    motor3.setSpeed(i);
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor3.setSpeed(i);
    delay(10);
}
  motor4.run(FORWARD);
  for (i=0; i<255; i++) {
    motor4.setSpeed(i);  
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor4.setSpeed(i);  
    delay(10);
 } 
  motor1.run(BACKWARD);
  for (i=0; i<255; i++) {
    motor1.setSpeed(i);
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor1.setSpeed(i);
    delay(10);
}
  motor2.run(BACKWARD);
  for (i=0; i<255; i++) {
    motor2.setSpeed(i);
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor2.setSpeed(i);
    delay(10);
}
  motor3.run(BACKWARD);
  for (i=0; i<255; i++) {
    motor3.setSpeed(i);
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor3.setSpeed(i);
    delay(10);
}
  motor4.run(BACKWARD);
  for (i=0; i<255; i++) {
    motor4.setSpeed(i);  
    delay(10);
}
  for (i=255; i!=0; i--) {
    motor4.setSpeed(i);  
    delay(10);
}
  motor1.run(RELEASE);
  motor2.run(RELEASE);
  motor3.run(RELEASE);
  motor4.run(RELEASE);
  delay(1000);
}
That code worked and it did exactly what I wanted it to do.  So now I am trying to add a joystick to control the speed and direction of just one motor.  Later I will expand it to control four motors then try to control a stepper motor then two stepper motors.  The code that I was trying to write that didn't work and I don't know what to fix looks like this:
#include <AFMotor.h>
AF_DCMotor motor1(1);
int joyVert = A0;
int motor1.setSpeed = 0;
int joyposVert = 512;
void setup() {
  Serial.begin(9600);          
motor1.run(FORWARD);
}

void loop() {
// Read the Joystick
joyposVert = analogRead(joyVert); 
// Determine if this is a forward or backward motion
// Do this by reading the Verticle Value
// Apply results to MotorSpeed and to Direction
if (joyposVert < 460);
// Set motor1 backward
  motor1.run(BACKWARD);
 }
joyposVert = joyposVert - 460;
joyposVert = joyposVert * -1;
motor1.setSpeed = map(joyposVert, 0, 460, 0, 255);
}
else if (joyposVert > 564);
{
  motor1.run(FORWARD);
motor1.setSpeed = map(joyposVert, 564, 1023, 0, 255);
}
else
{
motor1.setSpeed = 0;
}
I have taken code from other people's projects and tried to use their code to write some code for myself and I don't know if I need to go about it in another way or if it will even work for me.
