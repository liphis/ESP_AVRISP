# AVR In-System Programming over WiFi for ESP32

This library allows an ESP32 module with the HSPI port available to become
an AVR In-System Programmer.

## Hardware

The ESP32 module connects to the AVR target chip via the standard 6-pin
AVR "Recommended In-System Programming Interface Connector Layout" as seen
in [AVR910](http://www.atmel.com/images/doc0943.pdf) among other places.

If the AVR target is powered by a different Vcc than what powers your ESP32
chip, you **must provide voltage level shifting** or some other form of buffers.
Exposing the pins of ESP32 to anything larger than 3.6V will damage it.

Connections are as follows:

ESP32 | AVR / SPI  
--------|------------
GPIO12  | MISO
GPIO13  | MOSI
GPIO14  | SCK
any*    | RESET

For RESET use a GPIO other than 0, 2 and 15 (bootselect pins), and apply an
external pullup/down so that the target is normally running.

## Usage

See the included example. In short:

```arduino

// Create the programmer object
ESP32AVRISP avrprog(PORT, RESET_PIN)
// ... with custom SPI frequency
ESP32AVRISP avrprog(PORT, RESET_PIN, 4e6)

// Check current connection state, but don't perform any actions
AVRISPState_t state = avrprog.update();

// Serve the pending connection, execute STK500 commands
AVRISPState_t state = avrprog.serve();
```

### License and Authors

This library started off from the source of ArduinoISP "sketch" included with
the Arduino IDE:

    ArduinoISP version 04m3
    Copyright (c) 2008-2011 Randall Bohn
    If you require a license, see
        http://www.opensource.org/licenses/bsd-license.php

    Support for TCP on ESP32
    Copyright (c) Kiril Zyapkov <kiril@robotev.com>.
