## Sonoff-Tasmota - digiDIM Temporary Fork @ 6.6.0.6

Precompiled bins supplied in the bins folder of this repo.

### Changes

- Dimming status sent at all power state changes 
- Power topic updated if a dimming change causes the power state to change
- Martin Jerry SD-01 Dimmer support
- digiDIMv(X) indicated in the information tab for support purposes

I personally use this build on Tuya based dimmers and the Martin Jerry SD-01 Dimmer in our home with Home Assistant. [Shown here](https://www.digiblur.com/2018/12/state-of-dimmer-tasmota-dimmer-updates.html)

[Martin Jerry SD-01 Dimmer](https://amzn.to/2L8XeFS)	

[Tessan SD-02 Dimmer](https://amzn.to/2TfmTzh)	

[Lesim Dimmer with Number Display](https://amzn.to/2EetlT1)	

[Upgraded TreatLife Dimmer with Touch Panel](https://amzn.to/2Tbym2N)	

[Moes Dimmer similar to the Oittm but possibly ships to additional countries](https://amzn.to/2PvO1bm)

### Martin Jerry SD-01 & Tessan SD-02 Dimmer Setup

## Manual Flashing(Soldering Method)

Unplug the faceplate from the rear dimming module until you are ready to connect it to mains power!  Do not connect the USB flasher to the faceplate while mains power is applied to the unit!  The magic smoke will come out or worse!

Solder the wires for flashing like you normally would for a Tuya module flash [as shown here](https://github.com/arendst/Sonoff-Tasmota/wiki/SM-SO301) .  You do not need to solder GPIO 0 as the UP button is also GPIO 0, simply hold this button up during boot.  Use the latest bin file provided in this [folder](https://github.com/digiblur/Sonoff-Tasmota/tree/development/bins).

## Tuya-Convert (OTA Method)

The device should be in pairing mode (fast blink) upon applying power to the switch.  Follow the standard Tuya-Convert process.  After the dimmer is on the network, download the latest latest bin file provided in this [folder](https://github.com/digiblur/Sonoff-Tasmota/tree/development/bins) to a folder on your computer.  Use the Firmware Upgrade button on the Tasmota GUI and browse to the bin file that was just downloaded.

## Setup

Once you have the device connected to your WiFi and MQTT broker, change the module type to the MJ-SD01 Dimmer.  Let the device reboot and issue the following backlog on the console.  Make sure every command takes effect:

```
backlog rule1 on switch3#state=2 do dimmer + endon on switch2#state=2 do dimmer - endon on switch2#state=3 do dimmer 20 endon on switch3#state=3 do dimmer 100 endon; rule1 1; setoption32 8; switchmode1 6; switchmode2 5; switchmode3 5
```

Once the switch reboots.  Issue a switchmode3 command and make sure it comes back as a setting of "5".  This will verify all of the above backlog commands went through correctly.  If you do not see switchmode3 set to 5, you can issue each of the backlog commands separately one at a time and watch them go through.

In the rule1 you can change the "do dimmer 20" section to any value you like, a long press of down will set dimmer to 20%, a long press up will set the dimmer to 100%.  Modify these to your needs.

Optional Rule for ON/OFF long press to send an MQTT toggle message to another switch/topic (example):  
```
rule2 on switch1#state=3 do publish Table-Dimmer/Main TOGGLE endon 
```
```
rule2 1
```
NOTE: In the future, when you are preparing to flash a stock build of Tasmota to the MJ-SD01 Dimmer, select the Generic template first before flashing to prevent a possible conflict with another device template.

BONUS: Want the Red LED on while the light is off? Run this rule:  
```
Rule3 on power1#state=1 do ledpower 0 endon on power1#state=0 do ledpower 1 endon  
```
```
Rule3 1
```

--------

## Sample Configuration YAML Code

```yaml
- platform: mqtt
  name: "TuyaTest"
  state_topic: "stat/TuyaTest/POWER"
  command_topic: "cmnd/TuyaTest/POWER"
  availability_topic: "tele/TuyaTest/LWT"
  brightness_state_topic: "stat/TuyaTest/RESULT"
  brightness_command_topic: "cmnd/TuyaTest/Dimmer"
  brightness_scale: 100
  brightness_value_template: "{{ value_json.Dimmer }}"
  qos: 1
  payload_on: "ON"
  payload_off: "OFF"
  payload_available: "Online"
  payload_not_available: "Offline"
  retain: false
```
