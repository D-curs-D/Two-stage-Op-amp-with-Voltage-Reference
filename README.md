# Two stage Operational Amplifier with Voltage Reference Circuit
_The project is focused on the creation of an Operational Amplifier with it's own Voltage reference circuit to bias it. The design is based on the 28nm iPDK from SYNOPSYS and hence, is done on it's custom design suite._<br>
![main-image]()

### Table of Contents
- [Introduction and  Abstract](#Introduction)
- [Testing the PDK](#PDK-Test)
  - [NMOS](#NMOS)
  - [PMOS](#PMOS)
  - [Determining Sizes](#Conclusion-and-Sizing)
  - [Inverter Design](#Inverter) 

### Introduction
__Operational amplifier (Op-Amp)__ circuits are _the most important and essential building blocks in the design of different high precision analog and mixed signal blocks_, where the performance is directly dependent to how good your op-amp is. These op-amp circuits have substituted the conventional solid state analog control systems used in industrial applications. With the advent of newer and shrinking device technologies these circuits have become popular and are almost used in implementation of various analog systems, analog to digital converter circuits, digital to analog converter circuits, analog instrumentation design, analog computation and performing the tasks such as higher order active filtering of signals, amplification of signal voltages, signal transduction and ultra high speed conversion of signals. General purpose op-amp circuits are used to realize circuits such as differentiators, high speed comparator circuits, clippers, clampers, antilog and log amplifier circuits, integrators, various waveform generation, addition of analog input signals, buffering of signals, sample and hold circuits, negative impedance converters, differential amplifiers, inverting and non-inverting amplifiers regulated power supplies and many other applications.

For Biasing an Op-amp, we need to create a constant refence current that is capable of withstanding the variations in the supply(practically also temperature). That is why the design also includes a __Beta Multiplier Current Refernce Circuit(BMCR)__, that produces the bias voltages necessary.

The core reason to why this projects stands as the way it is, is for learning purpose and also to develop designs capable of being used in an actual chip to further better my understanding of how individual blocks work in an analog circuit. Operational Amplifiers are ubiquitous, the major building blocks as mentioned earlier. Hence, are also a great tool in understanding circuit design up close. 

### PDK-Test
The design was done on __Synopsys Custom Design Suite__ using it's 28nm technology node as a part of it's iPDK(Interoperable Process Design Kit). More information about the pdk(process design kit) can be found [here](https://news.synopsys.com/index.php?s=20295&item=123069). This is a 28nm node, so the minimum channel length is __28nm__, but for analof design a lot of things change, especially in sub-micron processes where the second order effects are prevalent, and makes it difficult to obtain near ideal statictics from our design. So to tackle them, some suggests to use about 2-5 times the minimum length defined. A lot of other such techniques exists as well. 

So, let's start with the most crucial part of our design, the __Aspect Ratio(S)__ of the devices used. This is done based off our requirements of the current and also the swing expected. The first steps is hence, to see how our mosfets behave, that is how the models work. To do this, we characterise them individually.

#### NMOS
The __N-Channel Metal Oxide-Semiconductor Field Effect Transistors__ or __NMOS__ for short, are a type of MOS transistors where an N type channel is created for current conduction when we apply a potential at the gate of the transistor. To characterise a MOS transistor, we need to find a couple values and also deterine a couple constants that we can use further to design rest of the circuits.

For starters, we take a length and we stick to it for the rest of our estimations. ___I would be using L as 5 * Lmin.___
