Use a simple ESP82xx device to connect to the `vtrust-flash` access point as required by [Tuya-Convert](https://github.com/ct-Open-Source/tuya-convert). This simple sketch continually attempts to connect to `vtrust-flash` until successfully connected. This eliminates the need to connect a PC or a mobile device to this AP.  

Use a NodeMCU or a Wemos D1 Mini and flash [`tc_donor_mini.generic_1M.bin`](https://github.com/digiblur/Sonoff-Tasmota/blob/development/tc_donor_mini/tc_donor_mini.generic_1M.bin) on it using the usual [Tasmota flash settings](https://github.com/arendst/Sonoff-Tasmota/wiki/Flashing) (DOUT, erase flash, etc.).  

Once the device is flashed, apply power and start the Tuya-Convert process. As soon as the `start_flash.sh` creates the `vtrust-flash` access point, this device will connect to it and remain connected to it as long as the AP is available (throughtout flashing one or more Tuya devices).  
