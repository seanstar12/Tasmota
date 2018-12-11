Martin Jerry SD-01 Dimmer Setup

Unplug the faceplate from the rear dimming module until you are ready to connect it to mains power!  Do not connect the USB flasher to the faceplate while mains power is applied to the unit!

Solder the wires for flashing like you normally would for a Tuya module flash.  You do not need to solder GPIO 0 as the UP button is also GPIO 0, simply hold this button up during boot.  

Once you have the device connected to your WiFi and MQTT broker.  Change the module type to the MJ-SD01 Dimmer.  Let the device reboot and issue the following backlog on the console.  Make sure every command takes effect:

backlog rule1 on switch3#state=2 do dimmer + endon on switch2#state=2 do dimmer - endon on switch2#state=3 do dimmer 20 endon on switch3#state=3 do dimmer 100 endon; rule1 1; setoption32 10; switchmode1 6; switchmode2 5; switchmode3 5

Optional Rule for ON/OFF long press to send an MQTT toggle message to another switch/topic (example):
rule2 on switch1#state=3 do publish Table-Dimmer/Main TOGGLE endon
rule2 1

In the rule1 you can change the "do dimmer 20" section to any value you like, a long press of down will set dimmer to 20%, a long press up will set the dimmer to 100%.  Modify these to your needs.
