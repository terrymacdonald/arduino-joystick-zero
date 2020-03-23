# arduino-joystick-zero
Arduino library that allows various Arduino boards to appear as a Joystick or Gamepad. The following devices *should* be compatible, but only the Arduino Zero has been tested by me:
   - Arduino AVR Boards (Uno, Uno Wifi, Nano, Mega, Leonardo and more)
   - Arduino SAM Boards (Due)
   - Arduino SAMD Boards (Zero, MKR)

The code in this library is largely based on Matthew Heironimus code from https://github.com/MHeironimus/ArduinoJoystickLibrary, with the primary difference between the two being that this library is built using the native HID library, rather than the custom DynamicHID library Matthew developed and used. This change allows the JoystickZero library to work with many different Arduinos, and yet still benefit from the amazing dynamic HID profiles that Matthew created. 

The Joystick Library *should* work with all the Arduino versions supported by the AVR, SAM and SAMD cores. To see a list of those devices, Open the Arduino IDE, select the Tools menu, then Boards, then Boards Manager, and look at the list of boards under the AVR, SAM and SAMD cores.

# How to Use
## Install and prepare the Arduino IDE
1. You need to install the Arduino IDE if you haven't already. You can do that from https://www.arduino.cc/en/Main/Software.
2. Open the Arduino IDE.
3. Select the Tools menu, then Boards, then Boards Manager...
4. Depending on the Arduino you have, you need to install the Arduino Core for your board into the Arduino IDE:
   - Arduino AVR Boards (Uno, Uno Wifi, Nano, Mega, Leonardo and more)
   - Arduino SAM Boards (Due)
   - Arduino SAMD Boards (Zero, MKR)
   - To install an Arduino Core click on the booard core, then click the Install button. 
   - The board core will be downloaded, and will show up in the list of boards available under Tools > Board.

## Install JoystickZero into the IDE
1. Using your file manager, copy the JoystickZero folder into your IDE libraries directory. On Windows 10 this is typically at `C:\Program Files (x86)\Arduino\libraries`. On Linux machines, this is typically `$HOME/Arduino/libraries`.
2. Start (or restart) the Arduino IDE. 
3. Click on the Sketch menu item, then select Include Library, and check that you can see the JoystickZero library listed under the Contributed Libraries section of the menu.
4. The library is ready to be used!

## Connect your Arduino IDE to the Arduino board

1. Plug in your Arduino to the computer running the Arduino IDE. This will typically be using a USB cable, but may not be.
2. Click on Tools menu, then Port, then select the COM Port that your Arduino has used to connect to your computer. The COM Port needs to be correct, so that the Arduino IDE can talk to the Arduino board. If the Port is set incorrectly, the rest of the instructions cannot be performed.
3. Check the port is set correctly by showing the Board information. This can be done by clicking on Tools menu, then Get Board Info. You should see the Vendor ID, Product ID and Serial number of the Arduino board you're using. If you don't get that, then check the Arduino is plugged in, and that you have selected the correct port.

## Create a sketch that uses JoystickZero

Now you need to create an Arduino Sketch that uses the JoystickZero library. Here is a simple example taken from Matthew's github:

```
#include <JoystickZero.h>

// Create the Joystick
Joystick_ Joystick;

// Constant that maps the phyical pin to the joystick button.
const int pinToButtonMap = 9;

void setup() {
  // Initialize Button Pins
  pinMode(pinToButtonMap, INPUT_PULLUP);

  // Initialize Joystick Library
  Joystick.begin();
}

// Last state of the button
int lastButtonState = 0;

void loop() {

  // Read pin values
  int currentButtonState = !digitalRead(pinToButtonMap);
  if (currentButtonState != lastButtonState)
  {
    Joystick.setButton(0, currentButtonState);
    lastButtonState = currentButtonState;
  }

  delay(50);
}
```

## Compile and upload the Sketch

1. Click checkmark button in the toolbar to compile and verify the code you write.
2. Click the right facing arrow button to compile and upload the sketch
3. You should see a message in the log window at the bottom of the screen telling you the upload has worked and that the Verify was successful. It should also say that the VPU was reset (i.e. the Arduino was rebooted).

## Change the driver (for Windows)
For Windows computers the Arduino will still keep it's original name hich is shows up. This is due to the Windows device driver still recognising that it is an Arduino. But now (thanks to our changes) the Arduino is acting like a Joystick. You will need to change the driver associated with the device in order for Windows to now treat it like a Joystick.

1. Type 'Device Manager' into the search bar, and press enter
2. Device manager will open.
3. Click on the 'Ports (COM & LPT)' section
4. There will be a Arduino Port listed there with a Error symbol over it. That is the Arduino trying to talk Joystick when Windows thinks it's an Arduino! We need to tell Windows it's a Joystick now.
5. Right-click on the Arduino port entry, and select 'Update driver'
6. Click on 'Browse my computer for driver software'
7. Click on 'Let me pick from a list of available drivers on my computer'
8. Select 'USB Input Device' from the list (DO NOT pick the Arduino devices, as we want Windows to talk Joystick, not Arduino!)

The device should now be listed within the 'Devices and Printers' section of the Control Panel. You can check by:

1. Open up Windows Settings
2. Click on the 'Devices and printers' link in the right hand side of the window.

You should see your new Arduino Joystick listed there as it's original name (e.g. min was listed as 'Arduino Zero').

## Test!
Now it's time to test! The new Arduino Joystick should turn up in your list of USB devices. What you see depends on whether its a Windows system, Mac or Linux.

For Windows computers you can test with the Game controller settings.

1. Open up Windows Settings
2. Click on the 'Devices and printers' link in the right hand side of the window.
3. Right click on the Arduino Joystick you want to change, and click on 'Game controller settings'
4. Select the Arduino from the list and click 'Properties'
5. You should now be able to press all your buttons, and you should see Windows understand them, and show you movement on the test window (Note what you see on the test window depends on what features and functions you use from the JoystickZero library).

If your movements are all odd, you may need to calibrate you joystick.

## Calibrate!
You may need to calibrate the device if the range of motion isn't being fully detected, or the pedals aren't working. For Windows computers you can calibrate with the Game controller settings window.

1. Open up Windows Settings
2. Click on the 'Devices and printers' link in the right hand side of the window.
3. Right click on the Arduino Joystick you want to change, and click on 'Game controller settings'
4. Select the Arduino from the list and click 'Properties'
5. Click on the Settings tab
6. Click on the 'Calibrate' button
7. Follow the calibration wizard to calibrate each of the JoystickZero features you've enabled in your Sketch.

And now enjoy your AWESOME Arduino Joystick!

# A more advanced example
The example below was developed for use in an Analogue Hydraulic Handbrake, so it

* turns off all features except the Brake
* Changes the Brake to a 12-bit resolution as the Arduino Zero supported it
I actually tested this on a NANO-ARM (Arduino Zero compatible board in an Arduino Nano form factor!) and it worked perfectly! You can find out more about the Arduino NANO-ARM here: https://wiki.protoneer.co.nz/NANO-ARM
```
// ===================================
// Analogue Hydraulic Handbrake for Arduino Zero
// by Terry MacDonald
// ===================================
// Connect the +5V cable from the pressure sensor to the Arduino Zero VIN pin
// Connect the ground cable from the pressure sensor to the Arduino Zero GND pin
// Connect the signal cable from the pressure sensor to the Arduino Zero A0 pin
// NOTE - If your Arduino Zero has 3.3V Analogue inputs, then you will need a
//        resistive divider to the pressure sensor signal cable to step
//        the 5V output to 3.3V that the Arduino input needs. Without this
//        you could fry your Arduino! See the Step-down level section here:
//        https://hackaday.com/2016/12/05/taking-it-to-another-level-making-3-3v-and-5v-logic-communicate-with-level-shifters/

// Uses a modified version of Matthew Heironimous' ArduinoJoystickLibrary
// changed to work with many more Arduino devices including the Arduino Zero 
// (see the original at https://github.com/MHeironimus/ArduinoJoystickLibrary)
#include <JoystickZero.h>

// This sets the Analogue pin to read from the Arduino
const int pinConnectedToSensor = A0;

// This sets the resolution of the Arduino, which is the number of digital
// values that the analogue signal will be converted into.
// A 10-bit Analogue resolution gives you 1024 different levels
// A 12-bit Analogue resolution gives you 4096 different levels
const int numBitsAnalogueResolution = 12;
const int maxAnalogueValue = pow(2,numBitsAnalogueResolution)-1;

// This controls the debug mode so you get informative messages in the Serial Monitor
// Change the debug value below to true to get debug messages
// Change the debug value below to false to stop getting debug messages
const bool debug = false;

// This counts the loops so we show debug messages on the Serial Monitor every 10 loops
// just to make sure we don't scroll too many times!
int loopCounter = 0;

// Set the features of the Joystick that we want
// This will customise what USB data is sent to the computer.
// With a handbrake, we ONLY want the Brake Axis....
Joystick_ Joystick(JOYSTICK_DEFAULT_REPORT_ID, JOYSTICK_TYPE_GAMEPAD, 0, 0, false, false, false, false, false, false, false, false, false, true, false);

void setup() {
  
  if (debug) {
    // Wait for the SerialUSB library to be available
    while (!SerialUSB);
    // Start the Serial USB so we report what we're doing
    SerialUSB.begin(115200);
    SerialUSB.println("Joystick starting");
    
    // Set the chosen Arduino pin to Input mode (we don't need pullup resistors)
    SerialUSB.print("Setting signal input pin to ");
    SerialUSB.println(pinConnectedToSensor);
  }
  pinMode(pinConnectedToSensor, INPUT); 
  
  // Change the Analogue Read Resolution to the chosen analogue resolution
  if (debug) {
    SerialUSB.print("Setting analogue resolution to ");
    SerialUSB.print(numBitsAnalogueResolution);
    SerialUSB.println("-bits");
  }
  analogReadResolution(numBitsAnalogueResolution);

  // Start the Joystick emulation
  Joystick.begin(true);

  // Set the Joystick library Brake Max range using the chosen analogue resolution
  if (debug) {
    SerialUSB.print("Setting brake range to 0 <-> ");
    SerialUSB.println(maxAnalogueValue);
  }
  Joystick.setBrakeRange(0,maxAnalogueValue);
}

void loop() {
  // Read the value from the sensor
  int readValue = analogRead(pinConnectedToSensor);

  // Describe what we're doing into the Serial Monitor
  if (debug) {
    if (loopCounter % 10 == 0) {
      SerialUSB.print("PIN ");
      SerialUSB.print(pinConnectedToSensor);
      SerialUSB.print(" set to ");
      SerialUSB.println(readValue);
    }
    loopCounter++;
  }

  // Update the computer with the current Brake value.
  Joystick.setBrake(readValue);

  // Delay for 30 microseconds to try and smooth out the signals
  delay(30);
}
```

# Advanced details courtesy of Matthew Heironimus

## Features
The joystick or gamepad can have the following features:
- Buttons (default: 32)
- Up to 2 Hat Switches
- X, Y, and/or Z Axis (up to 16-bit precision)
- X, Y, and/or Z Axis Rotation (up to 16-bit precision)
- Rudder (up to 16-bit precision)
- Throttle (up to 16-bit precision)
- Accelerator (up to 16-bit precision)
- Brake (up to 16-bit precision)
- Steering (up to 16-bit precision)

## Joystick Library API
The following API is available if the Joystick library in included in a sketch file.

### Joystick\_(...)
Constructor used to initialize and setup the Joystick. The following optional parameters are available:

- `uint8_t hidReportId` - Default: `0x03` - Indicates the joystick's HID report ID. This value must be unique if you are creating multiple instances of Joystick. Do not use `0x01` or `0x02` as they are used by the built-in Arduino Keyboard and Mouse libraries.
- `uint8_t joystickType` - Default: `JOYSTICK_TYPE_JOYSTICK` or `0x04` - Indicates the HID input device type. Supported values:
  - `JOYSTICK_TYPE_JOYSTICK` or `0x04` - Joystick 
  - `JOYSTICK_TYPE_GAMEPAD` or `0x05` - Gamepad
  - `JOYSTICK_TYPE_MULTI_AXIS` or `0x08` - Multi-axis Controller 
- `uint8_t buttonCount` - Default: `32` - Indicates how many buttons will be available on the joystick.
- `uint8_t hatSwitchCount` - Default: `2` - Indicates how many hat switches will be available on the joystick. Range: `0` - `2`
- `bool includeXAxis` - Default: `true` - Indicates if the X Axis is available on the joystick.
- `bool includeYAxis` - Default: `true` - Indicates if the Y Axis is available on the joystick.
- `bool includeZAxis` - Default: `true` - Indicates if the Z Axis (in some situations this is the right X Axis) is available on the joystick.
- `bool includeRxAxis` - Default: `true` - Indicates if the X Axis Rotation (in some situations this is the right Y Axis) is available on the joystick.
- `bool includeRyAxis` - Default: `true` - Indicates if the Y Axis Rotation is available on the joystick.
- `bool includeRzAxis` - Default: `true` - Indicates if the Z Axis Rotation is available on the joystick.
- `bool includeRudder` - Default: `true` - Indicates if the Rudder is available on the joystick.
- `bool includeThrottle` - Default: `true` - Indicates if the Throttle is available on the joystick.
- `bool includeAccelerator` - Default: `true` - Indicates if the Accelerator is available on the joystick.
- `bool includeBrake` - Default: `true` - Indicates if the Brake is available on the joystick.
- `bool includeSteering` - Default: `true` - Indicates if the Steering is available on the joystick.

The following constants define the default values for the constructor parameters listed above:

- `JOYSTICK_DEFAULT_REPORT_ID` is set to `0x03`
- `JOYSTICK_DEFAULT_BUTTON_COUNT` is set to `32`
- `JOYSTICK_DEFAULT_HATSWITCH_COUNT` is set to `2`

### Joystick.begin(bool initAutoSendState)
Starts emulating a game controller connected to a computer. By default, all methods update the game controller state immediately. If `initAutoSendState` is set to `false`, the `Joystick.sendState` method must be called to update the game controller state.

### Joystick.end()
Stops the game controller emulation to a connected computer.

### Joystick.setXAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the X axis. Default: `0` to `1023`

### Joystick.setXAxis(int16_t value)
Sets the X axis value. See `setXAxisRange` for the range.

### Joystick.setYAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Y axis. Default: `0` to `1023`

### Joystick.setYAxis(int16_t value)
Sets the Y axis value. See `setYAxisRange` for the range.

### Joystick.setZAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Z axis. Default: `0` to `1023`

### Joystick.setZAxis(int16_t value)
Sets the Z axis value. See `setZAxisRange` for the range.

### Joystick.setRxAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the X axis rotation. Default: `0` to `1023`

### Joystick.setRxAxis(int16_t value)
Sets the X axis rotation value. See `setRxAxisRange` for the range.

### Joystick.setRyAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Y axis rotation. Default: `0` to `1023`

### Joystick.setRyAxis(int16_t value)
Sets the Y axis rotation value. See `setRyAxisRange` for the range.

### Joystick.setRzAxisRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Z axis rotation. Default: `0` to `1023`

### Joystick.setRzAxis(int16_t value)
Sets the Z axis rotation value. See `setRzAxisRange` for the range.

### Joystick.setRudderRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Rudder. Default: `0` to `1023`

### Joystick.setRudder(int16_t value)
Sets the Rudder value. See `setRudderRange` for the range.

### Joystick.setThrottleRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Throttle. Default: `0` to `1023`

### Joystick.setThrottle(int16_t value)
Sets the Throttle value. See `setThrottleRange` for the range.

### Joystick.setAcceleratorRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Accelerator. Default: `0` to `1023`

### Joystick.setAccelerator(int16_t value)
Sets the Accelerator value. See `setAcceleratorRange` for the range.

### Joystick.setBrakeRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Brake. Default: `0` to `1023`

### Joystick.setBrake(int16_t value)
Sets the Brake value. See `setBrakeRange` for the range.

### Joystick.setSteeringRange(int16_t minimum, int16_t maximum)
Sets the range of values that will be used for the Steering. Default: `0` to `1023`

### Joystick.setSteering(int16_t value)
Sets the Steering value. See `setSteeringRange` for the range.

### Joystick.setButton(uint8_t button, uint8_t value)
Sets the state (`0` or `1`) of the specified button (range: `0` - (`buttonCount - 1`)). The button is the 0-based button number (i.e. button #1 is `0`, button #2 is `1`, etc.). The value is `1` if the button is pressed and `0` if the button is released.

### Joystick.pressButton(uint8_t button)
Press the indicated button (range: `0` - (`buttonCount - 1`)). The button is the 0-based button number (i.e. button #1 is `0`, button #2 is `1`, etc.).

### Joystick.releaseButton(uint8_t button)
Release the indicated button (range: `0` - (`buttonCount - 1`)). The button is the 0-based button number (i.e. button #1 is `0`, button #2 is `1`, etc.).

### Joystick.setHatSwitch(int8_t hatSwitch, int16_t value)
Sets the value of the specified hat switch. The hatSwitch is 0-based (i.e. hat switch #1 is `0` and hat switch #2 is `1`). The value is from 0° to 360°, but in 45° increments. Any value less than 45° will be rounded down (i.e. 44° is rounded down to 0°, 89° is rounded down to 45°, etc.). Set the value to `JOYSTICK_HATSWITCH_RELEASE` or `-1` to release the hat switch.

### Joystick.sendState()
Sends the updated joystick state to the host computer. Only needs to be called if `AutoSendState` is `false` (see `Joystick.begin` for more details).
