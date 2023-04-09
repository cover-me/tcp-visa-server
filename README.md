# What is it?

This is a LabVIEW prototype of a Virtual Instrument Software Architecture (VISA) program based on the TCP/IP protocol. It behaviors as a virtual instrument which can be reached by VISA write/read functions.

# What is it used for?
 
 **Short answer**
 
It can be used as a wrapper program for instruments which do not support VISA write/read functions. 

**Long answer**

Using VISA write/read functions makes a data acquisition program, which takes data from and sets parameters in multiple instruments (lockin, multimeter, ...), simpler. Unfortunately, not all instruments follow this paradigm. Sometimes one has to work with DLL drivers. Sometimes one wants to read/set values on the GUI of a very specific third-party program. A VISA wrapping server could help without rewriting the elegant data-acquisition program. The data-acquisition program asks the tcp-visa server for readings and settings, and the latter takes care of the rest.

# Further reading

## A TCP-VISA sever for LabVIEW executables

If you plan to use the TCP-VISA server as a wrapper for getting (setting) readings (values) on LabVIEW executables or VIs provided by an instrument manufacturer, you may want to check this [example](https://github.com/cover-me/FP-monitor) out.

## A note on the terminator
Unlike GPIB which uses a real termination line (called EOL) for terminating communication, Eithernet uses termination characters which is a software implementation. As shown in the programming diagram below, we use the termination character "CRLF" in the server, which means "\r\n". 

To work together with [instrDAQ](https://github.com/cover-me/instrDAQ), we can either add the termination character ("\0D\0A") directly after reading and outputting commands (`RdCmd`, `OutCmd`, note that the INI file does not support strings in the form of "\r\n"), or use the `AdvPara` parameter (see https://github.com/cover-me/instrDAQ#add-a-model-that-echosrequire-a-terminatorbaud-rate-not-9600). Below is an example,

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

## Programming diagram and front panel

![image](https://user-images.githubusercontent.com/22870592/119033461-3e2a2f00-b97b-11eb-89b2-695ccd9798a9.png)

![image](https://user-images.githubusercontent.com/22870592/119033470-41251f80-b97b-11eb-83a2-7a93c4c24e6b.png)

## Snapshots from a real application

See [cover-me/repository/Leiden/TC_messenger_2021_02_01/](https://github.com/cover-me/repository/tree/master/Leiden/TC_messenger_2021_02_01). 

This program works as a mediator between the data acquisition program and Leiden's temperature controlling program (a LabVIEW executable).

![image](https://user-images.githubusercontent.com/22870592/119033538-526e2c00-b97b-11eb-81af-ce5ff5eb0dab.png)

![image](https://user-images.githubusercontent.com/22870592/119033546-539f5900-b97b-11eb-890a-c84c9cdb5875.png)

## Further reading on the data-taking program:

[A prototype for data taking programs](https://cover-me.github.io/2020/01/11/A-prototype-for-data-taking-programs.html)

[Data taking program: make it non-atomic](https://cover-me.github.io/2021/10/17/data-taking-program-make-it-non-atomic.html)


