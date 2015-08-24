# LED warning for low battery #

Based on the [excellent tutorial](http://diydrones.com/profiles/blogs/led-low-battery-warning-on) from u4eake on DIYDrones forum, here's a quick how-to.

## Hardware setup ##

Here's a schematic (provided by thnilsen on RCGroups) showing how to make the voltage divider:

![http://megapirate.googlecode.com/svn/wiki/Images/VotageDivider.png](http://megapirate.googlecode.com/svn/wiki/Images/VotageDivider.png)

It's also possible to use the [Flydusense V2](http://flyduino.net/Flydusense-V2-mit-LLC), which is ready for the voltage divider and the LED driver.

Now, you will need to make the LED driver.
Here's a schematic (provided by aBUGSnightmare on RCGroups), using a [ULN2803](http://www.sparkfun.com/products/312) (transistor. Each output can handle up to 500mA:

![http://megapirate.googlecode.com/svn/wiki/Images/2803led.jpg](http://megapirate.googlecode.com/svn/wiki/Images/2803led.jpg)

Here's a [nice guide](http://oomlout.com/8TRA/8TRA-Guide.pdf) about the ULN2803.

You can also used the built in LED driver of the Flydusense V2.

**Connections between Flyduino and voltage divider/LED driver**

Connect the output of your voltage divider to PIN A0 of the Flyduino.
Then, connect the PIN A1 to the IN of the ULN2803 corresponding to the OUT driving your lEDs.

If you're using the Flydusense V2, connect the PIN A1 to the CPU PIN, near the LED driver.

## Sotfware modifications ##

1. In ArduCopterMega.pde

Find the medium\_loop() function and in it find case 4.

There is a call to the read\_battery() function there.

Insert a call to handle\_relay\_lights() right after the read\_battery() call.

```
// This case controls the slow loop
 
//---------------------------------
 
case 4:
 
  loop_step = 5;
 
  medium_loopCounter = 0;
 

  delta_ms_medium_loop = millis() - medium_loopTimer;
 
  medium_loopTimer     = millis();
 

  if (g.battery_monitoring != 0)
 
{
 
  read_battery();                               
 
[COLOR: RED] handle_relay_lights();[/COLOR]
 
  }
```

2. APM\_config.h

Add the following code :

```
#ifndef BATTERY_EVENT
 # define BATTERY_EVENT ENABLED
#endif

#ifndef LOW_VOLTAGE
 # define LOW_VOLTAGE 9.9
#endif

#ifndef MID_VOLTAGE
 # define MID_VOLTAGE 10.6
#endif

#ifndef VOLT_DIV_RATIO
 # define VOLT_DIV_RATIO 3.33
#endif

#ifndef INPUT_VOLTAGE
 # define INPUT_VOLTAGE 5.26
#endif
 
 
 
// global variables for motor leds and battery monitoring through onboard relay 
 
int rl_state   = 0 ;     // this global variable is used to save the on/off state of the relay
 
int rl_counter = 0 ;    // this global variable is used for the flash rate timer
```

3. Add the [RunningLights.pde](http://megapirate.googlecode.com/files/RunningLights.pde) to your MegaPirate folder.

4. Copy and paste to a safe place the original events.pde that you will found in your MegaPirate folder, and paste this new [events.pde](http://megapirate.googlecode.com/files/events.pde)

5. You're done! Adjust the MID and LOW voltage values to your needs.