Introduction:

The first thing you do when you buy a Raspberry Pi is to access the terminal using SSH, with PuTTY for instance. This is a great when you have network connectivity, but what if you want to use your Pi without network access? You could connect using the micro USB port as a serial port, but then you can’t use that port for anything else. In this article I’ll walk you through a simple project to access a Raspberry Pi terminal over Bluetooth using your Apple device. You can create files, install packages, run scripts or do any of the things you normally do over SSH.

You can use the concepts in this tutorial to add Bluetooth connectivity for lots of different projects. Note that the Raspberry Pi 3 has built in Bluetooth functionality.

Hardware:

I’ll be using an HC-06 module as a Bluetooth transceiver. You can find it for less than 8E shipped on Amazon. This little module will act as a Bluetooth slave device that sends and receives serial data. There are only 4 pins to hook up, your standard ground and Vcc (3.3V) and the Tx/Rx Serial pins. The HC-06 Tx pin goes the Rpi Rx pin and the Rx pin on the Rpi Tx pin. 

  **PINS Identification:**

**_KEY: According to the data sheet, I need to pull­up this pin while power­on­reset of the module to enforce AT mode. We don't need it for this project!_**

**_VCC: Indicated in the range of 3.6V­6V. The module worked for me both with 3.3V and 5V._**

**_GND: Ground_**

**_TXD: Serial output of the module, to be connected to RX of the microcontroller. Note that this signal is using 3.3V logic level_**

**_RXD: Serial input of the module, to be connected to the TX of the microcontroller. Note that this signal is using 3.3V logic levels._**

Software:

The hardware was pretty simple,  now let’s tackle software. I’ll go over the main parts and then post the entire script at the end.

We are going to be controlling the serial port using a Python script. If you haven’t already, install the Python Serial library just to install it run  
``` 
sudo apt-get install python-serial 

``` 

Our Python script will have 2 main jobs:

1) Read any serial data coming in from the Bluetooth module and send it to a terminal process.
2) Send any data coming out of the terminal to the Bluetooth module.

Usage: 

Now that you have your hardware hooked up and your Python script is running on your Pi, you will need to connect to it using a Bluetooth terminal app. I used BlueTerm for IOS. 
Once you open the app search for the HC-06 and connect to it. If there is an option to add line termination characters (usually either “\n ” or “\r\n”) you will get more consistent performance. You can now type any commands you want in your app and see the result. You can even create and edit files. Interactive terminal programs like Vim likely won’t work however. One final step you might want to do to actually use this in the field is to start your Python script when the Pi boots up. To do this just add a command to start the script in the ``` /etc/rc.local ``` file. For example, adding the line:``` python ~/BluetoothTerm.py ``` and your script will start running when your Pi boots up.
