---
title: Power Management
---

PunchThrough Design boasts excellent battery life for the LightBlue Bean. In order to really maximize this, we need to understand how the Bean manages power.

<!-- more -->

When programming the Bean, you may be tempted to use the standard Arduino `delay()` function, however during the delay the Bean is still running at full power yet doing nothing. The LightBlue Bean has a function called `Bean.sleep()` that takes in an integer that denotes how long to sleep in milliseconds. While asleep, the Bean similarly does not perform any operations, but instead uses hardly any power. It actually puts the microcontroller to sleep and the Bluetooth LE chip wakes it up. Pass in `0xFFFFFFFF` to sleep indefinitely.

You can also use interrupts to wake up the Bean:

```
#include <PinChangeInt.h>
void setup() {
    pinMode(0, INPUT);
    attachPinChangeInterrupt(0, pinChanged, CHANGE);
}
void loop() {
    Bean.sleep(0xFFFFFFFF);
}
void pinChanged() {
    Bean.setLed(255, 0, 0);
    Bean.sleep(100);
    Bean.setLed(0, 0, 0);
}
```

In the following above, we create a `pinChanged()` callback function that blinks the LED. In setup we set the `attachPinChangeInterrupt` which uses the `PinChangeInt.h` Arduino library to call the `pinChanged()` callback function when pin 0 changes. Otherwise the Bean sleeps indefinitely, which we set in the main `loop()` function.
