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
  - An active GPS Antenna (https://www.adafruit.com/product/960)
  - A enabled SIM card from any IOT Services provider, I used Hologram (https://hologram.io/) They offer a free 1MB/Month Data     Roaming on your first prototype card, but be careful, the SMS and Calls aren't free and will be added to our balance.
