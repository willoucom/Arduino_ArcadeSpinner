# Arduino_ArcadeSpinner

This project is a modern re-creation of the spinners used in arcades, mainly for brick-breakers like arkanoid for example.

The objective is to offer a feeling close to the original equipment by using components that are easy to obtain.

![Img!](img/pic.jpg "Arcade Spinner")

## In action

### Arkanoid with rotary controller
[![IMAGE ALT TEXT](http://img.youtube.com/vi/QnDsVMLP6zM/0.jpg)](http://www.youtube.com/watch?v=QnDsVMLP6zM "Arkanoid with rotary controller on MisterFPGA")

### Puzz'Loop 2 with rotary controller
[![IMAGE ALT TEXT](http://img.youtube.com/vi/TzKd8ytODUM/0.jpg)](http://www.youtube.com/watch?v=TzKd8ytODUM "Puzz'Loop 2 with rotary controller on MisterFPGA")

### Eco Fighter / Ultimate Ecology with rotary controller
[![IMAGE ALT TEXT](http://img.youtube.com/vi/sb1mO-uq1ag/0.jpg)](http://www.youtube.com/watch?v=sb1mO-uq1ag "Eco Fighter / Ultimate Ecology with rotary controller on MisterFPGA")



## Usage

By default the device operate as a 4 button controller with Spinner/Paddle attached

You can activate an additional behavior by holding a button when you plug in the device : 

- Press Button0 (Big Left button) to enable MouseX Emulation
- Press Button1 (Big Right button) to enable Paddle Emulation
- Press Button2 (Small Left button) to switch device to mr.Spinner (enabling composite device on mister fpga)
 - In mr.Spinner mode, each button press is redirected to button 1

## MisterFPGA

### Working

| Game                          | Note|
|---                            |--- | 
| Arkanoid (World)              | |
| Block Gal                     | |
| Breakout (TTL)                | Enable spinner in controls|
| Crater Raider                 | |
| Dark Planet                   | |
| Demolition Derby              | |
| Discs of Tron                 | |
| Eco Fighters                  | Use mr.Spinner mode |
| Forgotten Worlds              | Use mr.Spinner mode |
| Kick                          | |
| Kozmik Krooz'r                | |
| Midnight Resistance           | Use mr.Spinner mode |
| Moonwar                       | |
| Pong                          | Use Paddle emulation and set your player to Paddle |
| Pop'n Bounce (NeoGeo)         | Enable Paddle in Dip settings (Soft dip->Pop'n bounce->Controller = paddle) |
| Puzz Loop 2                   | Enable Paddle in Service mode (System -> Operation) |
| Tron                          | |
| Two Tigers (tron conversion)  | |

### Working with workaround
| Game                  | Note|
|---                    |---| 
| Block block (world)   | Use alternatives/block block (world 910910), must use Mouse Emulation |
| Super Breakout        | Enable paddle in controls, must use paddle emulation |

## Mame

You can use the controller with mame and derivative by using the controller in Mouse Emulation mode.

> Note: You can define the default behavior of the controller by modifying the source code, to do this modify the default value of the variable `bool mouse_emu = 0;` to 1t which will have the effect of operating the controller in mouse mode

You need to change the `dial device assignment` option in the mame settings either in mame.ini or using the GUI.

### Edit mame.ini 
```
dial_device               mouse
```
### Using MAME GUI

![Img!](img/mame.jpg "MAME")

## Build yourself

### Parts 

- Rotary encoder (Model: C38S6G5-600B-G24N)
- 40mm Knob with internal diameter of 6mm 
- Arduino Pro Micro
- 2 24mm button
- 2 30mm button
- Enclosure 3D printed or any box

#### Prepare the rotary encoder

![Img!](img/encoder.jpg "rotary encoder")

1. Remove the metal enclosure and extract the mechanism.
2. Unsolder all wires.
3. Solder fresh wires on GND, VCC, A and B pads.
4. Recycle the original metal case and wire for another project.

### Arduino Pinout

![Img!](img/arduino.jpg "Arduino")

The buttons must be grounded, you can create a daisy chain to use a common GND for your 4 buttons, you can solder the buttons or use wire connectors.

Bridge pad J1 to send +5v to encoder instead of 3.3v

```
Encoder
GND     GND
VCC     VCC
GP_3    A
GP_2    B

Buttons
GP_5    Button0
GP_4    Button1
GP_15   Button2
GP_14   Button3
```

### Flash your Arduino

1. Install Aduino IDE 2.x
2. Open Arduino_ArcadeSpinner.ino
    1. (Optionnal) Change options in the configuration section.
3. Connect your Arduino pro micro to your computer
4. Select Arduino Leonardo with the correct com port
5. Flash the controller

## Technical informations
In the mister source code there is two special cases

If the device name start by MiSTer-S1 the device is considered as a mr.Spinner

```
// mr.Spinner
// 0x120  - Button
// Axis 7 - EV_REL is spinner
// Axis 8 - EV_ABS is Paddle
// Overlays on other existing gamepads
if (strstr(uniq, "MiSTer-S1")) input[n].quirk = QUIRK_PDSP;
if (strstr(input[n].name, "MiSTer-S1")) input[n].quirk = QUIRK_PDSP;
```

If the device name start by MiSTer-A1 the device is considered as an arcade stick with a spinner and/or paddle
```
// Arcade with spinner and/or paddle:
// Axis 7 - EV_REL is spinner
// Axis 8 - EV_ABS is Paddle
// Includes other buttons and axes, works as a full featured gamepad.
if (strstr(uniq, "MiSTer-A1")) input[n].quirk = QUIRK_PDSP_ARCADE;
if (strstr(input[n].name, "MiSTer-A1")) input[n].quirk = QUIRK_PDSP_ARCADE;
```

## Rotary encoder

The rotary encoder uses a magnet to detect rotation, it translates the orientation into pulses which are then interpreted by the arduino controller.

![Img!](img/magnet.jpg "Magnet view")
![Img!](img/magnet2.jpg "Magnet controller")
