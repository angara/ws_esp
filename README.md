# ESP32 based weather station

## Wind sensor wiring

PR-3000-FSJT-N01

- green - 485-A
- blue  - 485-B
- black - Gnd
- brown - +Vcc (10-30V)

4800 8N1 CRC

120 Ohm line terminal resistor

addr: 1

- https://devices.esphome.io/devices/Renke-RS-FSJT-N01-Wind-Speed
- https://devices.esphome.io/devices/Renke-RS-FXJT-N01-Wind-Direction

### Modbus notes

Sample:

- send (addr, func, reg, num, crc): 0x01  0x03  0x00 0x00  0x00 0x01  0x84 0x0A
- recv (addr, func, len, bytes, crc): 0x01  0x03  0x02  0x00 0x56  0x38 0x7A
- value: 0x00 0x56 -> 86 -> 8.6 m/s

config registers: 40001, 40002

- ? 0100H 40101 device address (0-252) read and write
- ? 0101H 40102 baud rate (2400/4800/9600) read and write
- ? The device modbus address is stored in register 2000.
- ? The device baud rate is configured in register 2001 using an ID:

- ? Baud rate ID
- ? 2400  0
- ? 4800  1
- ? 9600  2

```sh
mbpoll -m rtu -b 4800 -d 8 -P none -s 1 -a 1 -r 0 -c 1 -l 1000 -o 1 /dev/tty.usbserial-1340
```

- https://kotyara12.ru/iot/esp32_rs485_modbus/
- https://minimalmodbus.readthedocs.io/en/stable/
- https://pymodbus.readthedocs.io/en/latest/ ???

## ESP32

- https://lastminuteengineers.com/esp32-pinout-reference/
- https://lastminuteengineers.com/esp32-wroom-32-pinout-reference/
- https://github.com/espressif/esptool
- https://docs.micropython.org/en/latest/reference/mpremote.html

```sh
pip install esptool mpremote
```

- https://micropython.org/download/ESP32_GENERIC/
- https://micropython.org/download/SEEED_XIAO_SAMD21/

### Other tools

- https://pypi.org/project/adafruit-ampy/
- https://github.com/dhylands/rshell
- https://github.com/wendlers/mpfshell

## Application

### Libraries

Copy files from repo to `mrequests` folder:

- https://github.com/SpotlightKid/mrequests

### Config

Copy `config.py` to `.config.py` and set correct variable values.

```python
SUBMIT_USER = ""
SUBMIT_PASS = ""

WIFI_SSID = ""
WIFI_PASS = ""
```

Autostart at boot (set/remove)

```python
with open("main.py",'w') as f: f.write("import app; app.main()")

import os; os.remove('main.py')
```
