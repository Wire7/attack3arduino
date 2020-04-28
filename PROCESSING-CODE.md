//# attack3arduino
//Controlling Servos with Logitech Attack3 Joystick and Arduino

// This is the processing code, use after you flashed the firmata sketch to arduino!



import processing.serial.*;

import net.java.games.input.*;
import org.gamecontrolplus.*;
import org.gamecontrolplus.gui.*;

import cc.arduino.*;
import org.firmata.*;

ControlDevice cont;  //Sets the Devices Name that will be used to control the Arduino e.g. Joystick 
ControlIO control;
Arduino arduino;

float joystickX;
float joystickY;
float joystickZ;

boolean but1;
boolean but2;
boolean but3;
boolean but4;
boolean but5;
boolean but6;
boolean but7;
boolean but8;
boolean but9;
boolean but10;
boolean but11;

int servoXPin=3;  // Where servoX is connected to arduino
int servoYPin=5;  // Where servoY is connected to arduino
int servoZpin=12;  //
int led1Pin=6;
int led2Pin=7;
int led3Pin=8;
int led4Pin=9;

void setup() {

  size(360, 200);
  control = ControlIO.getInstance(this);
  cont = control.getMatchedDevice("attack3");  // between "" there is the name givin to the joystick or the controller used

  if (cont == null) {  // if the device "joystick or Gamepad etc .. " is not connected to USB then sow the following msg
    println("Controller not connected! Please check it out.");
    System.exit(-1);
  } //End of "if" code

  println(Arduino.list());  //Reads the connected USB devices and prints it to the Serial Monitor

  arduino = new Arduino(this, Arduino.list()[2], 57600); // change the number between [] to the device port's number
  arduino.pinMode(servoXPin, Arduino.SERVO);
  arduino.pinMode(servoYPin, Arduino.SERVO);
  arduino.pinMode(led1Pin, Arduino.OUTPUT);
  arduino.pinMode(led2Pin, Arduino.OUTPUT);
  arduino.pinMode(led3Pin, Arduino.OUTPUT);
  arduino.pinMode(led4Pin, Arduino.OUTPUT);
  
  
} //End of void setup loop

public void getUserInput() {

  joystickX = map(cont.getSlider("servoX").getValue(), 1, -1, 0, 150);     // joystick configuration
  joystickY = map(cont.getSlider("servoY").getValue(), -1, 1, 12, 180);    // joystick configuration
  but1 = cont.getButton("bn1").pressed();
  but2 = cont.getButton("bn2").pressed();
  but3 = cont.getButton("bn3").pressed();
  but4 = cont.getButton("bn4").pressed();
  but5 = cont.getButton("bn5").pressed();
  but6 = cont.getButton("bn6").pressed();
  but7 = cont.getButton("bn7").pressed();
  but8 = cont.getButton("bn8").pressed();
  but9 = cont.getButton("bn9").pressed();
  but10 = cont.getButton("bn10").pressed();
  but11 = cont.getButton("bn11").pressed();
  
  
 
  
   


  print(but1);
  print (", ");
  print(joystickX);
  print (", ");
  println (joystickY);
} // End of public void loop

void draw() {

  getUserInput();
  background(joystickX, 100, 255);
  background(joystickY, 5, 68);

  arduino.servoWrite(servoXPin, (int)joystickX);
  arduino.servoWrite(servoYPin, (int)joystickY);

  if (but1 == true) {
    arduino.digitalWrite(led1Pin, Arduino.HIGH);
  } // End if 1
  
  if (but2 == true) {
    arduino.digitalWrite(led1Pin, Arduino.LOW);
    arduino.digitalWrite(led2Pin, Arduino.LOW);
    arduino.digitalWrite(led3Pin, Arduino.LOW);
    arduino.digitalWrite(led4Pin, Arduino.LOW);
  } // End if 2
  
  if (but3 == true) {
    arduino.servoWrite(servoXPin, 10);
    delay (500);
    arduino.servoWrite(servoXPin, 140);
    delay (500);
  } // End if 3
  
  if (but4 == true) {
    arduino.servoWrite(servoXPin, 150);
  } // End if 4
  
  if (but5 == true) {
    arduino.servoWrite(servoXPin, 1);
  } // End if 5
  
  
  if (but6 == true) {
    arduino.digitalWrite(led2Pin, Arduino.HIGH);
  } // End if 6
  
  if (but7 == true) {
    arduino.digitalWrite(led3Pin, Arduino.HIGH);
  } // End if 7
  
  if (but8 == true) {
    arduino.digitalWrite(led4Pin, Arduino.HIGH);
  } // End if 8
  
  
  
}
