# SM23-IOT-01

IOT task 2 (Connection of robot control panel with ESP32)

# Introduction
Using an Arduino IDE Programmer, we have connected the ESP32 to a power source and a control panel, also wrote the code to connect it to the Wi-Fi network, Then connect the four output pins of the microcontroller to four LEDs, each with it's own resistor in the series, in addition we programmed the ESP32 to turn on a specific LED when the robot moves in a specific direction, and turn off the others LED's simultaneously, after wards the robot should stops instead of the LED's we are using to turn off, finally we assign each LED to a specific direction of the robot movement (forward, backward, right, left, then stop all of it and repeat) and turn on the corresponding LED when the robot moves in the chosen direction, by turning off the LED when the robot stops. 

# The Components:
1. ESP32
2. 4 LEDS
3. 4 resistors
3. Wires to  connect

# Code explanation:

We start the code with some include statements that import the libraries needed for WiFi, HTTP and serial communication. Then it defines a macro USE_SERIAL that refers to the Serial object. Next, it creates a WiFiMulti object that can handle multiple WiFi networks. Then it defines the setup function that runs once when the Arduino board is connected with the ESP32. In this function, it sets four pins (27, 25, 26 and 18) as output pins, initializes the serial connection with a rate of 115200, prints some messages to the serial monitor from the web link given by smart methods, and adds a WiFi network with its SSID and password to the WiFiMulti object. Finally, it defines the loop function that runs repeatedly after the setup function.

In this function, it checks if the WiFi connection is established, and if so, it creates an HTTPClient object that can send and receive HTTP requests. It uses this object to send GET requests from five different URLs: (https://s-m.com.sa/b.html, https://s-m.com.sa/f.html and https://s-m.com.sa/r.html, https://s-m.com.sa/l.html, https://s-m.com.sasb.html) These URLs are supposed to return either "backward", "forward", "right", "left" or "stop" as the response body, indicating the direction that the robot should move. The code block then reads the response body and prints it to the serial monitor. Depending on the response, it sets the output pins to HIGH or LOW to control the robot's movement. 

For example, if the response is "backward", it sets pin 27 to HIGH and the rest to LOW, which makes the robot move backward. If the response is "forward", it sets pin 25 to HIGH and the rest to LOW, which makes the robot move forward. If the response is "right", it sets pin 26 to HIGH and pin 18 to LOW, which makes the robot turn right. The code block repeats this process for each URL in each loop iteration.

# Code for Connection the robot control panel with ESP32 throw wifi:
```
#include <Arduino.h>
#include <WiFi.h>
#include <WiFiMulti.h>
#include <HTTPClient.h>

#define USE_SERIAL Serial

WiFiMulti wifiMulti;

void setup() {
  pinMode(27,OUTPUT);
  pinMode(25,OUTPUT);
  pinMode(26,OUTPUT);
  pinMode(18,OUTPUT);

    USE_SERIAL.begin(115200);
    USE_SERIAL.println();
    USE_SERIAL.println();
    USE_SERIAL.println();

    for(uint8_t t = 4; t > 0; t--) {
        USE_SERIAL.printf("[SETUP] WAIT %d...\n", t);
        USE_SERIAL.flush();
        delay(1000);
    }

    wifiMulti.addAP("Smart_Methods1", "123456789");

}

void loop() {
    // wait for WiFi connection
    if((wifiMulti.run() == WL_CONNECTED)) {

        HTTPClient http;

        USE_SERIAL.print("[HTTP] begin...\n");
        // configure traged server and url
        //http.begin("https://www.howsmyssl.com/a/check", ca); //HTTPS
        http.begin("https://s-m.com.sa/b.html"); //HTTP

        USE_SERIAL.print("[HTTP] GET...\n");
        // start connection and send HTTP header
        int httpCode = http.GET();

        // httpCode will be negative on error
        if(httpCode > 0) {
            // HTTP header has been send and Server response header has been handled
            USE_SERIAL.printf("[HTTP] GET... code: %d\n", httpCode);

            // file found at server
            if(httpCode == HTTP_CODE_OK) {
                String payload = http.getString();
                USE_SERIAL.println(payload);
                if (payload=="backward"){
                  digitalWrite(27, HIGH);
                  digitalWrite(25,LOW);
                  digitalWrite(26,LOW);
                  digitalWrite(18,LOW);
                }
    
                 }
        } else {
            USE_SERIAL.printf("[HTTP] GET... failed, error: %s\n", http.errorToString(httpCode).c_str());
        }

        http.end();
    }

    if((wifiMulti.run() == WL_CONNECTED)) {

        HTTPClient http;

        USE_SERIAL.print("[HTTP] begin...\n");
        // configure traged server and url
        //http.begin("https://www.howsmyssl.com/a/check", ca); //HTTPS
        http.begin("https://s-m.com.sa/f.html"); //HTTP

        USE_SERIAL.print("[HTTP] GET...\n");
        // start connection and send HTTP header
        int httpCode = http.GET();
     if(httpCode > 0) {
            // HTTP header has been send and Server response header has been handled
            USE_SERIAL.printf("[HTTP] GET... code: %d\n", httpCode);

            // file found at server
            if(httpCode == HTTP_CODE_OK) {
                String payload = http.getString();
                USE_SERIAL.println(payload);
                if (payload=="forward"){
                  digitalWrite(27, LOW);
                  digitalWrite(25, HIGH);
                  digitalWrite(26, LOW);
                  digitalWrite(18, LOW);
                }
    
                 }
        } else {
            USE_SERIAL.printf("[HTTP] GET... failed, error: %s\n", http.errorToString(httpCode).c_str());
        }

        http.end();
    }

    if((wifiMulti.run() == WL_CONNECTED)) {

        HTTPClient http;

        USE_SERIAL.print("[HTTP] begin...\n");
        // configure traged server and url
        //http.begin("https://www.howsmyssl.com/a/check", ca); //HTTPS
        http.begin("https://s-m.com.sa/r.html"); //HTTP

        USE_SERIAL.print("[HTTP] GET...\n");
        // start connection and send HTTP header
        int httpCode = http.GET();

     if(httpCode > 0) {
            // HTTP header has been send and Server response header has been handled
            USE_SERIAL.printf("[HTTP] GET... code: %d\n", httpCode);

            // file found at server
            if(httpCode == HTTP_CODE_OK) {
                String payload = http.getString();
                USE_SERIAL.println(payload);
                if (payload=="right"){
                  digitalWrite(27, LOW);
                  digitalWrite(25, LOW);
                  digitalWrite(26, HIGH);
                  digitalWrite(18, LOW);
                }
    
                 }
        } else {
            USE_SERIAL.printf("[HTTP] GET... failed, error: %s\n", http.errorToString(httpCode).c_str());
        }

        http.end();
    }

    if((wifiMulti.run() == WL_CONNECTED)) {

        HTTPClient http;

        USE_SERIAL.print("[HTTP] begin...\n");
        // configure traged server and url
        //http.begin("https://www.howsmyssl.com/a/check", ca); //HTTPS
        http.begin("https://s-m.com.sa/l.html"); //HTTP

        USE_SERIAL.print("[HTTP] GET...\n");
        // start connection and send HTTP header
        int httpCode = http.GET();

     if(httpCode > 0) {
            // HTTP header has been send and Server response header has been handled
            USE_SERIAL.printf("[HTTP] GET... code: %d\n", httpCode);

            // file found at server
            if(httpCode == HTTP_CODE_OK) {
                String payload = http.getString();
                USE_SERIAL.println(payload);
                if (payload=="left"){
                  digitalWrite(27, LOW);
                  digitalWrite(25, LOW);
                  digitalWrite(26, LOW);
                  digitalWrite(18, HIGH);
                }
    
                 }
        } else {
            USE_SERIAL.printf("[HTTP] GET... failed, error: %s\n", http.errorToString(httpCode).c_str());
        }

        http.end();
    }

    delay(1000);
    if((wifiMulti.run() == WL_CONNECTED)) {

        HTTPClient http;

        USE_SERIAL.print("[HTTP] begin...\n");
        // configure traged server and url
        //http.begin("https://www.howsmyssl.com/a/check", ca); //HTTPS
        http.begin("https://s-m.com.sa/s.html"); //HTTP

        USE_SERIAL.print("[HTTP] GET...\n");
        // start connection and send HTTP header
        int httpCode = http.GET();

        // httpCode will be negative on error
        if(httpCode > 0) {
            // HTTP header has been send and Server response header has been handled
            USE_SERIAL.printf("[HTTP] GET... code: %d\n", httpCode);

            // file found at server
            if(httpCode == HTTP_CODE_OK) {
                String payload = http.getString();
                USE_SERIAL.println(payload);
                if (payload=="stop"){
                  digitalWrite(27, LOW);
                   digitalWrite(25,LOW);
                   digitalWrite(26,LOW);
                   digitalWrite(18,LOW);
                }
    
                 }
        } else {
            USE_SERIAL.printf("[HTTP] GET... failed, error: %s\n", http.errorToString(httpCode).c_str());
        }

        http.end();
    }

}
```
