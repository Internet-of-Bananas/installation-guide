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

**Attention**: The IoB devices contains fragile parts, be careful when handling them. Avoid place them in a metal surface, you can short-circuit the microcontroller and sensors, and damage them.

## 1. Installation of Arduino IDE
To update the code it is necessary to download the *Arduino IDE* - the software that allows writing and sending code to the Arduino microcontroller, NodeMCU Esp8266, among others.

Access https://www.arduino.cc/en/Main/Software and download Arduino according to your operating system. When the download is complete, install Arduino. During the installation, confirmations may appear to install the Arduino components, if these warnings appear, confirm the installation.

Along with the Arduino software, USB drivers are installed, however depending on the operating system, it may be that the USB driver for your NodeMCU Esp8266 has not been installed.

To check, connect the USB cable to your NodeMCU board, and the other end of the cable connect to the computer's USB port. Again, depending on the operating system, it may automatically recognize and install the driver, if it's your case, wait until this process finishes. 

Then open the Arduino IDE, select the `Tools> Port` menu and observe the *Com* ports listed. Press `Esc` to exit the selected menu. Disconnect the board's cable from the USB port. Again, select the `Tools> Port` menu, and note which *Com* port is no longer present, the one that dissapeared is the *Com* port of your board. If it continues with the same *Com* ports listed, or if there's none, the NodeMCU Esp8266 driver has not been installed. In this case, follow the procedure in item *1.1. CH340 driver installation*.

### 1.1. CH340 driver installation
If the NodeMCU Esp8266 driver was not installed, you should try to install the CH340 Driver. For that, exit Arduino IDE and download the file according to your operating system:
- [Linux](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#linux)
- [Windows](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#windows-710)
- [Mac OSX](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all#mac-osx)

Follow the instruction and install the driver. If an alert window appears, confirm the installation of the file. After installing the driver, close the window.

To check if the installation was well succeded, open the Arduino IDE, select the `Tools> Port` menu and observe the *Com* ports listed. Press `Esc` to exit the selected menu. Disconnect the board's cable from the USB port. Again, select the `Tools> Port` menu, and note which *Com* port is no longer present, the one that dissapeared is the *Com* port of your board.

### 1.2. Add the Esp8266 board 

### 1.3. Verify the installation

## 2. Add libraries to the Arduino IDE

## 3. Update the code


