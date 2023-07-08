# Task-2-part-2
#include "ThingSpeak.h"
#include <ESP8266WiFi.h>

//------- WI-FI details ----------//
char ssid[] = "SSID"; //SSID here
char pass[] = "PASSWORD"; // Password here
//--------------------------------//

//----------- Channel details ----------------//
unsigned long Channel_ID =123456; // Your Channel ID
const char * myWriteAPIKey = "ACBDE12345"; //Your write API key
//-------------------------------------------//

const int Field_Number_1 = 1;
String value = "";
int value_1 = 0;
WiFiClient  client;

void setup()
{
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  ThingSpeak.begin(client);
  internet();
}

void loop()
{
  internet();
  if (Serial.available() > 0)
  {
    delay(100);
    while (Serial.available() > 0)
    {
      value = Serial.readString();
      if (value[0] == '*')
      {
        if (value[2] == '#')
        {
          value_1 = value[1] - 0x30;
        }
      }
    }
  }
  upload();
}

void internet()
{
  if (WiFi.status() != WL_CONNECTED)
  {
    while (WiFi.status() != WL_CONNECTED)
    {
      WiFi.begin(ssid, pass);
      delay(5000);
    }
  }
}

void upload()
{
  ThingSpeak.writeField(Channel_ID, Field_Number_1, value_1, myWriteAPIKey);
  delay(15000);
  value = "";
}

