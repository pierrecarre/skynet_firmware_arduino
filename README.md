```
 SSSSS  kk                            tt
SS      kk  kk yy   yy nn nnn    eee  tt
 SSSSS  kkkkk  yy   yy nnn  nn ee   e tttt
     SS kk kk   yyyyyy nn   nn eeeee  tt
 SSSSS  kk  kk      yy nn   nn  eeeee  tttt
                yyyyy
```

Skynet Firmware allows you to connect to Skynet.im via your Arduino and an Arduino ethernet or wifi shield (or any other device that properly implements the Client interface)
 * http://arduino.cc/en/Main/ArduinoEthernetShield
 * http://arduino.cc/en/Main/ArduinoBoardEthernet
 * http://arduino.cc/en/Main/ArduinoWiFiShield
 * http://www.adafruit.com/products/1491
 * https://pinocc.io/
 * https://www.spark.io/

##Videos
* https://www.youtube.com/watch?v=oQzWKPER_ic
* https://www.youtube.com/watch?v=ZJNlqZXbrbM

##Install
* Clone or download and unzip.
* Rename the resulting folder to remove any - characters, and place folder in the libraries directory of your Arduino sketch directory. EX ~/Documents/Arduino/libraries/skynet_firmware_arduino_master
* Also requires the ArduinoJsonParser library https://github.com/bblanchon/ArduinoJsonParser  
* After adding both, Close/reopen arduino and if all is well you'll find File->Examples->SkynetClient

##Examples
Find full examples in the File->Examples->SkynetClient menu but generally, theres 3 ways to use: onMessage, log message,  or bind.
###onMessage
The simplest method is to register a function to be called when Skynet receives a message for your device:
```cpp
void setup()
{
  ...
  skynetclient.setMessageDelegate(onMessage);
}
void loop(){
  skynetclient.monitor();
}
```

Then you can do whatever you want with that message including json parse it or string match it:
```cpp
void onMessage(const char * const data) {
  JsonParser<16> parser;

  Serial.print("Parse ");
  Serial.println(data);

  JsonHashTable hashTable = parser.parseHashTable((char*)data);

  if (strcmp(payload, "on") == 0)
  {
      digitalWrite(LEDPIN, HIGH)
  }else{
      digitalWrite(LEDPIN, LOW)
  }
}
```
###Log
Even simpler than that is to just send data to Skynet with the logMessage command: 
```cpp
void loop(){
    //Craft a string with your data like "light":"423","temp":"356"
  skynetclient.logMessage(message);
}
```
Now you can subscribe to your data elsewhere. See the api page for rest examples with Rest, Javascript, and more! http://skynet.im/#api
###Bind
Lastly, we've created the ability to bind 2 devices to just Skynet like a virtual serial cable. This is good for passing lots of information between devices without all the addressing overhead. You can read the data out of the Skynet Client just like a Serial device:
```cpp
void loop() {
  skynetclient.monitor();
  while(skynetclient.available())
  	Serial.print(skynetclient.read());
}
```
This can be seen in our Firmata sketch paired with the Node example in the node_client directory. (see also: https://www.npmjs.org/package/skynet-serial)

LICENSE
-------

(LGPL License)

Copyright (c) 2014 Octoblu <info@octoblu.com>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
