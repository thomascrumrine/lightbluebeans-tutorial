---
title: Lunchbox Monitor
---

![apple](/images/apple_smaller.jpg)
*Good thing apples don't need refrigeration!*

The FDA recommends not allowing perishable foods to stay above 40 degrees Fahrenheit for more than 2 hours. [This project](https://www.hackster.io/cwchesney/lunch-monitor-2f9b5b?ref=user&ref_id=104024&offset=0) shows how we can use a light blue bean to monitor the temperature of our lunch box and display a red light when there has been at least two hours of 40+ degrees. However, does anyone really want to be opening their lunchbox all day to check on it? Let’s take this a step further and use the LightBlue Bean's iBeacon to display the status of our lunch on our phone.

*We will not go into much detail of the iOS side of things, as that is not what this tutorial is about*

The LightBlue Bean can act as an iBeacon that passively broadcasts data to anything in its range that is listening for it via Bluetooth LE. This can be used to notify users when they walk near a landmark in a park, or to update a tv or monitor when a user walks near it in a store. This is similar to geolocation, but iBeacons have the potential to work in places that geolocation will not, such as places with poor GPS signal. It emphasizes proximity to objects rather than physical location on earth.

In the Lunchbox Monitor project referenced above, we periodically check the Bean's temperature. When the temperature is above 40 degrees Fahrenheit, we increment a timer. Once the timer reaches 120 minutes, the Arduino starts blinking red.

In our revised app, we will start broadcasting as an iBeacon using a certain identifier. When the food is in a "bad" state, we change the identifier of our iBeacon. **Here is the code:**

```cpp
#define TTINTERVAL 60000 // Check every minute
#define TICK 1 // increment our minutes by 1
#define TTLIMIT 120
void setup() {
  Bean.enableConfigSave(false); // We do not need to save to NVRAM
  Bean.setBeaconParameters(0xAAAA, 0xBEEF, 0xCAFE); // Initial (okay) state
  Bean.setBeaconEnable(true); // Start broadcasting
}
void loop() {
  static uint8_t minAbove5C = 0;    //minutes above 40F or 5C
  int8_t lTemp = Bean.getTemperature();
  if ( lTemp >= 5 ) { // getTemperature() returns in Celcius
    minAbove5C += TICK;
  }
  // put current temp and minAbove5C in scratch here
  Bean.setScratchData (1, (uint8_t *) &lTemp, 1);
  Bean.setScratchData (2, &minAbove5C, 1);
  if ( minAbove5C >= TTLIMIT) {
    Bean.setBeaconParameters(0xDEAD, 0xBEEF, 0xCAFE);
    Bean.sleep(0xFFFFFFFF) // Sleep indefinitely
  }
  else {
    Bean.sleep(TTINTERVAL);
  }
}
```

### So What's Going on? ###
Let's go through by line number:
* 1: Define `TTINTERVAL` to 60000 in milliseconds (= 1 minute)
* 2: Define `TICK` to 1, this is our own increment, so we can use minutes
* 3: Define `TTLIMIT` to 120, we will increment by `TICK` and this is our limit (2 hours)
* 5: We do not need persistent state across the Bean's power cycles, so don't use NVRAM (non-volatile) which has a finite number of writes. If this does not make sense, do not worry about it, it is not important at this stage.
* 6: Initialize our emitting UUID
* 7: Turn iBeacon on
* 10: Initialize our minutes greater than 40 degrees to 0.
* 11: Get the temperature in Celcius.
* 12: If the temperature is greater thatn 5 degrees Celcius or 40 degrees Fahrenheit, increment our minutes counter.
* 16: (and 17.) sets the scratch data, which would send a notify() event to any devices listening to this via Bluetooth LE.
* 18: Check if we are past 120 minutes.
* 19: If so, change our emiting UUID (bad-state)
* 20: Sleep indefinitely, we are done with Arduino chip, iBeacon just uses the Bluetooth chip.
* 23: If we have not reached the limit yet, sleep for a minute.


For a mobile app to interface with this, we would simply listen for both UUIDs. When neither are there, we know we are not in range or the Bean is not emitting. If the good UUID is emitting, we know our food is still okay. If the bad UUID is emitting, we know our food is bad.
