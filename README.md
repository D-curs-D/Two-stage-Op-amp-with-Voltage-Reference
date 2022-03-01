# Two stage Operational Amplifier with Voltage Reference Circuit
_The project is focused on the creation of an Operational Amplifier with it's own Voltage reference circuit to bias it. The design is based on the 28nm iPDK from SYNOPSYS and hence, is done on it's custom design suite._<br>
![main-image](/images/Cover.PNG)

### Table of Contents
- [Introduction and  Abstract](#Introduction)
- [Testing the PDK](#PDK-Test)
  - [NMOS](#NMOS)
  - [PMOS](#PMOS)
  - [Determining Sizes](#Conclusion-and-Sizing)
  - [Inverter Design](#Inverter)
- [Main Design](#Main-Design)
  - [Voltage Refernce Circuit](#Voltage-Reference-Circuit)
  - [2 Stage OpAMP](#2-Stage-OpAMP)
- [Final Say](#Conclusions)
- [References](#References)

### Introduction
__Operational amplifier (Op-Amp)__ circuits are _the most important and essential building blocks in the design of different high precision analog and mixed signal blocks_, where the performance is directly dependent to how good your op-amp is. These op-amp circuits have substituted the conventional solid state analog control systems used in industrial applications. With the advent of newer and shrinking device technologies these circuits have become popular and are almost used in implementation of various analog systems, analog to digital converter circuits, digital to analog converter circuits, analog instrumentation design, analog computation and performing the tasks such as higher order active filtering of signals, amplification of signal voltages, signal transduction and ultra high speed conversion of signals. General purpose op-amp circuits are used to realize circuits such as differentiators, high speed comparator circuits, clippers, clampers, antilog and log amplifier circuits, integrators, various waveform generation, addition of analog input signals, buffering of signals, sample and hold circuits, negative impedance converters, differential amplifiers, inverting and non-inverting amplifiers regulated power supplies and many other applications.

For Biasing an Op-amp, we need to create a constant refence current that is capable of withstanding the variations in the supply(practically also temperature). That is why the design also includes a __Beta Multiplier Current Refernce Circuit(BMCR)__, that produces the bias voltages necessary.

The core reason to why this projects stands as the way it is, is for learning purpose and also to develop designs capable of being used in an actual chip to further better my understanding of how individual blocks work in an analog circuit. Operational Amplifiers are ubiquitous, the major building blocks as mentioned earlier. Hence, are also a great tool in understanding circuit design up close. 

### PDK-Test
The design was done on __Synopsys Custom Design Suite__ using it's 28nm technology node as a part of it's iPDK(Interoperable Process Design Kit). More information about the pdk(process design kit) can be found [here](https://news.synopsys.com/index.php?s=20295&item=123069). This is a 28nm node, so the minimum channel length is __28nm__, but for analof design a lot of things change, especially in sub-micron processes where the second order effects are prevalent, and makes it difficult to obtain near ideal statictics from our design. So to tackle them, some suggests to use about 2-5 times the minimum length defined. A lot of other such techniques exists as well. 

So, let's start with the most crucial part of our design, the __Aspect Ratio(S)__ of the devices used. This is done based off our requirements of the current and also the swing expected. The first steps is hence, to see how our mosfets behave, that is how the models work. To do this, we characterise them individually.

#### NMOS
The __N-Channel Metal Oxide-Semiconductor Field Effect Transistors__ or __NMOS__ for short, are a type of MOS transistors where an N type channel is created for current conduction when we apply a potential at the gate of the transistor. To characterise a MOS transistor, we need to find a couple values and also deterine a couple constants that we can use further to design rest of the circuits. The testbench is shown below:<br>
![nmos char](/images/nmoschar.PNG)<br>

For starters, we take a length and we stick to it for the rest of our estimations. ___I would be using L as 5 * Lmin.___. Below is the Ids vs Vgs curve for the NMOS device.<br>
![nmos ids vs vgs](/images/nmos_idsvsvgs.PNG)<br>

Using this curve we can assume the __Vth__ to be closer to __0.5 Volts__. This would be a lot useful when we scale up, that is move to actual calculations.<br><br> Next we find the Vds vs Ids, this would help us determine the current to move forward with.
![image](/images/nmos_idsvsvds.PNG)
<br>Above results gives us conclusive values of currents that can be chosen for __NMOS__


#### PMOS
Similarly, we did the same experiments for __P-Channel Metal Oxide-Semiconductor Field Effect Transistor__ or __PMOS__ for short. The results we obtained and the tests are presented below.
<br>
![image](/images/pmos_isdvsvsg.PNG)
<br>
![image](/images/pmos_isdvsvsd.PNG)


#### Conclusion and Sizing
After the above tests, we decided to take the length as __2 * Lmin__ (~60nm) and width as __ 50 * Wmin __ for NMOS and For PMOS the length was the same but we got the width to be double of that.

#### Inverter
We also test run a CMOS Inverter for our design. It yeilded out some standard results that we expect off an inverter, this was done to ensure that our device works as expected and also to add nother test design to our suite.<br>
![image](/images/Inverterdc.PNG)
<br><br>
![image](/images/InverterTran.PNG)
<br><br>
![image](/images/Inverterdcsim.PNG)
<br><br>


### Main Design
Now finally it's time to start with our main design, this would include two crucial components:
- Voltage Refernce Circuit
- Operational Amplifier(2-Stage)

Both of these are designed and analysed in the Synposys tools. 

#### Voltage Refernce Circuit
Voltage Reference Circuit is used to produce stable voltage that would allow a constant current through our transistors and Bias them to a region and would not change it with changes in supply voltage. The circuit used is given below,
<br>![image](/images/BMCR.PNG)
<br><br>Following were the results obtained from it<br>
![images](/images/BMCR_Waveform.PNG)
<br><br>this was further used to produce a couple more voltages. Whose circuit is given below<br><br>
![asdf](/images/Vref.PNG)

THis block would be used in the next stage where we designed an Opamp. 

#### 2 Stage OpAMP
Below is the circuit diagram of a standard two stage Operational Amplifier with a compensation capacitor<br><br>
![image](/images/opamp.PNG)

This wass simulated with a 1V ac input at one input end, which corresponds to 0dB on a 20dB logarithmic scale. The below result is what we obtained
<br>![images](/images/opamp_result.PNG)

### Conclusions
As you can see, the design was not able to achieve a large gain, even though the design was done considering a 60dB gain. The reasons being:
- Mismatch of component sizes
- Improper calculation, since an actual parameter was difficult to obtain. 
- Design consideration, even though it was done for a small channel process.

But what is amazing is being able to calculate and also display results for a device. The opportunity to use synopsys tools and also experiment with them was a beautiful opportunity. I am grateful for the whole team that gave me this chance to experiment and showcase my design at this platform. 

Special thanks to IIT Hyderabad, Synopsys India and VSD. 

### References
The main reference were books like CMOS Analog Circuit Design by Allen & Holdberg and also by Razavi. Then there were also a couple papers involved whose links I would be attaching below:
- [An improved 2 stage opamp with rail-to-rai! gain-boosted folded cascode input stage](https://ieeexplore.ieee.org/document/8292126)
- [Design of low noise low power two stage CMOS operational amplifier](https://ieeexplore.ieee.org/abstract/document/6963068)
- [Design and implementation of operational amplifiers with programmable characteristics in a 90nm CMOS process](https://ieeexplore.ieee.org/document/5274950)

___Thank you___
