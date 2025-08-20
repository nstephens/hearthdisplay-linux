# Background
The hearth display (https://hearthdisplay.com/) is a consumer touch screen, wall mount calendar sharing device similar to products such as skylight. The product has a moderate development cycle, including additional features such as tasks organizing and routines. The initial price is fairly expensive, and requires a $9/mo subscription fee to boot. 

While the hardware is nice, and the interface is adequate, we found this to be insufficient to our use cases and were going to get rid of them. Until we realized that these are android boards that we could potentially do something more interesting with. 

# Goals
Our goals are to unlock the device completely, providing opportunity to flash a newer image and create opportunity to leverage it with a more complete linux distro or updated android install. Some potential goals could be utilizing the device as a Home Assistant dashboard, or leveraging the built-in camera that is currently unused.

Note: sideloading apps for use as a HomeAssistant dashboard is not recommended with the OEM install as the version of webview is quite old and unable to render many of the modern elements.

# Current State
Work is being driven to create backup images to allow for OEM restore if things go awry. Once that is complete, we will begin working on a full root, and then begin efforts to create compatible images that match the above goals.

Nothing usable or suggested yet.

# Device Details
## Hardware

[RockChip rk3568 board](https://www.rock-chips.com/uploads/pdf/2022.8.26/191/RK3568%20Brief%20Datasheet.pdf)
2GB RAM

Daughterboards:
- usb header digitizer
- mic
- usb/ethernet hub
- button outputs

## Software
Android 10.3

# Steps

1. Click gear icon in top right of main view, select 'Display Settings'
2. Click 'Advanced settings' in lower left of sidebar
3. Select S'ystem'
4. Select 'Developer Options'
5. Under debugging section, enable 'USB Debugging' and (if desired) 'Wireless Debugging'
6. Enable 'Disable adb authorization timeout'

At this point you can connect a [USB mini cable](https://a.co/d/cOgfh2Z) to the back of the display and to your computer. Otherwise you can connect wirelessly. If you choose to go wireless, we strongly recommend using [scrcpy](https://github.com/Genymobile/scrcpy) to remotely manage the display.

