# User Guide
![](http://www.kinoma.com/create/user-guide/img/cover.png)

***
## Important Information about Kinoma Create

The most important thing to know is that Kinoma Create is primarily a 3.3-volt system. If you’re new to 3.3v systems or to hardware in general, this means that (unlike with an Arduino or some other boards), you can only send up to 3.3 volts into input pins on the device without doing damage to the pin. The exception to this is the front pins, which can be set to either 3.3v or 5v mode. When in 5v mode, it is safe to input up to 5v into those pins.

Download [Kinoma Studio](/develop/studio/). You can try out your own applications in our desktop simulator, build apps for iOS and Android, or run any of our samples (included with Kinoma Studio) on your Kinoma Create.

Our team is ready to support you as you work with your Kinoma Create. The best way to get answers to your questions is to post on the [Kinoma Forum](http://forum.kinoma.com/).

We want you all to successfully bring your product concepts to life, and we’ll be here to help you along the way. Enjoy your Kinoma Create!

***
## Kinoma Create Front
![](http://www.kinoma.com/create/user-guide/img/create-front.png)

***
## Kinoma Create Back
![](http://www.kinoma.com/create/user-guide/img/create-back.png)

***
## Kinoma Create First Run

1.Remove back cover and install battery.

![](http://www.kinoma.com/create/user-guide/img/firstRun01.png)

2.Begin charging device with supplied charger. (Note: Kinoma Create has specific requirements for the charging block and cable. Using charging systems other than the one supplied with Kinoma Create may not charge the battery).

![](http://www.kinoma.com/create/user-guide/img/firstRun02.png)

3.Press the power button on the back of Kinoma Create. Kinoma Create typically takes 35 seconds to boot up.

![](http://www.kinoma.com/create/user-guide/img/firstRun03.png)

4.Use the Wi-Fi app to add your Kinoma Create to your network.

![](http://www.kinoma.com/create/user-guide/img/firstRun04.png)

5.Use the Settings app to set the time zone.

![](http://www.kinoma.com/create/user-guide/img/firstRun05.png)

***
## Powering On/Off & Sleep
Press Power button to start

If powered up, and a short press, device shows menu to Shutdown, enter Sleep mode or return to Shell.

If powered up, and in Sleep mode, a short press wakes up

Reset device button. Use paper clip or similar to reach reset button and perform a hard reset

Kinoma Create typically takes 35 seconds to boot-up

![](http://www.kinoma.com/create/user-guide/img/shutdown-sleep.png)

***
## Charging Internal Battery
Kinoma Create uses a 3.7V, 2600mAh Li-Ion battery. The battery style is 18650.

Kinoma Create has specific requirements for the charging block and cable. Using charging systems other than the one supplied with Kinoma Create may not charge the battery. The battery power graph is calibrated to the included Tenergy battery.

![](http://www.kinoma.com/create/user-guide/img/battery.png)

***
## Settings

The Settings app on Kinoma Create allows for setting device attributes, updating firmware and software and viewing device information.

If updates are available an UPDATE notification will appear. Tap to update.

![](http://www.kinoma.com/create/user-guide/img/settings.png)

**Name**: Set Device name if desired. This is the name that this Kinoma Create will use for network connections, including debugging in the Kinoma Studio IDE.

**Time zone**: It is important to set the local timezone for apps that use time and date based APIs. The optional daylight savings check box needs to be manually updated to match local conditions.

**Startup App**: For testing, Kinoma Create can be set to launch into a specific app. The default is the Kinoma Create Home which contains utility and configuration apps.

![](http://www.kinoma.com/create/user-guide/img/settings02.png)

**Quit Gesture**: Specifies the gesture used to force-quit the currently-running application and return to Home.

**Delete Apps**: Allows removal of apps that have been installed on Kinoma Create. Note: Built-in apps, such as Wi-Fi and Settings cannot be deleted.

**Backlight**: Increase battery life by adjusting the brightness of the screen's backlighting.

![](http://www.kinoma.com/create/user-guide/img/settings03.png)

**Debugging**: Kinoma Create can be set to allow debugging over Wi-Fi and require a password to debug.

**Kinoma Software**: Shows the current version number and Updates if available.

**Firmware (OS)**: Shows the current firmware version number and Updates if available.

**Setup SD Card**: Format and partition an SD to become a bootable file system.

**Clear Caches**: Allows clearing of Kinoma Studio App, Cookie and HTTP caches.

**MAC Address**: Shows the MAC address of this Kinoma Create. The MAC address is the unique id of the device.

![](http://www.kinoma.com/create/user-guide/img/settings04.png)

***
## On-Board Apps
**Battery**: Shows current charge status on the tile and charging and usage history in the app view. The most current value is at the left.

![](http://www.kinoma.com/create/user-guide/img/battery-charging.png) = Charging

![](http://www.kinoma.com/create/user-guide/img/battery-charged.png) = Fully Charged

![](http://www.kinoma.com/create/user-guide/img/battery-depleting.png) = Depleting

![](http://www.kinoma.com/create/user-guide/img/battery-depleted.png) = Fully Depleted

![](http://www.kinoma.com/create/user-guide/img/battery-app.png)

**File Sharing**: Allows mounting your Kinoma Create on your PC (Windows and Mac). File Sharing implements the WebDAV protocol.

![](http://www.kinoma.com/create/user-guide/img/fileSharing-app.png)

**Front Pins**: The 16 front facing pins can be targeted by the Front Pins app to support Digital, Analog In, and I2C connections. Power can be supplied at 3.3 or 5 volts. Input pins will handle up to 3.3v.

![](http://www.kinoma.com/create/user-guide/img/frontPins-app.png)

**Files**: Files app provides simple view and management functionality for files on Kinoma Create. It can display PNG and JPEG images, MPEG-4 files, and the first 8KB of text based files. Individual files can be deleted.

![](http://www.kinoma.com/create/user-guide/img/files-app.png)

**Logs**: Presents information about the operation of Kinoma Create. Kinoma Create logs Pin Settings, Settings, system events and Wi-Fi configurations.

![](http://www.kinoma.com/create/user-guide/img/logs-app.png)

**Samples**: Shows current content from the Kinoma GitHub Samples repository. Select samples to download and view on the device. Select View Source to view the JavaScript source and other project assets.

![](http://www.kinoma.com/create/user-guide/img/samples-app.png)

**Net Scanner**: View the SSDP devices on the current network. Returns name, urn, uuid, and url. Services and xml device description are shown if available.

![](http://www.kinoma.com/create/user-guide/img/netScanner-app.png)

***
## Setting up a bootable SD card

Install an SD card and you can set it up as a bootable volume. As part of the setup process, the latest Kinoma Create system image will download. Once you have created a bootable SD card you can boot from the SD card and update the firmware on your Kinoma Create. <span style="color:red">Note: formatting an SD card as a bootable volume completely erases the contents of the SD card being prepared</span>.

There is a toggle to start from a bootable SD card if present.

![](http://www.kinoma.com/create/user-guide/img/boot-sd-card01.png)

***
## Connecting to a PC

You can connect your Kinoma Create to your PC via either Micro USB or Mini USB to power it.

If you connect via the Mini USB Port, you can access the serial console. [FTDI Drivers](http://www.ftdichip.com/FTDrivers.htm) are required for serial connections over USB.

![](http://www.kinoma.com/create/user-guide/img/connect-pc.png)

***
## Using Hardware Pins (Front)

**Front Pins**: The 16 front facing pins can be targeted by the Front Pins app to support Digital, Analog In, and I2C connections. Power can be supplied at 3.3 or 5 volts. Input pins will handle up to 3.3v. The front pins are designed to conveniently fit readily-available sensors mounted on breakout boards.

**Open Sensor Cover**
While holding onto the sides of Kinoma Create, press up on the sensor cover with your thumbs.

![](http://www.kinoma.com/create/user-guide/img/front-pins.png)

***
## Using Hardware Pins (Rear)

![](http://www.kinoma.com/create/user-guide/img/rear-pins.png)

**Rear Pins**: The 50 pins accessible behind the rear access panel have dedicated functionality to support Digital, Analog Input, PWM, I2C, and Serial connections. Power can be supplied at 3.3 or 5 volts. Input pins will handle up to 3.3v.

For more information on coding with the hardware pins visit kinoma.com/create

![](http://www.kinoma.com/create/user-guide/img/back-view-rear-pins.png)

***
## Kinoma Create Stand

Kinoma create includes an adjustable stand. Use the dowels in the rear in the stand holes to set the stand to the angle desired.

![](http://www.kinoma.com/create/user-guide/img/stand.png)

***
## Specifications

![](http://www.kinoma.com/create/user-guide/img/Kinoma-Create-exploded-view.jpg)

* QVGA capacitive touch screen
* 800 MHz Marvell ARM SoC
* Wi-Fi (802.11b/g)
* 128 MB RAM, 16 MB SPI Flash
* microSD slot
* Speaker and microphone
* Wired (USB) and portable power options — includes 2600mAh 3.7V Lithium Ion (Li-ion) 18650 rechargeable battery
* No breadboard required for many breakout-board-based sensors
* Custom, lightweight Linux distribution to support the Kinoma platform
* Digital input/output (20-36, configurable), analog input (0-16, configurable), I2C (1 physical bus and two soft buses), UART Serial (1 physical bus), PWM (3)

*Recommended operating temperature: -5°c to +50°c (23°f to 122°f)*

***
## FCC Statement

This equipment has been tested and found to comply with the limits for a Class B digital device, pursuant to part 15 of the FCC Rules. These limits are designed to provide reasonable protection against harmful interference in a residential installation. This equipment generates, uses and can radiate radio frequency energy and, if not installed and used in accordance with the instructions, may cause harmful interference to radio communications. However, there is no guarantee that interference will not occur in a particular installation. If this equipment does cause harmful interference to radio or television reception, which can be determined by turning the equipment off and on, the user is encouraged to try to correct the interference by one or more of the following measures:

* Reorient or relocate the receiving antenna.
* Increase the separation between the equipment and receiver.
* Connect the equipment into an outlet on a circuit different from that to which the receiver is connected.
* Consult the dealer or an experienced radio/TV technician for help.

## FCC Radiation Exposure Statement

This equipment complies with FCC RF radiation exposure limits set forth for an uncontrolled environment. This transmitter must not be co-located or operating in conjunction with any other antenna or transmitter. This equipment should be installed and operated with a minimum distance of 20 centimeters between the radiator and your body.

This device complies with Part 15 of the FCC Rules. Operation is subject to the following two conditions: (1) this device may not cause harmful interference, and (2) this device must accept any interference received, including interference that may cause undesired operation.

**Caution!** Any changes or modifications not expressly approved by the party responsible for compliance could void the user's authority to operate the equipment.

## Canada Statement

This device complies with Industry Canada licence-exempt RSS standard(s). Operation is subject to the following two conditions: (1) this device may not cause interference, and (2) this device must accept any interference, including interference that may cause undesired operation of the device.

Le présent appareil est conforme aux CNR d'Industrie Canada applicables aux appareils radio exempts de licence. L'exploitation est autorisée aux deux conditions suivantes : (1) l'appareil ne doit pas produire de brouillage, et (2) l'utilisateur de l'appareil doit accepter tout brouillage radioélectrique subi, même si le brouillage est susceptible d'en compromettre le fonctionnement.

The device meets the exemption from the routine evaluation limits in section 2.5 of RSS 102 and compliance with RSS-102 RF exposure, users can obtain Canadian information on RF exposure and compliance.

Le dispositif rencontre l'exemption des limites courantes d'évaluation dans la section 2.5 de RSS 102 et la conformité à l'exposition de RSS-102 rf, utilisateurs peut obtenir l'information canadienne sur l'exposition et la conformité de rf.

This transmitter must not be co-located or operating in conjunction with any other antenna or transmitter. This equipment should be installed and operated with a minimum distance of 20 centimeters between the radiator and your body.

Cet émetteur ne doit pas être Co-placé ou ne fonctionnant en même temps qu'aucune autre antenne ou émetteur. Cet équipement devrait être installé et actionné avec une distance minimum de 20 centimètres entre le radiateur et votre corps.