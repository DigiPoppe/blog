---
layout: default
title: nrf905
description: NRF905 with ESP8266
---

# Using nRF905 with esp8266-12E

This is how i succeeded using the nRF905 with an esp8266-12E development board.

When using some examples from internet it was not clear to me which pins to connect and 
how to use the given examples. Then i found [RadioHead Packet Radio library for 
embedded microprocessors](http://www.airspayce.com/mikem/arduino/RadioHead/index.html). 
After download and unzip, i placed the files in the Arduino library directory. The examples
were then available in the Arduino IDE. With a few modifications to the example code i got 
it up and running.

The RadioHead file `RH_NRF905.h` assumes these connections:

Connect the nRF905 to Teensy (or Arduino with suitable level shifters) like this

```
                 CPU          nRF905 module
                 3V3----------VCC   (3.3V)
             pin D8-----------CE    (chip enable in)
             pin D9-----------TX_EN (transmit enable in)
          SS pin D10----------CSN   (chip select in)
         SCK pin D13----------SCK   (SPI clock in)
        MOSI pin D11----------MOSI  (SPI Data in)
        MISO pin D12----------MISO  (SPI data out)
                 GND----------GND   (ground in)
```


The esp8266-12E has two SPI ports, but which one is used bij RadioHead? And which pin is 
the SS line? Well, these are the settings which worked for me:
 
 ```
             ESP8266          nRF905 module
                 3V3----------VCC   (3.3V)
             pin D1-----------CE    (chip enable in)
             pin D2-----------TX_EN (transmit enable in)
          SS pin D4-----------CSN   (chip select in)
        HSCK pin D5-----------SCK   (SPI clock in)
        MOSI pin D7-----------MOSI  (SPI Data in)
        MISO pin D6-----------MISO  (SPI data out)
                 GND----------GND   (ground in)
```

Because of changes to default pins connected to CE, TX_EN and CSN, i had to adapt 
this in the example code. The RadioHead library supports this in the init function. 

```c++
// Singleton instance of the radio driver
uint8_t chipEnablePin  = D1;
uint8_t txEnablePin    = D2;
uint8_t slaveSelectPin = D4;
RH_NRF905 nrf905(chipEnablePin, txEnablePin, slaveSelectPin);
```

In my setup i used the nrf905_client on a Lolin esp8266-12E board and the nrf905_server on
a NodeMCU esp8266-12E board.


