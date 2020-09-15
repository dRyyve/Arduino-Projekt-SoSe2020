# Arduino-Projekt-SoSe2020

#include <Keypad.h>

//Verkabelung: 
//rohr1: Pin 9 und 13
//rohr2: Pin 10 und 14
//rohr3: Pin 11 und 15
//rohr4: Pin 12 und 16

const int DELAY_TIME = 600; //Delay zwischen Relais 1 und 2

//KEYPAD
const byte ROWS = 4;
const byte COLS = 4; 
char keys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

byte rowPins[ROWS] = {17, 2, 3, 4}; 
byte colPins[COLS] = {5, 6,7,8};
Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );



//RELAY
const int relayPin[]={9,10,11,12, 13, 14, 15, 16};// OUTPUT PINS
String relayNames[] ={"CH1", "CH2","CH3","CH4","CH5","CH6","CH7","CH8"};//NAMES

void setup(){

  Serial.begin(9600);
  for(int i=0; i<8; i++)    //ARRAY
  {
    pinMode(relayPin[i], OUTPUT); // SET RELAY PINS AS OUTPUT
    digitalWrite(relayPin[i], HIGH);// INITIAL RELAY STATUS OFF
  }
  
  Serial.println("8 channel relay keypad");
  
}
  
void loop(){
   
  int val;
  int knum;
  char key = keypad.getKey();
  
  
  if(key && key !='*' && key !='#' && key !='0' && key !='9' ){
    knum = (int)key-49;// CONVERT CHAR TO INTEGER
    if(knum>=1 && knum<=4){
  knum -=1; //um 1 dekrementieren, da der erste array index 0 ist! 
  //a number between 1 and 4 was pressed!
  digitalWrite(relayPin[knum], LOW);// TURN ON FIRST RELAY
  delay(DELAY_TIME);
  digitalWrite(relayPin[knum], HIGH);// TURN OFF FIRST RELAY

  digitalWrite(relayPin[knum+4], LOW);// TURN ON second RELAY
  delay(DELAY_TIME);
  digitalWrite(relayPin[knum+4], HIGH);// TURN OFF second RELAY
    }

delay(50);

}}
