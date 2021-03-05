# Internet of Bananas
## Instalation Guide

The **Internet of Bananas** (IoB) is a critical design project in which people around the World will build simple device to measure the colour of a banana and the temperature and humidity of the air around it for an entire week. During the project, researchers, tinkerers and activists around the Globe will create their own “augmented bananas” and track them during their lifetime, until they are rotten. The data collected by the sensors will then be visualised online on Adafruit IO, where each participant will showcase the information of their individual banana and will be able to access a World map of the entire Internet of Bananas.

---

The IoB is based on the microcontroller NodeMCU Esp8266 with the color sensor TCS3200 and the temperature and humidity sensor DHT11, and thru a WiFi connection sends the data to the internet, in the Adafruit IO platform. The Adafruit IO is server service based on [MQTT protocol](https://en.wikipedia.org/wiki/MQTT), and has an accessible library that can be used with the NodeMCU Esp8266, programing in the Arduino IDE. The Arduino IDE is the software to write and to upload the code to microcontrollers. 

To publish the data in Adafruit IO it's necessary to create an account - you can create one in this link: [https://accounts.adafruit.com/users/sign_up](https://accounts.adafruit.com/users/sign_up). Then you will have a user name and key that will be used in the code to publish the data. In the Adafruit IO vocabulary, the data's name is called *feed*, so there will be three feeds, color, temperature and humidity. Only you have control on the data that is published in your account - in the appropriate time, we will kindly ask you to set your feeds as *public*, so we can show them in the global map on our website. =)

![Schematic diagram of the Internet of Bananas](https://drive.google.com/uc?export=view&id=1LpIFhPiTVFzD0ErvvocMIws-D-d8hxrt "Schematic diagram of the IoB")

We have already developed the code to be used in the IoB project, however each participant has to update it with the following information:
- the name and the password of your Wi-Fi network to connect to the internet;
- the user name and key of your Adafruit IO account, so it can publish the feeds;
- the microcontroller pins numbers that you are using to plug the sensors;
- the color calibration parameters.

We will explain how to do the all these updates, and you can also read the comments in the [code](https://github.com/Internet-of-Bananas/code) that may help you to understand them. Just to be clear, you are not asked to share your personal information (user name, key and passwaord), you will do the above updates by yourself, in your computer. 

After updating the code, your IoB station is ready for the Banana Jam!

**Attention**: The IoB devices contains fragile parts, be careful when handling them. Do not place them in a metal or conductive surface, you can short-circuit the microcontroller and sensors, and damage them.

## 1. Installation of Arduino IDE
To update the code it is necessary to download the *Arduino IDE* - the software that allows writing and sending code to the Arduino microcontroller, NodeMCU Esp8266, among others.

Access https://www.arduino.cc/en/Main/Software and download Arduino according to your operating system. When the download is complete, install Arduino. During the installation, confirmations may appear to install the Arduino components, if these warnings appear, confirm the installation.

Along with the Arduino software, USB drivers are installed, however depending on the operating system, it may be that the USB driver for your NodeMCU Esp8266 has not been installed.

To check, connect the USB cable to your NodeMCU board, and the other end of the cable connect to the computer's USB port. Again, depending on the operating system, it may automatically recognize and install the driver, if it's your case, wait until this process finishes. 

Then open the Arduino IDE, select the `Tools > Port` menu and observe the *Com* ports listed. Press `Esc` to exit the selected menu. Disconnect the board's cable from the USB port. Again, select the `Tools > Port` menu, and note which *Com* port is no longer present, the one that dissapeared is the *Com* port of your board. If it continues with the same *Com* ports listed, or if there's none, the NodeMCU Esp8266 driver has not been installed. In this case, follow the procedure in item *1.1. CH340 driver installation*.

### 1.1. CH340 driver installation
If the NodeMCU Esp8266 driver was not installed, you should try to install the CH340 Driver. For that, exit Arduino IDE and download the file according to your operating system:
- [Linux](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#linux)
- [Windows](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#windows-710)
- [Mac OSX](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#mac-osx)

Follow the instruction and install the driver. If an alert window appears, confirm the installation of the file. After installing the driver, close the window.

To check if the installation was well succeded, open the Arduino IDE, select the `Tools > Port` menu and observe the *Com* ports listed. Press `Esc` to exit the selected menu. Disconnect the board's cable from the USB port. Again, select the `Tools > Port` menu, and note which *Com* port is no longer present, the one that dissapeared is the *Com* port of your board.

### 1.2. Add the NodeMCU Esp8266 board 
To work with the Esp8266 microcontrollers on the Arduino IDE it is necessary to install a board package. If the Arduino IDE is not open, open it, then select `File > Preferences` menu to open the preferences window. In the field `Additional Boards Manager URLs`, add: `https://arduino.esp8266.com/stable/package_esp8266com_index.json`. Click `OK`to close the window.

Then select the menu `Tools > Boards > Boards Manager`. In the search field, type `esp8266`, and install` ESP8266 by ESP8266 Community`.

After completing the installation, connect the USB cable to the NodeMCU, and the other end of the cable connect to the computer's USB.

In `Tools > Boards` menu, select the board `NodeMCU 1.0 (ESP-12E Module)` from the list. Again in the `Tools > Port` menu, select the *Com* port of your NodeMCU board.

### 1.3. Verify the installation
To test whether the installation was successful, open the menu `File > Examples > 01. Basics > Blink`.

Then select the menu command `Skecth > Upload`, or press `Ctrl + U` or `Command+U` in Mac computer. The Blink skecth will be sent to the microcontroller, and the Esp8266's internal LED will flash in a interval of 1000 milliseconds or 1 second.

To experiment with different intervals, replace the value of `delay (1000);` for example:

```// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}
```

After the changes in the *delay*, it is necessary to update the Esp8266 by uploading the code to the board, with the command in the menu `Skecth > Upload` or press `Ctrl + U` or `Command + U` in Mac computer.

## 2. Add libraries to the Arduino IDE
Libraries are additional codes that extend the functionality of the Arduino IDE for use with modules, sensors, among others, developed and shared by collaborators. In the Internet of Bananas you will need to install the following libraries:

- [EWMA](https://github.com/jonnieZG/EWMA), Exponentially Weighted Moving Average, by Arsen Torbarina, to filter noise of the color sensor;
- [Adafruit_MQTT](https://github.com/adafruit/Adafruit_MQTT_Library), by Adafruit, to access and publish data in the Adafruit IO server;
- [DHT sensor library](https://github.com/adafruit/DHT-sensor-library) by Adafruit, to use with the DHT11 sensor. 

To install the libraries on the Arduino IDE, select the menu `Sketch > Include Library > Manage Libraries`. In the search field, type the name of the library EWMA, then click the `Install` button to install the library. Repeat the same procedure to install the Adafruit_MQTT library and DHT sensor library. If there's a warning asking to install additional libraries, confirm yes to install the complementary itens.

## 3. Updating the code
In the respository [https://github.com/Internet-of-Bananas/code](https://github.com/Internet-of-Bananas/code) there are six code files, that increase gradually the complexity:
- 1_iobColorTest is a test to use the color sensor;
- 2_iobColorFilter continues the previous sketch and add a color filter using the EWMA library;
- 3_iobColorRGBhex converts the color number in hexadecimal values;
- 4_iobDHT11Test tests the temperature and humidity sensor, using the DHT sensor library;
- 5_iobColorDHT joins the color and temperature sketch;
- 6_iob that is the final code for the IoB, using the MQTT library to publish the data.

It is presented below how to update the file **6_iob** to use it in the Internet of Bananas. The code is available at [https://github.com/Internet-of-Bananas/code](https://github.com/Internet-of-Bananas/code). On the GitHub page, click the `Code > Download ZIP` button. After downloading, unzip the files, then copy the folders to the preferred location on your computer. On the Arduino IDE, select the `File > Open` menu and open the `6_iob.ino` file, located in the previously copied folder. 

Note: in Arduino IDE the folder and the file must have the same name, so if you renamed the folder, you have to rename the file `.ino` with the same name.

Before sending the code to ESP8266, you need to make three changes:

- Name and password of the WiFi network, so that the device can connect to the internet;
- Adafruit IO server account login and key;
- Number the data feeds according to the respective station number.

### 3.1. The WiFi credentials
To connect to the internet you need to specift the name and password of your WiFi network. To update the sketch with your name and password, locate the code below:

```/************************* WiFi Access Point *********************************/

#define WLAN_SSID      "wifiName"     //  Name of the WiFi network.
#define WLAN_PASS      "wifiPassword" //  WiFi password.
```

Replace the `wifiName` with your WiFi name, and `wifiPassword` with your WiFi password, keeping the quotes. Write exactly how it is, with space and upper and lower case, if there's any.

### 3.2. Adafruit IO credentials
To access the Adafruit IO server you need to specify the user name and the key. If you don't have one, you can create it here [// https://accounts.adafruit.com/users/sign_up](https://accounts.adafruit.com/users/sign_up). To update the sketch, locate the code below:

```/************************* Adafruit.io Setup *********************************/
// You need an account at the Adafruit IO to publish data.
// If you don't have it, you can create one at
// https://accounts.adafruit.com/users/sign_up

#define AIO_SERVER      "io.adafruit.com"       //  Address of the MQTT server from Adafruit IO, don't change.
#define AIO_SERVERPORT  1883                    //  Number of server port from Adafruit IO.
#define AIO_USERNAME    "username"  //  Adafruit IO user name.
#define AIO_KEY         "key"    //  Adafruit IO user key.
``` 

Replace `username` with the user name of your account at Adafruit IO, and `key` with your account's key.

### 3.3. Upload the code to the NodeMCU Esp8266
After completing the code updates, connect the NodeMCU board to the computer with the USB cable. In the Arduino IDE, select the menu `Skecth > Upload`, or press `Ctrl + U` or `Command + U`. 

To check if the code updates were successful, select `Tools > Serial monitor`. In the window that opens, in the lower right corner, select speed `9600`.

As the data is sent, the confirmation `Ok, sent!` is displayed for each feed in the Serial Monitor window. If the data is not being sent or there's an error, check if the name and password of the WiFi network and user name and key of your account at Adafruit IO are correct.

**As a form of visual confirmation, the ESP8266 blue LED flashes when data is being sent.**

In addition to these verification modes, you can check the feeds at [Adafruit IO](https://io.adafruit.com/) to see if the data is arriving.

After confirming that the updates have been well succeeded, you are ready to go for the Banana Jam!

## 4. Create a dashboard at Adafruit IO
To visualize the data from your IoB you can create a dashboard at Adafruit IO platform.

## 5. Assembling the device on the banana
The NodeMCU board and the sensors should be attached to the banana...
