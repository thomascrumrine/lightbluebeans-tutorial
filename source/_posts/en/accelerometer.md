---
title: Accelerometer
---

The LightBlue Bean has a 3-axis accelerometer whose data you can access through simple API calls.

<!-- more -->

The easiest way to access accelerometer data is through the `AccelerationReading` struct returned by `Bean.getAcceleration()`. This struct contains acceleration values for each axis as well as the acceleration sensitivity. Here is an example of printing this data in the `loop()` function of a sketch:

```
void loop() {
    AccelerationReading acceleration = Bean.getAcceleration();
    Serial.print(acceleration.xAxis);
    Serial.print(acceleration.yAxis);
    Serial.print(acceleration.zAxis);
}
```

Mentioned above was that the `AccelerationReading` struct contains acceleration sensitivity information. This is the range of sensitivity that the accelerometer will read in g-force. You can set it through the `setAccelerometerRange()`, passing in an integer 2, 4, 8, or 16. When this is set the bean will not register acceleration past plus or minus the value that is set. You can also use motion events to trigger functions to call if there is a `LOW_G_EVENT` (< 8g) or a `HIGH_G_EVENT` (>= 8g). You would register these events in `setup()` and check for them in `loop()`:

```
void setup() {
    Bean.setAccelerationRange(16);
    Bean.enableMotionEvent(LOW_G_EVENT);
    Bean.enableMotionEvent(HIGH_G_EVENT);    
}
void loop() {
    if (Bean.checkMotionEvent(LOW_G_EVENT)) {
        Serial.print("HIGH_G_EVENT!");
    }
    else if (Bean.checkMotionEvent(HIGH_G_EVENT)) {
        Serial.print("LOW_G_EVENT!");
    }
}
```

Setting these parameters are especially helpful if in your project, you do not care what the exact acceleration of the device is, but just want to check for sudden movements, etc.
