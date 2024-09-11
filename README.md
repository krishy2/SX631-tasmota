# SX631-tasmota
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
5. Tools, Edit Script:

>D

>B
smlj=0
;don't send teleperiod MQTT at boot, because we can have 0 values (meter didn't send data yet)
->sensor53 r
>R
smlj=0
;don't send teleperiod MQTT at script restart, because we can have 0 values (meter didn't send data yet)
>S
if upsecs>22
then
smlj|=1
endif
;only send teleperiod MQTT if 22 seconds passed since boot (during this time meter most probably sent data)
>M 1
+1,13,o,16,115200,SX631,1
1,1-0:32.7.0(@1,L1 Voltage,V,volts_l1,1
1,1-0:52.7.0(@1,L2 Voltage,V,volts_l2,1
1,1-0:72.7.0(@1,L3 Voltage,V,volts_l3,1
1,1-0:31.7.0(@1,L1 Current,A,curr_l1,1
1,1-0:51.7.0(@1,L2 Current,A,curr_l2,1
1,1-0:71.7.0(@1,L3 Current,A,curr_l3,1
1,1-0:14.7.0(@1,Frequency,Hz,freq,2
1,0-0:96.14.0(@1,Current tariff,,tariff,0
1,=h<hr/>
1,1-0:1.8.0(@1,Energy import,kWh,enrg_imp,3
1,1-0:2.8.0(@1,Energy export,kWh,enrg_exp,3
1,=h<hr/>
1,1-0:1.8.1(@1,Energy import T1,kWh,enrg_imp_t1,3
1,1-0:1.8.2(@1,Energy import T2,kWh,enrg_imp_t2,3
1,1-0:2.8.1(@1,Energy export T1,kWh,enrg_exp_t1,3
1,1-0:2.8.2(@1,Energy export T2,kWh,enrg_exp_t2,3
1,1-0:1.7.0(@1,Power import,kW,pwr_imp,3
1,1-0:2.7.0(@1,Power export,kW,pwr_exp,3
1,1-0:13.7.0(@1,Power factor,,factor,3
1,=h<hr/>
1,1-0:3.8.0(@1,Reactive nrg import,kvarh,nrg_reac_imp,3
1,1-0:4.8.0(@1,Reactive nrg export,kvarh,nrg_reac_exp,3
1,1-0:5.8.0(@1,Reactive energy QI,kvarh,nrg_reac_q1,3
1,1-0:6.8.0(@1,Reactive energy QII,kvarh,nrg_reac_q2,3
1,1-0:7.8.0(@1,Reactive energy QIII,kvarh,nrg_reac_q3,3
1,1-0:8.8.0(@1,Reactive energy QIV,kvarh,nrg_reac_q4,3
1,1-0:5.7.0(@1,Reactive power QI,kvar,pwr_reac_q1,3
1,1-0:6.7.0(@1,Reactive power QII,kvar,pwr_reac_q2,3
1,1-0:7.7.0(@1,Reactive power QIII,kvar,pwr_reac_q3,3
1,1-0:8.7.0(@1,Reactive power QIV,kvar,pwr_reac_q4,3
#

Documentation for this meter: https://tasmota.github.io/docs/Smart-Meter-Interface/#sanxing-sx6x1-sxxu1x-ascii-obis

Hungarian documentation for SX631 (and other) meters (register list): https://www.eon.hu/content/dam/eon/eon-hungary/documents/Lakossagi/aram/muszaki-ugyek/p1_port%20felhaszn_interfesz_taj_%2020230210.pdf
