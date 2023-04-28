# ESP-8266 Deauther
Today we're doing something different. We will leave the computer for a while and make something with our hands. Let's make the [Wi-Fi Deauther from Spacehuhn](https://github.com/SpacehuhnTech/esp8266_deauther). Check [his Github](https://github.com/SpacehuhnTech), it's awesome all the stuff he makes!

## List of Components
I'm going to keep it simple and make just a prototype to see if I make it work. Later, once I have time, I will make a better version of it. 

 - **NodeMCU ESP-8266** (obviously)
 - **Protoboard**
 - **Wire jumpers**
 - **USB cable**
 - **OLED display** (optional)
 - **Buttons** (optional right now)

![Materials](/Process%20pictures/materials.jpg)

## Display and Buttons Setup
The hardware setup might not be always the first place to start but there is a reason. The first thing you need to know is what GPIOs you're using for the display and the buttons so, in case you use different ones, you will have to change a bit the code.

![Initial setup](/Process%20pictures/initial-setup.jpg)

In my case, as this is a prototype and I only want to kick the tyres and see if it works, I'm going for the default setup. You can read more about different setups [here.](https://deauther.com/docs/diy/display-setup)

I'm using an **SSD1306 OLED display** and it's the **I2C (4 pins) version**. This display is pretty easy and straight forward to setup. 

![Display power](/Process%20pictures/display-power.jpg)

|Display|ESP-8266 pin|
|--|--|
|GND|GND|
|VCC|3V|
|SCL|D2|
|SDA|D1|

![Display setup](/Process%20pictures/display-setup.jpg)

For the butthons, as I didn't have any, I had to improvise something:

![Improvised buttons](/Process%20pictures/improvised-buttons.jpg)

I connected some jump wires following the default setup + another ground jumper to make contact and simmulate a button touch. Color code might be a bit messy but, forgetting about the display:

|Color|Action|ESP-8266 pin| 
|--|--|--|
|Green|UP|D5|
|Blue|DOWN|D6|
|Orange|A|D7|

Not beautiful but it works for a prototype so let's upload the code then!

## Uploading the code
I decided to do this step by downloading the code and uploading it using Arduino IDE. But before we have to configure Arduino IDE to be able to talk to our NodeMCU board. So just follow [Spacehuhn's steps](https://deauther.com/docs/diy/installation-arduino) for it:

1. Download the code [here.](https://deauther.com/docs/download) 
2. Extract the ESP8266 Deauther zip.
3.  Go into the  `esp8266_deauther`  folder and open  `esp8266_deauther.ino`  with Arduino IDE
4.  In Arduino IDE, go to  `File`  >  `Preferences`  and add this URL to the  `Additional Boards Manager URLs`:  `https://raw.githubusercontent.com/SpacehuhnTech/arduino/main/package_spacehuhn_index.json`
5.  Now go to  `Tools`  >  `Board`  >  `Boards Manager`, search  `deauther`, and install  `Deauther ESP8266 Boards`
6.  Select your board at  `Tools`  >  `Board`  and be sure it is at  `Deauther ESP8266 Boards`  (and  **not**  at  `ESP8266 Modules`)!
7.  Plugin your Deauther and select its COM port at  `Tools`  >  `Port`
8.  Optional: To reset/override previous settings select  `Tools`  >  `Erase Flash`  >  `All Flash Contents`
9.  Press upload

> **Note:** As I am using an I2C display, I selected the sample code in **Tools > Deauther config > Display Example I2C**.

Now everything should be ready to go!

But it didn't. I had some troubles with the display. I managed to solve them and all of that is in the troubleshooting section.

## Troubleshooting

My display was not working. So first thing I did? Check the connections. Everything looked fine and I tried it on an arduino UNO to make sure it works and it did.

Problem was the code.

Original code of **A_config.h**:

    #elif defined(DISPLAY_EXAMPLE_I2C)
    
    // ===== DISPLAY ===== //
    //#define SH1106_I2C
      #define SSD1306_I2C
    
      #define I2C_ADDR 0x3C
      #define I2C_SDA 5
      #define I2C_SCL 4
    
    // #define FLIP_DIPLAY true

Of course you have to uncomment the display you are using. But, as well, you need to add one more thing. Here is the code with what I made it work:

    #elif defined(DISPLAY_EXAMPLE_I2C)
    
    // ===== DISPLAY ===== //
      //#define SH1106_I2C
      #define SSD1306_I2C
    
      #define I2C_ADDR 0x3C
      #define I2C_SDA 5
      #define I2C_SCL 4
    
      #define USE_DISPLAY true
      #define FLIP_DIPLAY true

Just add `#define USE_DISPLAY true` and uncomment `#define FLIP_DIPLAY true`. It should work now!

## Testing the Deauther!
Now let's plug it back into the computer to power it on... Hurray! *Habemus Deauther!* 

![Deauther](/Process%20pictures/deauther.jpg)

I tried it with my IoT network and it disconnected all the smart bulbs from the Wi-Fi in a few seconds. Now it's time to play around a bit more and start thinking about a case to place it definitely.

I will be posting updates and improvements as I manage to find time to make them!

Have a nice day and happy Deauthing!
