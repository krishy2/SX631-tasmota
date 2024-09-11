# SX631-tasmota with Slimmelezer+
Read the SX631 meter with Slimmelezer and Tasmota

I've ordered the Slimmelezer+: https://www.zuidwijk.com/product/slimmelezer-plus/

How to use it with Tasmota?
https://github.com/helander/tasmota-slimmelezer

1. You need to build your own firmware.
https://tasmota.github.io/docs/Compile-your-build/
I've used Tasmocompiler with the following custom parameters:

#ifndef USE_SCRIPT
#define USE_SCRIPT
#endif
#ifndef USE_SML_M
#define USE_SML_M
#endif
#ifdef USE_RULES
#undef USE_RULES
#endif
#define USE_SML_SCRIPT_CMD
#define USE_SML_SPECOPT
#define SML_REPLACE_VARS
#define USE_TCP_BRIDGE

2. OTA your Slimmelezer+ with the .bin tasmota file
3. Connect you Wifi, if it wasn't in your config
4. Module configuration: Generic(18)
5. Tools, Edit Script: find in the script file.


Documentation for this meter: https://tasmota.github.io/docs/Smart-Meter-Interface/#sanxing-sx6x1-sxxu1x-ascii-obis

Hungarian documentation for SX631 (and other) meters (register list): https://www.eon.hu/content/dam/eon/eon-hungary/documents/Lakossagi/aram/muszaki-ugyek/p1_port%20felhaszn_interfesz_taj_%2020230210.pdf
