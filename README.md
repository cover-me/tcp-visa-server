# tcp-visa-server
This is a prototype working as a wrapper program for instruments that don't support VISA write and read functions. 

Using VISA write and read functions would made a data acquisition program dealing with multiple instruments (lockin, multimeter, ...) simpler. However, not every data point can be fetched by VISA functions. Sometimes one has to work with .dll. Sometimes one wants to read/set on a program (yes...). In order not to recode the data acquisition program, a tcp visa server is used. The data acquisition program ask the tcp visa server for data or settings, and the tcp visa server take care of the rest.

If you want to use the tcp-visa-server as a wrapper to get/set readings/values on LabVIEW executables or VIs, you may want to consider using some sort of "path" string to referece the LabVIEW controls like [this](https://github.com/cover-me/FP-monitor).
