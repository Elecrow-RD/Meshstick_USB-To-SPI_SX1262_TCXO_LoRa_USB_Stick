# USB MESHSTICK Elecrow Driver Installation and Firmware Flashing Guide

This guide explains how to install the USB MESHSTICK Elecrow V1.0 driver on Windows and use IMSProg to flash the test firmware to the onboard AT24C02 EEPROM.

## File Structure

```text
.
├── firmware/
│   └── 24C02C-20260713.bin
├── docs/
│   └── images/
├── IMSProg_win-amd64/
│   ├── driver/
│   │   └── InstallDriver.exe
│   └── IMSProg.exe
├── driver-installed.jpg
├── driver-not-installed.jpg
└── README.md
```

- Driver installer: `IMSProg_win-amd64/driver/InstallDriver.exe`
- Firmware file: `firmware/24C02C-20260713.bin`
- Flashing tool: `IMSProg_win-amd64/IMSProg.exe`

## Requirements

- Windows computer
- USB MESHSTICK Elecrow V1.0
- USB data cable

## Install the Driver

1. Open the `IMSProg_win-amd64/driver` folder.
2. Right-click `InstallDriver.exe` and select **Run as administrator**.

   ![Select the driver installer](./docs/images/driver-installer-file.png)

3. In the Device Driver Installation Wizard, click **Next**.

   ![Device Driver Installation Wizard](./docs/images/driver-install-wizard.png)

4. Wait for the driver installation to finish, then click **Finish**.

   ![Driver installation completed](./docs/images/driver-install-complete.png)

If Windows displays a User Account Control prompt, confirm it to continue the installation.

After connecting the device to a USB port, check its power status using the indicator light:

| Before driver installation | After driver installation |
| --- | --- |
| ![Before driver installation](./driver-not-installed.jpg) | ![After driver installation](./driver-installed.jpg) |

## Flash the Firmware

1. Open the `IMSProg_win-amd64` folder and run `IMSProg.exe`.

   ![Run IMSProg](./docs/images/imsprog-launch.png)

2. Configure the chip settings on the left side of IMSProg as follows:

   | Setting | Value |
   | --- | --- |
   | Type | `24 EEPROM` |
   | Manufacturer | `ATMEL` |
   | Name | `AT24C02` |
   | Size | `256 B` |
   | Page size | `8` |
   | VCC, Volt | `3.3 V` |
   | I2C bus speed | `400 kHz` |

3. Click the **Open File** button on the toolbar and select `firmware/24C02C-20260713.bin`.

   ![Configure the chip and open the firmware](./docs/images/imsprog-chip-settings.png)

4. Remove the device enclosure before connecting the USB MESHSTICK to the computer.
5. Before flashing, install a jumper cap on the J2 header to short the two J2 pins. This changes the main controller's BOOT pin level and forces the device into firmware flashing (BOOT download) mode. With the jumper cap removed, the device starts in normal operating mode.

   ![Device enclosure removed and J2 jumper cap installed](./docs/images/j2-jumper-cap-installed.png)

   **Important:** J2 must be shorted before the device is connected. Otherwise, the computer cannot detect the flashing interface.

6. Connect the USB MESHSTICK to a USB port on the computer. IMSProg should automatically detect the device and display a green `Connected` status at the bottom of the window.
7. Keep `Erase`, `Program`, and `Verify` selected, then click `Go!`.

   ![Confirm the connection and start flashing](./docs/images/imsprog-connected-go.png)

   If IMSProg displays `Not Connected`, check that the J2 jumper cap is firmly installed and reliably shorts both J2 pins.

8. Wait for flashing and verification to finish. The message `The operation was successful!` confirms that the firmware was flashed successfully. Unplug the USB device, then remove the J2 jumper cap to restore normal startup mode.

   ![Firmware flashed successfully](./docs/images/flash-success.png)

   > **Warning:** The J2 jumper cap must be removed after flashing. If J2 remains shorted, the device will stay in flashing mode after power-on and cannot run normal Mesh functions.

9. Reassemble and reconnect the device, then repeat the communication transmission and reception test to confirm that it operates normally after flashing.

Do not disconnect the device or close IMSProg while flashing is in progress. Only unplug it after IMSProg reports a successful operation.

## Troubleshooting

### IMSProg Does Not Display Connected

1. Confirm that the driver was installed successfully.
2. Confirm that the J2 jumper cap is firmly installed and shorts both J2 pins.
3. Disconnect and reconnect the USB MESHSTICK.
4. Try another USB port or USB data cable.
5. Close and reopen IMSProg.

### Firmware Flashing Fails

1. Confirm that `firmware/24C02C-20260713.bin` is selected.
2. Verify that the chip model, voltage, and I2C bus speed match the settings listed above.
3. Confirm that the status is `Connected`, then click `Go!` again.

## File Verification

After downloading or copying the files, use SHA-256 to verify their integrity:

```text
InstallDriver.exe
7CAF24057C5047F1EADDAA8E72E6B8B52DAEC24284BBB96B3DACFE9EF669FB0D

24C02C-20260713.bin
C14D0A7029E3D46F1A179C80E08C896195BF5D47245DBB68CFFFF5C5605964F0
```
