# Home-automation-system
CODE FOR CONNECTING TO AURDINO UNO
#include <EEPROM.h>
#include <IRremote.h>
#include <SoftwareSerial.h>
SoftwareSerial mySerial(2, 3); // RX, TX D2, D3
#define RelayPin1 4  //D4
#define RelayPin2 5  //D5
#define RelayPin3 6  //D6
#define RelayPin4 7  //D7
#define SwitchPin1 10  //D10
#define SwitchPin2 11  //D11
#define SwitchPin3 12  //D12
#define SwitchPin4 13  //D13
#define IR_RECV_PIN A0  //A0
//Update the HEX code of IR Remote 0x<HEX CODE>
#define IR_Button_1   0x80BF49B6
#define IR_Button_2   0x80BFC936
#define IR_Button_3   0x80BF33CC
#define IR_Button_4   0x80BF718E
#define IR_All_Off    0x80BF3BC4
#define IR_All_On     0x80BFF10E
IRrecv irrecv(IR_RECV_PIN);
decode_results results;
// Switch State
bool SwitchState_1 = LOW;
bool SwitchState_2 = LOW;
bool SwitchState_3 = LOW;
bool SwitchState_4 = LOW;
char bt_data; // variable for storing bluetooth data
void relayOnOff(int relay){
 switch(relay){
      case 1:
            digitalWrite(RelayPin1, !digitalRead(RelayPin1)); // change state for relay-1
            EEPROM.update(0,digitalRead(RelayPin1));
            delay(100);
      break;
      case 2:
            digitalWrite(RelayPin2, !digitalRead(RelayPin2)); // change state for relay-2
            EEPROM.update(1,digitalRead(RelayPin2));
            delay(100);
      break;
      case 3:
            digitalWrite(RelayPin3, !digitalRead(RelayPin3)); // change state for relay-3
            EEPROM.update(2,digitalRead(RelayPin3));
            delay(100);
      break;
      case 4:
            digitalWrite(RelayPin4, !digitalRead(RelayPin4)); // change state for relay-4
            EEPROM.update(3,digitalRead(RelayPin4));
            delay(100);
      break;
      default : break;      
      }  
}
 
void eepromState()
{
  digitalWrite(RelayPin1, EEPROM.read(0)); delay(200);
  digitalWrite(RelayPin2, EEPROM.read(1)); delay(200);
  digitalWrite(RelayPin3, EEPROM.read(2)); delay(200);
  digitalWrite(RelayPin4, EEPROM.read(3)); delay(200);
}  
 
void bluetooth_control()
{
  if(mySerial.available()) {
    bt_data = mySerial.read();
    Serial.println(bt_data);
    switch(bt_data)
        {
          case 'A': digitalWrite(RelayPin1, LOW);  EEPROM.update(0,LOW); break; // if 'A' received Turn on Relay1
          case 'a': digitalWrite(RelayPin1, HIGH); EEPROM.update(0,HIGH); break; // if 'a' received Turn off Relay1
          case 'B': digitalWrite(RelayPin2, LOW);  EEPROM.update(1,LOW); break; // if 'B' received Turn on Relay2
          case 'b': digitalWrite(RelayPin2, HIGH); EEPROM.update(1,HIGH); break; // if 'b' received Turn off Relay2
          case 'C': digitalWrite(RelayPin3, LOW);  EEPROM.update(2,LOW); break; // if 'C' received Turn on Relay3
          case 'c': digitalWrite(RelayPin3, HIGH); EEPROM.update(2,HIGH); break; // if 'c' received Turn off Relay3
          case 'D': digitalWrite(RelayPin4, LOW);  EEPROM.update(3,LOW); break; // if 'D' received Turn on Relay4
