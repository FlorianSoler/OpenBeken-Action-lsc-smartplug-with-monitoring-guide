# Introduction
This guide provides instructions for flashing the Action LSC Smart Power Monitoring Plug with the WB2S Tuya board, featuring the [BK7231T MCU](https://developer.tuya.com/en/docs/iot/wb2s-module-datasheet?id=K9ghecl7kc479).

![lsc_smart_plug_box.png](./_resources/lsc_smart_plug_box.png)

## Requirements:
* USB to TTL or UART reader
![usb_ttl_uart_back.png](./_resources/usb_ttl_uart_back.png)
* Soldering iron
* Dupont cable or any wire for soldering
* [OpenBeken flasher software](https://openbekeniot.github.io/webapp/devicesList.html) or any other tool for flashing firmware.

# Step One

Begin by opening the smart plug case with a flat screwdriver.


![oppened_plug.png](./_resources/oppened_plug.png)


After opening, you should have access to the board.

# Step Two

Locate the Tuya board number on the back of the blue board:
![board_number.png](./_resources/board_number.png)
Google the board name, find Tuya documentation, and confirm the MCU (in this case, the board is a WB2S with [BK7231T MCU](https://developer.tuya.com/en/docs/iot/wb2s-module-datasheet?id=K9ghecl7kc479)). The manufacturer provides all the required data for flashing:

| Pin No. | Symbol | I/O type | Function                                               |
|---------|--------|----------|--------------------------------------------------------|
| 1       | VBAT   | P        | Power supply pin (3.3 V), connected to VBAT on IC      |
| 2       | PWM2   | I/O      | Common GPIO, connected to P8 on IC                     |
| 3       | GND    | P        | Power supply reference ground pin                     |
| 4       | PWM1   | I/O      | Common GPIO, connected to P7 on IC                     |
| 5       | 1RX    | I/O      | UART1_RXD, used as user-side serial interface, connected to P10 on IC |
| 6       | PWM0   | I/O      | Common GPIO, connected to P6 on IC                     |
| 7       | 1TX    | I/O      | UART1_TXD, used as user-side serial interface, connected to P11 on IC |
| 8       | AD     | AI       | ADC pin, connected to P23 on IC                        |
| 9       | PWM4   | I/O      | Common GPIO, connected to P24 on IC                    |
| 10      | CEN    | I        | Low-level reset, high-level active, Docking IC-CEN (internally pulled high) |
| 11      | PWM5   | I/O      | Common GPIO, connected to P26 on IC                    |

# Step Three

Solder wires on the GROUND, VBAT, RX, and TX pins. Highlighted pinout:

![board_wb2S.png](./_resources/board_wb2S.png)

![soldered_wire.png](./_resources/soldered_wire.png)

Connect pinout wires to the USB to UART adapter (ensure the MCU supports only 3.3V), and check Tuya RX is on the TTL board's TX and vice versa.

![usb_ttl_connected.png](./_resources/usb_ttl_connected.png)

# Step Four

Plug the UART reader into your computer and launch [OpenBeken flasher software](https://openbekeniot.github.io/webapp/devicesList.html).

 

![easy_uart_flasher.png](./_resources/easy_uart_flasher.png)

Start by making a firmware dump (backup) by clicking **Do firmware backup (read) only**.

![firmware_Backup.png](./_resources/firmware_Backup.png)

After making the backup, obtain the firmware pinout configuration in JSON, and save it.

![main_firmware_json_extraction.png](./_resources/main_firmware_json_extraction.png)

Locate the corresponding firmware and select the right one for your MCU, using the **_UA.bin** file, and click flash.

# Step Five

Unsolder all wires, put the board back in its enclosure, and plug it into an outlet. You should be able to see a WiFi hotspot:


![wifi_hotspot.png](./_resources/wifi_hotspot.png)

Go to the WiFi properties, enter the gateway IP in a web browser:

![wifi_gateway.png](./_resources/wifi_gateway.png)

You should see a web page like this:
![Capture d’écran 2024-02-23 183741.png](./_resources/Capture%20d’écran%202024-02-23%20183741.png)

# Step Six

Configure the power plug by inserting the JSON obtained in Step Four. Go to **launch web app** and into **import**, paste the JSON, and click **clear OBK and apply new script**.

![Capture d’écran 2024-02-23 184658.png](./_resources/Capture%20d’écran%202024-02-23%20184658.png)

After that you should have the following page when you return to the base menu :

![flashed_module.png](./_resources/flashed_module.png)

here my json obtained by reading the dump firmware :
```json
{
   "sel_pin_pin": 11,
   "rl1_lv": 1,
   "bt1_pin": 7,
   "led1_pin": 8,
   "net_trig": 2,
   "jv": "1.2.0",
   "netled1_lv": 1,
   "netled_reuse": 1,
   "bt1_type": 0,
   "ffc_select": 1,
   "vi_pin": 24,
   "resistor": 1,
   "over_cur": 17800,
   "bt1_lv": 0,
   "reset_t": 5,
   "netled1_pin": 10,
   "chip_type": 0,
   "lose_vol": 180,
   "over_vol": 264,
   "module": "WB2S",
   "ele_pin": 26,
   "ch_cddpid1": 9,
   "ch1_stat": 0,
   "led1_lv": 1,
   "rl1_type": 0,
   "ch_num": 1,
   "rl1_pin": 6,
   "vol_def": 0,
   "ch_dpid1": 1,
   "sel_pin_lv": 1,
   "crc": 10
}
```

# Congrat ! you have flash your action smart plug !
### Refer to [Tasmota Getting-Started](https://tasmota.github.io/docs/Getting-Started/#__tabbed_2_2)  or [OpenBeken website](https://openbekeniot.github.io/webapp/devicesList.html) guide for connecting this to you wifi and for using **MQTT**.