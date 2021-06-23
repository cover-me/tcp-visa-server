# tcp-visa-server
This is a prototype working as a wrapper program for instruments that don't support VISA write/read functions. 

Using VISA functions makes a data acquisition program that deals with multiple instruments (lockin, multimeter, ...) simpler. However, not every datapoint can be fetched by these functions. Sometimes people have to work with .dll. Sometimes people want to read/set values on the interface of a third-party program (yes...). In order not to rewrite the data acquisition program, a tcp visa server is needed. The data acquisition program asks the tcp visa server for data or settings, and the tcp visa server take care of the rest.

If you want to use the tcp-visa-server as a wrapper to get/set readings/values on LabVIEW executables or VIs, you may want to use some sort of path-like string to referece LabVIEW controls like [this](https://github.com/cover-me/FP-monitor).

# Note on the terminator
Unlike GPIB, which usually uses an physical line called EOL to terminate the communication, Eithernet use the termination character for termination. As shown in the snapshot below, we set the termination character to "CRLF", which means "\r\n". Note that INI files do not support strings like "\r\n". we have to use "\0D\0A" instead.

We can add the termination charactor ("\0D\0A") directly after read and output commands (RdCmd, OutCmd), or using the `AdvPara` (see https://github.com/cover-me/instrDAQ#add-a-model-that-echosrequire-a-terminatorbaud-rate-not-9600) 

```ini
; This is the comment line, ppms is a virtual instrument emulated by tcp VISA server
[ppms]
RdName=T(K)/B(kG)
RdCmd=READT\0D\0A/READB\0D\0A
SwpAvl=TRUE
OutName="T(K): Set T(K),rate(K per min)/B(kG): Set B(kG),rate(T per min)"
OutCMD="SETT #,##\0D\0A/SETB #,##\0D\0A"
AdvPara=,,/FALSE
```



# Snapshots
![image](https://user-images.githubusercontent.com/22870592/119033461-3e2a2f00-b97b-11eb-89b2-695ccd9798a9.png)

![image](https://user-images.githubusercontent.com/22870592/119033470-41251f80-b97b-11eb-83a2-7a93c4c24e6b.png)

# Snapshots from a real application

See [cover-me/repository/Leiden/TC_messenger_2021_02_01/](https://github.com/cover-me/repository/tree/master/Leiden/TC_messenger_2021_02_01). 

This program works as a mediator between the data acquisition program and Leiden's temperature controlling program.

![image](https://user-images.githubusercontent.com/22870592/119033538-526e2c00-b97b-11eb-81af-ce5ff5eb0dab.png)

![image](https://user-images.githubusercontent.com/22870592/119033546-539f5900-b97b-11eb-890a-c84c9cdb5875.png)
