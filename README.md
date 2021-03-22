# Internet of Bananas
## Installation Guide

The **Internet of Bananas** (IoB) is a critical design project in which people around the World will build simple device to measure the color of a banana and the temperature and humidity of the air around it for an entire week. During the project, researchers, tinkerers and activists around the Globe will create their own “augmented bananas” and track them during their lifetime, until they are rotten. The data collected by the sensors will then be visualised online on Adafruit IO, where each participant will showcase the information of their individual banana and will be able to access a World map of the entire Internet of Bananas.
In this guide, we will explain in detail how to build the sensors to attach to the banana and how to visualise and share the data produced online.

---
## 1. Introducion 

The IoB is based on a sensors and a software. 
The IoB sensor composed by a microcontroller NodeMCU Esp8266, a color sensor TCS3200 and a temperature and humidity sensor DHT11. 
The IoB software is a code written in Arduino IDE (the software to write and to upload the code to microcontrollers). It activates the different sensors, connects to your local WiFi connection, collects data from the sensors and sends them to the Adafruit IO platform. Adafruit IO is a server service based on the [MQTT protocol](https://en.wikipedia.org/wiki/MQTT). It has an accessible library that can be easily used by our NodeMCU Esp8266.

To build your IoB device, you only need the [required components](https://github.com/Internet-of-Bananas/material-list), and to follow the instructions in this guide.

As for the software, we have already developed the code to be used in the IoB project. To make it work, each participant has first to update it with the following information:
- the microcontroller pins numbers that used to plug the sensors;
- the color calibration parameters;
- the name and the password of their Wi-Fi network to connect to the internet;
- the user name and key of their Adafruit IO account, so it can publish the feeds.

We explain how to obtain the Adafruit credentails in the next section [2. Before Starting](https://github.com/Internet-of-Bananas/installation-guide#Before-starting), and how to do the updates in the section [3. Updating the code](https://github.com/Internet-of-Bananas/installation-guide#3-updating-the-code). The [code](https://github.com/Internet-of-Bananas/code) is also commented, so to help the users understand how to complete it. 

NB you are not asked to share any personal information (user name, key and passwaord), you will do the above updates by yourself, in your computer. 

**Attention**: The IoB devices contains fragile parts, be careful when handling them. Do not place them on a metal or conductive surface, you can short-circuit the microcontroller and sensors, and damage them. Do not place the microcontrollers and the sensors in a wet environment, water can damage them and you may risk injury.

## 2. Before starting
All the data produced from the IoB sensors will be visualised on a platform called Adafruit IO. This will allow each participant to showcase their own data on a "dashboard", and the IoB organisers to create a World Map of the Internet of Bananas. 

To publish the data in Adafruit IO it's necessary to have an account - you can create one for free at this link: [https://accounts.adafruit.com/users/sign_up](https://accounts.adafruit.com/users/sign_up). Once you have an account, on the Adafruit IO website, click on the menu `My Key` to see your credentials (user name and key), that will be used in the code to publish the data online. 

Adafruit IO will recieve from your IoB station three sets of data (or *feeds*): color, temperature and humidity. Only you have control on the data that is published in your account - in the appropriate time, we will kindly ask you to set your feeds as *public*, so you can share them with your friends and we can show them in the global map on our website. =)

![Schematic diagram of the Internet of Bananas](https://drive.google.com/uc?export=view&id=1LpIFhPiTVFzD0ErvvocMIws-D-d8hxrt "Schematic diagram of the IoB")

NB when we mention *NodeMCU*, *ESP8266*, *board*, *microcontroller* for the sake of comprehension, it's the same thing.

## 3. Installation of Arduino IDE
In order to update the IoB code and install it in your sensor, it is necessary to download *Arduino IDE*. This simple software will allow you to write or modify code and to upload it to your NodeMCU Esp8266 microcontroller.

Access https://www.arduino.cc/en/Main/Software and download Arduino (choose the version for your operating system!). When the download is complete, install Arduino. During the installation, confirmations may appear asking if you want to install the Arduino components, if these warnings appear, confirm the installation.

Along with the Arduino software, the installer should also install the USB drivers necessary for your computer to recognise your NodeMCU board. However, depending on your operating system, it may be that the USB driver for your NodeMCU Esp8266 has not been installed.

To check, use the USB cable to connect your NodeMCU board to the computer's USB port. After you connected them, wait a few moments: depending on the operating system, it may automatically recognize and install the driver, if this is the case, wait until this process finishes. 

Regardless, it is now time to control if your Arduino IDE has installed the USB driver. For this step, the NodeMCU *has* to be connected you your computer. 
Open the Arduino IDE, select the `Tools > Port` menu and observe which *Com* ports are listed. Keep them in mind, or write them down on a piece of paper.
Press `Esc` to exit the selected menu. Disconnect the board's cable from the USB port. Again, select the `Tools > Port` menu, and check which *Com* port is no longer present, the one that dissapeared is the *Com* port of your board. If there is no difference between the *Com* ports listed, or if there's none, then the NodeMCU Esp8266 driver has not been installed. In this case, follow the procedure in item *1.1. CH340 driver installation*. Otherwise, go to 1.2.

### 3.1. CH340 driver installation
If the NodeMCU Esp8266 driver was not installed, you should try to install a driver called CH340. To do that, exit Arduino IDE and download the driver (always according to your operating system!):
- [Linux](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#linux)
- [Windows](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#windows-710)
- [Mac OSX](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#mac-osx)

Follow the instructions and install the driver. If an alert window appears, confirm the installation of the file. After installing the driver, close the window.

To check if the installation was succesful, follow the same procedure as above. Namely: open the Arduino IDE, select the `Tools > Port` menu and observe the *Com* ports listed. Press `Esc` to exit the selected menu. Disconnect the board's cable from the USB port. Again, select the `Tools > Port` menu, and note which *Com* port is no longer present, the one that dissapeared is the *Com* port of your board.

### 3.2. Add the NodeMCU Esp8266 board 
To work with the Esp8266 microcontrollers on the Arduino IDE it is necessary to install a board package. If the Arduino IDE is not open, open it, then select `File > Preferences` menu to open the preferences window. In the field `Additional Boards Manager URLs`, add: `https://arduino.esp8266.com/stable/package_esp8266com_index.json`. Click `OK`to close the window.

Then select the menu `Tools > Boards > Boards Manager`. In the search field, type `esp8266`, and install` ESP8266 by ESP8266 Community`.

After completing the installation, connect the USB cable to the NodeMCU, and the other end of the cable connect to the computer's USB.

In `Tools > Boards` menu, select the board `NodeMCU 1.0 (ESP-12E Module)` from the list. Again in the `Tools > Port` menu, select the *Com* port of your NodeMCU board.

### 3.3. Verify the installation
To test whether the installation was successful, open the menu `File > Examples > 01. Basics > Blink`.

Then select the menu command `Skecth > Upload`, or press `Ctrl + U` or `Command+U` on a Mac computer. The Blink skecth will be sent to the microcontroller, and the Esp8266's internal LED will flash in a interval of 1000 milliseconds (1 second). If your board is blinking, the installation has been succesful.

You can experiment with different intervals by replace the numerical value of `delay (1000);` for example:

```// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(500);                       // wait for a second
  digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
  delay(2000);                       // wait for a second
}
```

After you change the duration of the *delay*, update the Esp8266 by uploading the code to the board, with the command in the menu `Skecth > Upload` or press `Ctrl + U` or `Command + U` in Mac computer. You will now notice that the interval of the blinking of your Microcontrollor had changed.

Well done! You have now succesfully connecnted your NodeMCU to your computer!

### 3.4. Add libraries to the Arduino IDE
In otder for the IoB software to work, you will need to download some additional libraries. Libraries are additional codes that extend the functionality of the Arduino IDE for use with modules, sensors, among others, developed and shared by collaborators. In the Internet of Bananas you will need to install the following libraries:

- [EWMA](https://github.com/jonnieZG/EWMA), Exponentially Weighted Moving Average, by Arsen Torbarina, to filter noise of the color sensor;
- [Adafruit_MQTT](https://github.com/adafruit/Adafruit_MQTT_Library), by Adafruit, to access and publish data in the Adafruit IO server;
- [DHT sensor library](https://github.com/adafruit/DHT-sensor-library) by Adafruit, to use with the DHT11 sensor. 

To install the libraries on the Arduino IDE, select the menu `Sketch > Include Library > Manage Libraries`. In the search field, type the name of the library EWMA, then click the `Install` button to install the library. Repeat the same procedure to install the Adafruit_MQTT library and DHT sensor library. If there's a warning asking to install additional libraries, confirm yes to install all the complementary items.

Well done! Now you have all the required libraries for the IoB to work! 


## 4. Download the code!
The code for the IoB project is available in the respository [https://github.com/Internet-of-Bananas/code](https://github.com/Internet-of-Bananas/code). There are six code files, of increasing complexity. For your information, here what each of the codes does:

- 1_iobColorTest is a test to use the color sensor;
- 2_iobColorFilter continues the previous sketch and add a color filter using the EWMA library;
- 3_iobColorRGBhex converts the color number in hexadecimal values;
- 4_iobDHT11Test tests the temperature and humidity sensor, using the DHT sensor library;
- 5_iobColorDHT joins the color and temperature sketch;
- 6_iob that is the final code for the IoB, using the MQTT library to publish the data online.

Feel free to download and experiment with any of these codes. For the IoB to work you need the `6_iob`. 

To download the code, on the [GitHub page](https://github.com/Internet-of-Bananas/code), click the `Code > Download ZIP` button. After downloading, unzip the files, then copy the folders to the preferred location on your computer. 

Note: in Arduino IDE the folder and the file must have the same name, so if you rename the folder, you will have to rename the file `.ino` with the same name.

## 5. Connecting the hardware pins
The sensors are connected to your NodeMCU using the female-female jumper cables to plug in the respective pins. The NodeMCU board has many pins, but we will focus on those that can be used in the IoB project, that are the power rail and the digital pins. 

The power rail is the power supply, the voltage, composed of two pins, the positive and the ground. Normally they are printed on the board as `3V3` for the tension of 3.3 volts, and `GND` or `G` for *ground*. In both sensors it's necessary to connect the power in the correpondents pins. In some cases the positive pin may be writen on the board as `+` or `VCC`, and the ground as `-`, `G` or `GND`.

The NodeMCU Esp8266 has nine digital pins, usally printed as D0, D1, D2, ..., D8. However, there is another way to number the pins, using the General Purpose Input/Output or GPIO. For instance, the D0 correspond to GPIO16, the D1 to GP05 (yes, it's very strange). What is important to know is: we can use both ways. If you want to specify in the code the pin D0, you can write `D0` or `16`, or the pin D1, you can write `D1` or `5`.  If you want to see the GPIO pinout of the NodeMCU board,you can have a look [here](https://www.monster.com.tw/wp-content/uploads/2018/03/PINOUT-NodeMCU_1.0-V2-y-V3-1.png). 

**Be careful**: before you connect the jumper cable in the pins, remember to disconnect the NodeMCU board from the USB cable, to avoid connection in the wrong pin with the board powered on. It's good (or at least not bad) practice to always double check the wiring, to confirm you coonect the cable in the correct pins.

### 5.1. Connecting the TCS3200 sensor
The color sensor TCS3200 has eight pins, but we use seven of them:
- S0
- S1
- S2
- S3
- Out
- GND
- VCC

The VCC must be connected to the 3V3 pin of the NodeMCU, the GND to the GND pin of the NodeMCU. The power rail doesn't need to be specified in the code, but the other pins you have to, in order for the sensor to work properly. On the Arduino IDE, select the `File > Open` menu and open the `1_iobColorTest` file, located in the previously copied folder, as we explained in the section [4. Download the code!](https://github.com/Internet-of-Bananas/installation-guide#4-download-the-code), and find the part shown below, where we specify the pins of the color sensor:

```
// *** Setup parameters for the Color Sensor TCS3200 ***
// Create the variable to define the pins
const int s0 = D6;    // Set the pin S0 on the NodeMCU.
const int s1 = D7;    // Set the pin S1 on the NodeMCU.
const int s2 = D3;    // Set the pin S2 on the NodeMCU.
const int s3 = D4;    // Set the pin S3 on the NodeMCU.
const int out = D5;   // Set the pin Output on the NodeMCU.
```

You can connect the pins of the sensor to NodeMCU following what is already set in the code, or you can change it, but then you have to update the code with the correspondents pins that you connected. Remember that the **D** is upper case, or if you choose to write the GPIO number you only write the number, without the letters *GPIO*. 

Disconnect your board fom the USB cable and plug the jumper cables in the respective pins. Double check the connection, after you finish, plug the board back in the USB.

To test if the TCS3200 sensor was succesfuly connected to the NodeMCU, upload the code to your board, select the menu `Skecth > Upload`, or press `Ctrl + U` or `Command + U`. When the upload is concluded, select `Tools > Serial monitor`. In the window that opens, in the lower right corner, select speed `9600`. Point the color sensor close to a surface and check if the numbers change. 

#### 5.1.1. Color calibration
It is necessary to do a color calibration of your TCS3200 sensor, because they vary from sensor to sensor so we can't use standard values. To calibrate, you will need a black and white paper. Then, you point the sensor to the black paper and write downd the numbers for the red, green and blue reading. Next, you repeat the same proceadure with white papaer. These numbers means the minimum and maximum reading of intensity of red, green and blue. 

To do the calibration, open the file `2_iobColorFilter`, if it is the case, update the pin definition as explained in the section [5.1. Connecting the TCS3200 sensor
](https://github.com/Internet-of-Bananas/installation-guide#51-connecting-the-tcs3200-sensor). Open the serial monitor, `Tools > Serial monitor`. In the window that opens, in the lower right corner, check if the speed is set to `9600`, otherwise do it. 

Place the sensor near a white paper and take note of the lowest values, from red, green and blue. Repeat the proceadure with a black paper, and take note of the highest values, from red, green and blue. These numbers will be necessary in the IoB code, for a more accurate color measurement.

For a better understanding how the color sensor works, we suggest you to run the code 1_iobColorTest, 2_iobColorFilter, and 3_iobColorRGBhex and read its comments (the text after the `//` or inside `/*    */`).

### 5.2. Connectiong the DHT11 sensor
The temperature and humidity sensor has three pins, the VCC, GND and the signal, which can be represented on the board as `out`. As we mention before, the power rail doesn't need to be specified in the code, only the signal pin. On the Arduino IDE, select the `File > Open` menu and open the `4_iobDHT11Test` file, located in the previously copied folder, and find the part shown below, where we specify the pins of the color sensor:

```
// *** Setup parameters for the Sensor DHT11 ***
#include "DHT.h"          // Add the library DHT.
#define DHTPIN D1         // Set the pin connected to the DHT sensor.
```

You can connect the signal pin of the sensor to NodeMCU following what is already set in the code, or you can change it, but then you have to update the code with the correspondent pin that you connected. Remember that the **D** is upper case, or if you choose to write the GPIO number you only write the number, without the letters *GPIO*. 

Disconnect your board fom the USB cable and plug the jumper cables in the respective pins. Double check the connection, after you finish, plug the board back in the USB.

To test if the DHT11 sensor was succesfuly connected to the NodeMCU, upload the code to your board, select the menu `Skecth > Upload`, or press `Ctrl + U` or `Command + U`. When the upload is concluded, select `Tools > Serial monitor`. In the window that opens, in the lower right corner, check if the speed is set to `9600`, otherwise do it.  Place the sensor inside your hands and check if the temperature increases.

## 6. The IoB code
It is now time to acquire the IoB software and to update it with the required informtion. To download the code, follow the instructions of the seciton [4. Download the code!](https://github.com/Internet-of-Bananas/installation-guide#4-download-the-code)

On the Arduino IDE, select the `File > Open` menu and open the `6_iob.ino` file, located in the previously copied folder. 

Before sending the code to ESP8266, you need to make some changes:

- specify the pins of the sensors, as explained in [5. Connecting the hardware pins](https://github.com/Internet-of-Bananas/installation-guide#5-connecting-the-hardware-pins);
- the color calibration parameters; 
- name and password of the WiFi network, so that the device can connect to the internet;
- Adafruit IO server account login and key;

### 6.1. The WiFi credentials
To connect to the internet you need to specify the name and password of your WiFi network. To update the sketch with your name and password, locate the code below:

``` 
/************************* WiFi Access Point *********************************/

#define WLAN_SSID      "wifiName"     //  Name of the WiFi network.
#define WLAN_PASS      "wifiPassword" //  WiFi password.
```

Replace the `wifiName` with your WiFi name, and `wifiPassword` with your WiFi password, keeping the quotes. Write exactly how it is, with space and upper and lower case, if there are any.

### 6.2. Adafruit IO credentials
To access the Adafruit IO server you need to specify the user name and the key. If you don't have one, you can create it here [// https://accounts.adafruit.com/users/sign_up](https://accounts.adafruit.com/users/sign_up). To update the sketch, locate the code below:

``` 
/************************* Adafruit.io Setup *********************************/
// You need an account at the Adafruit IO to publish data.
// If you don't have it, you can create one at
// https://accounts.adafruit.com/users/sign_up

#define AIO_SERVER      "io.adafruit.com"       //  Address of the MQTT server from Adafruit IO, don't change.
#define AIO_SERVERPORT  1883                    //  Number of server port from Adafruit IO.
#define AIO_USERNAME    "username"  //  Adafruit IO user name.
#define AIO_KEY         "key"    //  Adafruit IO user key.
``` 

Replace `username` with the user name of your account at Adafruit IO, and `key` with your account's key.

### 6.3. Upload the code to the NodeMCU Esp8266
After completing the code updates, connect the NodeMCU board to the computer with the USB cable. In the Arduino IDE, select the menu `Skecth > Upload`, or press `Ctrl + U` or `Command + U`. 

To check if the code updates were successful, select `Tools > Serial monitor`. In the window that opens, in the lower right corner, select speed `9600`.

As the data is sent, the confirmation `Ok, sent!` is displayed for each feed in the Serial Monitor window. If the data is not being sent or there's an error, check if the name and password of the WiFi network and user name and key of your account at Adafruit IO are correct.

In addition, you can check the feeds on your [Adafruit IO](https://io.adafruit.com/) page to see if the data is arriving.

After confirming that the updates have been well succeeded, you can set the feed as public and create a dashboard on Adafruit IO.

## 7. Adafruit IO
### 7.1. Set the feeds as public
To display your data on the Internet of Bananas map we kindly ask you to set the privacy of your feeds as public and inform us your username, so we can use the API `https://io.adafruit.com/api/v2/<username>/feeds` to get the color, temperature and humidity data. 

To set the feed as public, go to [https://io.adafruit.com](https://io.adafruit.com), select the menu `Feeds` and the `view all` to view all the feeds. Click on the name of the feed **color**, to open the feed's page. On the right side of the screen, select `Privacy` and the change de visibilty to `Public`. Repeat the same proceadure with the other two feeds.

### 7.2 Create a dashboard
To visualize the data from your IoB you can create a dashboard at your Adafruit IO account. To do so, go to [https://io.adafruit.com](https://io.adafruit.com), select the menu `Dashboards`, and the `view all` to view all the dashboards. Click on the button `+ New Dashboard`, and give it a name and click on `Create`. Then click on the name of the dashboard you just created to open its page. You can choose your own layout of dashboard. As example, we suggest to have a stream of data, a color indicator and charts.

To create a stream of data block, click on the gear icon, in the right top corner of the screen, and choose `Create New Block`, in the options listed, choose `Stream` and select the color, temperature and humidity feeds, and go to the `Next step`. In the following window, give a title to the stream block and click `Create block`. 

To create a color indicator for the color feed, click again on the gear icon, and choose `Create New Block`, select `Color Picker` and then choose the color feed, and go to `Next step`. Give a title and create the block. 

To create the chart, click on the gear icon, and choose `Create New Block`, select `Line Chart` and then choose the temperature feed, and go to `Next step`. Give a title and create the block. Repeat the same proceadure to create a line chart for the humidity feed.

After you have created the blocks, you can adjust the layout by click on the gear icon and select `Edit Layout`. After you move and resize the blocks, click on `Save Layout`.

If you want to share your dashboard with friends, go again on the gear icon and select `Dashboard Privacy` to unlock it, and still in the *gear* options, click on `Share Links` to copy the URL of you dashboard.

## 8. Assembling the device on the banana
The NodeMCU board and the sensors should be attached to the banana using the rubber band or a tape. 

The color sensor should face the banana skin and should be placed close to the banana, with the leds from the sensor touching the banana. To avoid light interferences, we recommend to put a opaque and dark tape around the sensor, creating a "wall", so there's no other source of light reaching the sensor.

Even though the DHT11 sensor measeasures the temperatura and humidity of the air, we suggest to placed it as close as possible to the banana. 

As we previously warned about, do not place NodeMCU and the sensor on a metal or conductive surface, you can damage the microcontroller and sensors. Do not place the microcontrollers and the sensors in a wet environment, water can damage them and you may risk injury.

- - - 

If you have any question, please contact us at internetofbananas@gmail.com.
