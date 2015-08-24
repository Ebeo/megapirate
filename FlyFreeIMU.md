# Flyduino and FreeIMU #

Here's the connection diagram of the Flyduino when used with MegaPirate code. You will find here all the pinout description and where to connect what.

For FreeIMU 0.3.5 BMP and FreeIMU 0.3.5 MS, connections are the same.

![http://megapirate.googlecode.com/svn/wiki/Images/final_flyduino_pinout_13-06-11.jpg](http://megapirate.googlecode.com/svn/wiki/Images/final_flyduino_pinout_13-06-11.jpg)

**Note**: _This diagram is for the Flyduino V1.1 (ATMega1280), the Serial Ports have changed on Flyudino V2.1 (ATMega2560) like this:_
| **V1.1** | **V2.1** |
|:---------|:---------|
|Serial1   |Serial2   |
|Serial2   |Serial3   |
|Serial3   |Serial1   |


  1. Connect your freeIMU to your Flydusense (and then to the Flyduino), or directly to the Flyduino, like this:

| **Flyduino** | **FreeIMU** |
|:-------------|:------------|
|GND           | GND         |
|SDA           |SDA          |
|SCL           |SCL          |
|5V            |VIN          |

**Note**: _This picture shows the FFIMU, which is now ended, but connections are the same._
![http://megapirate.googlecode.com/svn/wiki/Images/FFIMU%20connection.jpg](http://megapirate.googlecode.com/svn/wiki/Images/FFIMU%20connection.jpg)