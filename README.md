# Arduino Library for ESP32 to control TP Link Kasa Smart Plug (Local Network mode)
 This library will allow ESP32 to scan the TP Link Kasa smart plug and Light Switch on the local network.
 You can control the TP Link Smart Plugs and Light Switches. Please make sure you ESP32 is on the same
 WIFI network as your TP Link Smart Plug devices.
 
 This library is a fork of the library https://github.com/kj831ca/KasaSmartPlug developed by Kris Jearkul (released under the MIT License); the fork was made December 29, 2024.
 
 This library is also released under the MIT License as well.

 This fork incrementally includes the ability to report:
 - on Kasa KS and KP devices (*)
 - the Kasa device's MAC Id
 - to the Serial Output window

 (*) this library has been tested with a Kasa KP400 device - which is a one unit with two plugs - accessible at via one IP address and with on MAC id. This library will will report the (default) name of the unit, but not the name of two separate plugs if they have been individually named via the Kasa Phone App.  Sadly, the Kasa Phone App appears not to allow you to change the unit's default name. In any case, this library will report the KP400's default name, model, IP address, MAC Id and will report its state as off even if one or both plugs are on.
 
 # Dependencies
 This library requires ArduinoJson by Benoit Blanchon. 
 https://arduinojson.org/?utm_source=meta&utm_medium=library.properties
 Please make sure you install ArduinoJson to your Arduino IDE.
 
 
 # Example: Scan local network for TP Link Kasa Smart Plug devices and print out the device details.
 ~~~c++
     #include <WiFi.h>
     #include "KasaSmartPlug.h"
     
     const char *ssid = "your wifi ssid";
     const char *password = "your wifi password";
     
     KASAUtil kasaUtil;
     
     void setup()
     {
      int found;
      Serial.begin(115200);

      kasaUtil.SetDebug(true);

      // connect to WiFi
      Serial.printf("Connecting to %s ", ssid);
      WiFi.begin(ssid, password);
      while (WiFi.status() != WL_CONNECTED)
      {
        delay(500);
        Serial.print(".");
      }
      Serial.println(" CONNECTED");

      found = kasaUtil.ScanDevices();
      Serial.printf("\r\n Found device = %d", found);

      // Print out devices name and ip address found..
      for (int i = 0; i < found; i++)
      {
        KASASmartPlug *p = kasaUtil.GetSmartPlugByIndex(i);
        Serial.printf("\r\n %d. %s IP: %s Relay: %d", i, p->alias, p->ip_address, p->mac, p->state);  
      } 
     }
     
 ~~~
 
 # Example: Turn on the smart plug and read feedback
 
 ~~~c++
 //Get the Smart Plug object by using its alias name. 
 //Please note the kasaUtil.ScanDevices() need to be called.
 KASASmartPlug *testPlug = kasaUtil.GetSmartPlug("Kasa Device Alias Name");
 
 //Turn ON the plug
 testPlug->SetRelayState(1);
 
 //Turn OFF the plug
 testPlug->SetRelayState(0);
 
 //Read the plug information and relay state
 testPlug->QueryInfo();
 ~~~
 
 # Credit

 Thanks to Kris Jearkul for the library from which this one was forked ( https://github.com/kj831ca/KasaSmartPlug ).
  
 Thanks for the information from https://www.softscheck.com/en/reverse-engineering-tp-link-hs110/
 without this, it will be impossible for me to complete this library.
 
 Very well written library ArduinoJson by Benoit Blanchon. https://arduinojson.org/?utm_source=meta&utm_medium=library.properties 
 
