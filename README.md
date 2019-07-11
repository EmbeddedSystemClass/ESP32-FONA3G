# ESP32-FONA3G
This is a from scratch project to show and remember how to build up a WiFi/3G System based on an ESP32 and FONA 3G (SIM5320A)

# Hardware
In this project, the prototype is made of 2 main developpement boards.
  1. Adafruit HUZZAH32 – ESP32 Feather Board
  2. Adafruit FONA 3G Cellular + GPS Breakout
  
## Adafruit HUZZAH32 – ESP32 Feather Board
https://www.adafruit.com/product/3405
The HUZZAH32 is a very easy to use, very well packed developpement board that can stacks with many other Feather family boards (see more here: https://www.adafruit.com/feather) 

## Adafruit FONA 3G Cellular + GPS Breakout
https://learn.adafruit.com/adafruit-fona-3g-cellular-gps-breakout/overview

The Adafruit FONA 3G may not be the perfect match for this project considering a Feather packaging of a FONA exists, but this is the one I will be using in this project due to availability of this product.

This device also need the followings:
  - An active GPS Antenna (https://www.adafruit.com/product/960) (Make sure to solder the Active antenna modification on your FONA 3G board. (little pads under the GPSANT silk label)
  - A enabled SIM card from any IOT Services provider, I used Hologram (https://hologram.io/) They offer a free 1MB/Month Data Roaming on your first prototype card, but be careful, the SMS and Calls aren't free and will be added to our balance.
  
# Programming the HUZZAH32
This must be the easiest part of this project considering how well done the integration was made with different IDE. In my projects, I always use on of the following IDE to start developping with ESP32:
  
  1. Arduino IDE - https://github.com/espressif/arduino-esp32
  2. Atom IDE/Platformio - https://randomnerdtutorials.com/programming-esp32-with-atom-text-editor-and-platformio-ide/
  3. Visual Code Studio/Platformio -https://marketplace.visualstudio.com/items?itemName=platformio.platformio-ide
  
For this project, I will be using Visual Code Studio most of the time except for quick tests that may be done in Arduino IDE.

# Testing FONA 3G
Refer to this guide: https://learn.adafruit.com/adafruit-fona-3g-cellular-gps-breakout/direct-usb-connection
Direct USB connection will allow you to simply send AT-Commands to your device and verify it's functionalities.

The reference AT Commands Manual can be found here: http://www.mt-system.ru/sites/default/files/simcom_sim5320_atc_en_v1.23.pdf

Here are some AT Commands I consider to be most useful:

| AT Command  | Definition | Example | Output |
| ------------- | ------------- | ------------- | ------------- |
| AT  | Verify AT commands communication availability | AT | OK |
| ATI  | Request Module's factory information  | ATI | Manufacturer: SIMCOM INCORPORATED Model: SIMCOM_SIM5320A Revision: SIM5320A_V1.5 IMEI: 014682000000000 +GCAP: +CGSM,+DS,+ES |
| ATZ | Factory Reset Module, your best friend when messing up with random commands. | ATZ | OK |
| AT+CGACT=1 | Enable PDP Context | AT+CGACT=1 | OK |
| AT+CGDCONT? | Get PDP Context Value | AT+CGDCONT? | +CGDCONT: 1,"IP","hologram","0.0.0.0",0,0 |
| AT+CGDCONT=<cid>[,<PDP_type>
[,<APN>[,<PDP_addr>
[,<d_comp>[,<h_comp>]]]]] | Set PDP Context Value | AT+CGDCONT=1,1,"hologram","",0,0 | OK |
  
  ##GPS
  Note that the GPS commands will output the GPS values in a DDmm.mmmmmmO as default format. 
  The equation to get the degrees only equation is DD+mm.mmmmmm/60.0 * (O=='W'||O=='S'?-1:1)


