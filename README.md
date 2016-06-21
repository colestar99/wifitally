# Wifitally
An ESP8266 based tally light for use with Blackmagic ATEM.

## Goals
This project aims to be an open source solution to otherwise cost intensive commercial projects. We chose to use Wifi, because this enshures that it is legal and license free to operate in most countries. The ESP is a perfect fit for this project, because its cheap to get and has almost everything we need onboard.

## Requirements
To get the Wifitally to work you will also need to have some sort of Arduino Ethernet or an Arduino with an Ethernet Shield. Because the ATEM Controller only supports a limited number of concurrent connections and it needs a fast and stable connection we decided to use an Arduino with an Ethernet Shield as a bridge between the ATEM and the Wifitally.

**Minimal requirements:**

* Wifitally
* A single cell Lithium Polymer Battery
* Arduino + Ethernet Shield
* ATEM
* Wifi Access Point with at least two Ethernet ports for the Atem and the Arduino
* Some sort of FTDI or USB to Serial converter (we will add a Serial to USB bridge in the next Revision)

**Optional requirements**
* SSD1306 display 128 x 64 in Size, the i2c version (for details see the [Display](#Display) section)

## IDE Preparations

* Download Arduino IDE if you do not already have it on your computer (www.arduino.cc)
* Install the Skaarhoj Libraries (http://skaarhoj.com/wiki/index.php/ATEM\_Arduino\_Library)
* Patch your Ethernet library. https://github.com/aallan/Arduino/commit/3811729f82ef05f3ae43341022e7b65a92d333a2

## Usage
Get the PCB design for the Tally, which is located in hardware/ and get it manufactured by your favourite PCB Manufacturer or do it yourself if you are able to etch two sided PCBs.
Order the neccessary parts, we provide a shopping cart for Farnell in hardware/shoppingcart and get an ESP8266-07. Solder everything onto the PCB, if your finished with that, the hard part is over.

Turn on your Wifitally and put it in to flash mode. You can tell if it's in flash mode, if the green LED is on. If it's not, flip the programming switch and press reset. Now the green LED should light up.
Connect it to your PC using the USB to Serial converter. Flash the receiver part of the software onto it, you can find it in software/receiver.

To initialize the EEPROM (the part where the data is stored) make shure your tally is turned off, then press the "Mode/Setup" button, while powering on the Tally until the LEDs flash white once. Now you should see a wifi ssid named "Wifitally". You can connect to it using the password "wifitally2342".

The remaining steps are easy:

1. Assemble the Arduino and the Ethernet Shield and flash the Transmitter part in software/transmitter onto the Arduino. You may want to have a look at the IP setup part of the transmitter.
1. Connect the Arduino and the ATEM to the Access Point
1. Connect to your Wifitally using the SSID "Wifitally" and the Password "wifitally2342" and browse to the IP of the tally, it may be 192.168.4.1, if this won't work, have a look at your ip setup and checkout what ip your gateway has. This will be the IP address of your Tally.
1. Enter the SSID and the Passwort of your Access Point into "WLAN SSID" and "WLAN Password" and uncheck "Standalone". Now click save.
1. Your Wifitally will reboot now, shortly after, it should be connected to your Access Point. You will see when it is connected successfully if the blue leds stop blinking.
If you misconfigured the Settings, you can again initialize the tally by switching it off and pressing the Mode switch while turning it back on, until it flashes white once.

The Tally runs on a single cell LiPo battery, you can  (once it's fixed) charge it using the USB Port onboard. Currently the only purpose of the USB Port is to charge the battery.

### <a name="Display"></a> Display
First of all: Unfortunately if you want to have a Display, you will need an Arduino wich has bigger memory than an Arduino UNO.
If you chose to invest in the SSD1306 Display, you can use it to display stuff like "Low Battery on Tally 2" or "Tally 2 shut down because it's battery is drained". Make shure you have the i2c version and hook up the 4 pins. Should be labeled
* VCC, which goes to +5V or VCC
* GND, which goes to Ground or GND
* SDA, which goes to the SDA Pin of your Arduino
* SCL, which goes to the SCL Pin of your Arduino

Uncomment the display lines in the transmitter code. You will find
```c
// for display use, uncomment from here
....
// to here
```
Just remove the slashes in the lines between those two lines. There is more than one part, that needs uncommenting

### Setting the battery threshold
If you decided not to use a single cell LiPo or chose different values for R17/R18, you need to set the threshold to match your battery.
Of cause you can also remove R17 and R18 if you are using a powersupply

If you got your voltage divider right, do one of the following:

* Connect your almost empty battery to the Wifitally and open up the configuration page. Hit "Update battery threshold". The Wifitally will use the current voltage of the battery as threshold for the low battery warning.
* Measure the current voltage of your battery while the tally is powered on, go to the configuration and enter the measured voltage in to the corresponding input (in millivolts please!) and then enter your desired warning and cutoff voltage.

## Future Releases
Current status of this Project is still beta. There is some work to be done in the future releases, including:

**Software**

*

**Hardware**

* Adding a CP2102 to programm the ESP directly with the onboard USB Port
* Adding a LED breakout to be able to add external leds and / or fets for bigger leds
* fix the charger circuit
## Thanks to
Kasper Skaarhoj for the great work on the Arduino library for the ATEM Controller

This Project was born in the Chaostreff Dortmund www.ctdo.de
