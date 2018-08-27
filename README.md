# tcp-visa-server
This is a prototype working as a wrapper program for instruments that don't support VISA write and read functions. 

Using VISA write and read functions would made a data acquisition program dealing with multiple instruments (lockin, multimeter, ...) simpler. However, not every data point can be fetched by VISA functions. Sometimes one has to work with .dll. Sometimes one wants to read from a program. If that happens, why not try a tcp visa server?
