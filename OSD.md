# On Screen Display #

Here are informations on how to use an OSD with Flyduino.
There is a lot of different OSD, we will focus here on OSD that are already implemented in the code.

# Remzibi OSD #

Remzibi OSD, also known as "A poor man's OSD", is a nice and not so expensive hardware. Google it to find a dealer near you.

Here's the [RCGroups thread](http://www.rcgroups.com/forums/showthread.php?t=921467) relating to it.

![http://megapirate.googlecode.com/svn/wiki/Images/OSDREMZIBI%2001.jpg](http://megapirate.googlecode.com/svn/wiki/Images/OSDREMZIBI%2001.jpg)

## Software modifications ##

Based on the work of Syberian, Sir Alex and Fr3d-nexus on RCGroups, here are the modifications you will need to do:

1. APM\_config.h
  * Add this lines:
```
//#define OSD_PROTOCOL OSD_PROTOCOL_NONE                           //fr3d
#define OSD_PROTOCOL OSD_PROTOCOL_REMZIBI
    /*
        OSD_PROTOCOL_NONE
        OSD_PROTOCOL_SYBERIAN
        OSD_PROTOCOL_REMZIBI
    */                                    //fr3d
```
  * Change GPS protocol to NMEA:
```
//#define GPS_PROTOCOL GPS_PROTOCOL_NONE                                      //fr3d
#define GPS_PROTOCOL GPS_PROTOCOL_NMEA
```

2. config.h
  * Add this lines:
```
////////////////////////////////////////////////////////////////////////////// //fr3d
// OSD_PROTOCOL
//
#ifndef OSD_PROTOCOL
# define OSD_PROTOCOL         OSD_PROTOCOL_NONE
#endif                                                                            //fr3d
```

3. Defines.h
  * Add this lines:
```
#define GPS_PROTOCOL_UBLOX_I2C 8        //fr3d
#define GPS_PROTOCOL_BLACKVORTEX 9        //fr3d
```

4. MegaPirate.pde
  * Add this lines:
```
////////////////////////////////////////////////////////////////////////////////
// Serial ports
////////////////////////////////////////////////////////////////////////////////
//
// Note that FastSerial port buffers are allocated at ::begin time,
// so there is not much of a penalty to defining ports that we don't
// use.
//
FastSerialPort0(Serial);        // FTDI/console
#if OSD_PROTOCOL != OSD_PROTOCOL_NONE		//fr3d
	FastSerialPort1(Serial1);        // OSD port
#endif										//fr3d
FastSerialPort2(Serial2);       // GPS port
FastSerialPort3(Serial3);       // Telemetry port

////////////////////////////////////////////////////////////////////////////////
```
  * Just after those lines (~line 595)
```
	// write out the servo PWM values
	// ------------------------------
	set_servos_4();

	//if(motor_armed)
	//Log_Write_Attitude();

// agmatthews - USERHOOKS
#ifdef USERHOOK_FASTLOOP
   USERHOOK_FASTLOOP
#endif
```

  * Add this lines:
```
static void medium_loop()
{
	// OSD heartbeat at 50Hz //fr3d
	#if OSD_PROTOCOL != OSD_PROTOCOL_NONE
 		osd_heartbeat_50Hz();
	#endif					 //fr3d
	
	// LED Sequencer update at 50Hz
	#if LED_SEQUENCER == ENABLED
		sq_led_heartbeat(); 
	#endif					//fr3d
```

5. Motor.pde
  * Add this lines:
```
#if OSD_PROTOCOL == OSD_PROTOCOL_REMZIBI                                //fr3d
                    osd_init_home();
#endif                                    //fr3d
```

6. System.pde
  * Add this lines:
```
#if OSD_PROTOCOL != OSD_PROTOCOL_NONE         //fr3d
        osd_init();
#endif
```

7. Finally, add the following files to your MegaPirate folder:
  * [OSD\_Remzibi.pde](http://megapirate.googlecode.com/files/OSD_Remzibi.pde)
  * [OSD\_Syberian.pde](http://megapirate.googlecode.com/files/OSD_Syberian.pde)

## Hardware connections ##

  * Connect the GPS unit comming with the OSD to Serial2.

> If you're using Flyduino V2.1, you will need to hack a [GPS cable](http://flyduino.net/GPS-cable-20cm) and solder a four raw pins to it.

> Don't forget to swap the wires to connect Tx(GPS) to Rx(Flyduino) and Rx(GPS) to Tx(Flyduino).

  * Connect only one wire from Serial Tx1 (Flyduino) to Rx (OSD). Don't connect other wires (5V, GND, Tx2)

  * In order to have the battery level displayed in the OSD, you will have to use a voltage divider and connect it to A0. See the [LED warning for low battery](LEDwarning.md) page to find more informations.

## OSD configuration ##

You will find [here](http://www.happykillmore.com/Software/RemzibiOSD/Setup/Setup.exe) the configuration program and the [How-to web site](http://remzibiosd.happykillmore.com/)

The OSD's version working with the actual code is the Ardum1.73 that you will find inside [this download](http://www.rcgroups.com/forums/showatt.php?attachmentid=4317750&d=1317478636).

Follow the instructions on the How-to web site to load this version.