# Anycubic Kobra 2 Neo / GO LCD KlipperScreen

This will help you to set-up LCD for Anycubic Kobra 2 Neo, Kobra Go equipped with Klipper and Raspberry Pi.
Make sure you already have [KlipperScreen installed](https://klipperscreen.readthedocs.io/en/latest/Installation/).

## Power Connections
You can use power from the in-built printer PCB or the Raspberry Pi (RPI).

- **5V** -> 5V
- **GND** -> GND

## SPI Connections
- **SCK** -> GPIO 11 (SPI SCLK)
- **MOSI** -> GPIO 10 (SPI MOSI)
- **CS** -> GPIO 8 (SPI CE0)
- **RESET** -> GPIO 25
- **DC** (marked as MISO on the mainboard) -> GPIO 24

You can find images in the [ImagesMbWiring](ImagesMbWiring).

For images and more details, check the [Hardware Setup](https://github.com/jokubasver/Anycubic-Kobra-Go-Neo-LCD-Driver?tab=readme-ov-file#hardware-setup).

---

## Installation

You can install the LCD firmware binary by simply copy it to `/lib/firmware/`:

1. Clone the repository:

    ```bash
    git clone https://github.com/cheadrian/kobra2neo-klipper
    ```

2. Navigate to the firmware directory:

    ```bash
    cd ~/kobra2neo-klipper/KlipperScreen
    ```
3. Copy the firmware binary to the system:

    ```bash
    sudo cp kobra2neo-lcd.bin /lib/firmware/
    ```
   Now you go bellow to see how to modify `config.txt`.
---

## Creating the LCD Firmware Binaries

Optional: if you want to modify the LCD init command.

To create the firmware binary from scratch follow these steps:

1. Set the appropriate permissions for the command tool:

    ```bash
    chmod 755 mipi-dbi-cmd
    chmod +x mipi-dbi-cmd
    ```

2. Create the firmware binary:

    ```bash
    ./mipi-dbi-cmd kobra2neo-lcd.bin kobra2neo-lcd.txt
    ```

3. Copy the firmware binary to the system:

    ```bash
    sudo cp kobra2neo-lcd.bin /lib/firmware/
    ```

For tool information, check [this link](https://github.com/notro/panel-mipi-dbi/wiki).

---

## Raspberry Pi configuration

**Note:** If you're using a UART interface for Klipper-MCU communication, follow the steps below. If you're using a USB connection for Klipper, you can skip these lines.

1. Open the configuration file:

    ```bash
    sudo nano /boot/firmware/config.txt
    ```
	
2. Disable vc4-kms by commenting this line:

	```bash
	# dtoverlay=vc4-kms-v3d
	```

3. Add the following lines to enable UART and set up the LCD overlay:

    ```bash
    [all]
    # Enable UART interface for Klipper-MCU communication
    enable_uart=1
    dtoverlay=disable-bt
    
    # Set up LCD overlay and firmware details
    dtoverlay=mipi-dbi-spi,spi0-0,speed=32000000
    dtparam=compatible=kobra2neo-lcd\0panel-mipi-dbi-spi
    dtparam=write-only,cpha,cpol
    dtparam=width=320,height=240,width-mm=49,height-mm=37
    dtparam=reset-gpio=25,dc-gpio=24
	
	# Disable RPI splashscreen
	disable_splash=1
	
    ```
	
4. Display the kernel boot console at start up:

	```bash
	sudo nano /boot/firmware/cmdline.txt
	```
	
	Append this at the end:

	```bash
	fbcon=map:1 logo.nologo quiet
	```

5. Save and reboot:

    ```bash
    sudo reboot
    ```

---

## Control the interface

While the Kobra 2 Neo / Go doesn't have touchscreen, you can control interface by using an USB mouse or configure the rotary encoder to act as one.

You can find more details [about encoder here](https://github.com/SomeSpaceNerd/KlipperScreen-Encoder-Driver).

To display the cursor on KlipperScreen simply create `KlipperScreen.conf` in MainSail / Fluidd config and enter:

```bash
[main]
show_cursor: True
```

---

## Relevant Documentation

- [ILI9341V Display Controller Datasheet](https://www.orientdisplay.com/pdf/ILI9341V.pdf)
- [ST7796s Display Controller Datasheet](https://www.displayfuture.com/Display/datasheet/controller/ST7796s.pdf)

---

### `kobra2neo-lcd.txt` is based on [this discussion](https://forums.raspberrypi.com/viewtopic.php?p=2207118#p2207118).

---
