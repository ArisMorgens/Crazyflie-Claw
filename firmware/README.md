# Firmware


## Claw deck driver



Clone the [crazyflie-firmware](https://github.com/bitcraze/crazyflie-firmware) repository and add the driver for the claw deck ([claw.c](claw.c)) in [src/deck/drivers/src](https://github.com/bitcraze/crazyflie-firmware/tree/master/src/deck/drivers/src).

Also, add this line in [src/deck/drivers/src/Kbuild](https://github.com/bitcraze/crazyflie-firmware/blob/master/src/deck/drivers/src/Kbuild) to initialize the driver:
```
obj-y += claw.o
```

## Configure the firmware

Based on your platform, use its default configuration by running the following command:

#### Crazyflie 2.0, Crazyflie 2.1(+)
```
make cf2_defconfig
```
#### Crazyflie 2.1 Brushless
```
make cf21bl_defconfig
```
#### Crazyflie Bolt
```
make bolt_defconfig
```
Then, to force the deck driver, run this command:
```
make menuconfig
```
Navigate to **Expansion deck configuration → Deck discovery backends → Force load specified custom deck driver** and type "Claw".


## Build the firmware
Build the firmware on Linux by running this command:
```
make -j$(nproc)
```

## Flash the firmware

#### Manually entering bootloader mode

* Turn the Crazyflie off
* Start the Crazyflie in bootloader mode by pressing the power button for 3 seconds. Both the blue LEDs will blink.
* In your terminal, run
```
make cload
```

#### Automatically enter bootloader mode
* Make sure the Crazyflie is on
* In your terminal, run `CLOAD_CMDS="-w [CRAZYFLIE_URI]" make cload`
* or run `cfloader flash build/[BINARY_FILE] stm32-fw -w [CRAZYFLIE_URI]`
with [BINARY_FILE] being the binary file (**cf2.bin** for Crazyflie 2.x, **cf21bl.bin** for Crazyflie 2.1 Brushless etc.) and [CRAZYFLIE_URI] being the uri of the Crazyflie.


## Test the deck
After flasing the firmware to the Crazyflie and connecting to `cfclient`, the debug prints of the ClawDeck should appear in the Console tab.

```
---
SYS: Crazyflie 2.1 is up and running!
SYS: Build 9:06c667155de5 (2025.02 +9) CLEAN
SYS: I am 0x3933333533344718002D002E and I have 1024KB of flash!
CFGBLK: v1, verification [OK]
DECK_INFO: CONFIG_DECK_FORCE=Claw found
DECK_INFO: compile-time forced driver Claw added
DECK_CORE: 1 deck(s) found
DECK_CORE: Calling INIT on driver Claw for deck 0
ClawDeck: Initialize ClawDeck!
IMU: BMI088: Using I2C interface.
IMU: BMI088 Gyro connection [OK].
IMU: BMI088 Accel connection [OK]
IMU: BMP388 I2C connection [OK]
ESTIMATOR: Using Complementary (1) estimator
CONTROLLER: Using PID (1) controller
MTR-DRV: Using brushed motor driver
SYS: About to run tests in system.c.
SYS: NRF51 version: 2024.10 (CF21)
EEPROM: I2C connection [OK].
STORAGE: Storage check [OK].
IMU: BMI088 gyro self-test [OK]
ClawDeck: ClawDeck passed the test!
DECK_CORE: Deck 0 test [OK].
SYS: Self test passed!
STAB: Wait for sensor calibration...
SYS: Free heap: 17944 bytes
STAB: Starting stabilizer loop
```
