| Supported Target | ESP32-C3 |
| ---------------- | -------- |

# ESP32-C3 UART Bridge

The ESP32-C3 UART Bridge is an ESP-IDF project. This makes the ESP32-C3 act as USB to serial UART interface like FT232R or CP210x.
The communication is transferred in both directions between the Computer and the target MCU through the ESP32-C3 UART Bridge.

> NOTE: Firmware tested on ESP32-C3. But it may work in other ESP32 family.

See diagram below:

```
 COMPUTER            ESP32-C3             MCU
┌────────┐     ┌────────┬────────┐     ┌────────┐
│        │     │        │        │     │        │
│        │     │        │        │     │        │
│        │     │        │        │     │        │
│        │     │        │        │     │        │
│  USB   │◄───►│  CDC   │  UART  │◄───►│  UART  │
│        │     │        │        │     │        │
│        │     │        │        │     │        │
│        │     │        │        │     │        │
│        │     │        │        │     │        │
└────────┘     └────────┴────────┘     └────────┘
```


### Hardware Required

The example can be run on ESP32-C3 based development board connected to a computer with a single USB cable for flashing and bridging. The target MCU interface should have 3.3V logic. 

### Setup the Hardware

Connect the target MCU serial interface to the ESP32-C3 as follows.


-----------------------------------------------------------------------------------------
| ESP32-C3 chip         | Kconfig Option     | ESP32-C3 Pin | Target MCU UART Pin |
| ----------------------|--------------------|----------------------|--------------------|
| Transmit Data (TxD)   | UBRIDGE_UART_TXD | GPIO21             | RxD               |
| Receive Data (RxD)    | UBRIDGE_UART_RXD | GPIO20         | TxD               |
| Ground                | n/a                | GND                  | GND               |

See connection diagram below:

```
 ESP32-C3           MCU
┌────────┐       ┌────────┐
│        │       │        │
│        │       │        │
│ GPIO21 ├──────►│  RxD   │
│        │ 3.3v  │        │
│        │ logic │        │
│ GPIO20 │◄──────┤  TxD   │
│        │       │        │
│        │       │        │
└────────┘       └────────┘
```



## How to Compile the Project

[ESP-IDF](https://github.com/espressif/esp-idf) v5.0.1 or newer can be used to compile the project. Please read the
documentation of ESP-IDF for setting up the environment.

- `idf.py menuconfig` can be used to change the default configuration. The project-specific settings are in the "ESP32 Uart Bridge Configuration" sub-menu.
- `idf.py build` will build the project binaries.
- `idf.py -p PORT flash` will flash the ESP32-C3. The `PORT` is the serial port created by an ESP32-C3 USB-CDC profile connected to the USB D+/D- pins of the ESP32-C3. This serial connection has to be established to upload firmware into the ESP32-C3 and it also used to act as bridge. 

The initial flashing can be done by using other methods, see [this guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/get-started/establish-serial-connection.html).

### Configure the project

Use the command below to configure project using Kconfig menu as showed in the table above.
The default Kconfig values can be changed such as: `UBRIDGE_UART_BAUD_RATE`, `UBRIDGE_UART_PARITY`,  `UBRIDGE_UART_PORT_NUM` (Refer to Kconfig file).

```
idf.py menuconfig
```

> NOTE: The default baud rate is `57600` and port number `0`
>
> WARNING: The parity setting by default selected `none`. If you are going to flash STM32 using the ST serial bootloader you MUST select `even` parity. 

### Build and Flash

Build the project and flash it to the board:

```
idf.py set-target esp32c3
idf.py build
idf.py -p PORT flash
```



