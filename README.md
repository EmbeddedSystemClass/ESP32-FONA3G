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

## Testing FONA 3G
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
  
## Using the GPS
  Note that the GPS commands will output the GPS values in a DDmm.mmmmmm,O as default format. 
  The equation to get the degrees only equation is DD+mm.mmmmmm/60.0 * (O=='W'||O=='S'?-1:1)
  
  1. Make sure your Active antenna is connected and that you are in a signal accessible environnement (Not in a bunker), for example, in my office, I couldn't get a fix. Only by taking the device in the parking lot could I get a result. But at home in my kitchen, close to the windows and doors and exterior in general, I had a pretty good and quick fix with a 2m precision in not time.
  2. Either manually enable the GPS using AT+CGPS=1 (AT+CGPS? must return 1,1 if enabled) or set the AT+CGPSAUTO=1 to make the device auto boot it's gps anytime the power is on.
  3. call AT+CGPSINFO to get a DDmm.mmmmmm,O location reading.

## Enabling your 3G module and SIM card

Here are the steps I followed to assure that my device is up and running and can access network services:
1. Buy a SIM Card and insert it in your device. Mine was bought from Hologram so the guide can be followed here: https://hologram.io/docs/guide/connect/connect-device/
2. Once the card is activated on your hologram dashboard, it should show that the card status is Live (or green light). This means your device access as been allowed but your device has yet to establish a first contact with its service provider. 
3. Access the AT Commands mode and start by verifying your AT and ATI commands.
4. use: AT+CSQ to verify your signal strength (second number) as -XXdBm (The smaller the better, 115 being the worst.)
4. Set your APN context as follow: 
- AT+CGDCONT=1,"IP","hologram","",0,0   // This sets the context
- AT+CGATT=1                            // This will try to force the attachement to the network service provider.
- AT+CGACT=1                            // This request to active the context. this line needs to be called every time the device looses network service.
- AT+CGPADDR                            // If all went well, this command should return you a valid address such as 10.155.XXX.XXX or else but not 0.0.0.0
5. If all went well. your device should now be connected and ready to make some sweet HTTP/MQTT, or whatever protocol you prefer, requests.
