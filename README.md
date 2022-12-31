# UT23 Front Control Module v.1.1

By: Benjamin Liang, Controller Design Lead, University of Toronto Formula Racing Team

In this article, I will present the Front Control Module v.1.1 for UT23, our car for next year’s competition season. Each year, we, the University of Toronto Formula Racing Team (UTFR) design and build a new formula-style car from scratch to compete in international Formula SAE events. The drivers are the starts of all formula racing competition and rightfully so, a good driver can sometimes make the difference between a back marker and a mid field team, or a mid field team and being at the front of the grid. As such, the interface with the driver is critical in order to maximize performance, but more importantly safety. The Front Control Module is responsible for all the critical electrical functions around our front bulkhead, many of which are directly related to the safety of our driver and car. This PCB is designed to control our dash LEDs and LCD, interface with a wide variety of critical and noncritical sensors and communicate with the rest of our electrical system over CAN bus.

![image](https://user-images.githubusercontent.com/82067858/210156002-0ee56d6a-fde7-4e54-850d-f8c3f104a2fe.png)
 
The Front Control Module includes different components such as a Teensy 4.1, three CAN transceivers, two 5V linear regulators, a 3V3 linear regulator and a series of op amp and Mosfet circuits. In this article, I will go over how some of these components are used to interface with our driver, specifically the accelerator pedal position sensor (APPS) and the dash.

## Schematic Design

Specifically, the Front Control Module revolves around a Teensy 4.1 as the MCU. The Teensy 4.1 is responsible for all the previously mentioned functions of this controller. As it relates to the dash, the Teensy is responsible for interfacing with the dash LCD, controlling the ready to drive (RTD) speaker, and control a series of LEDs used to indicate IMD (insulation monitoring device) faults, AMS (accumulator management system) faults and the TS (tractive system) cockpit indicator. The insulation monitoring device throws a fault whenever the measured impedance between the TS (tractive system) and the LVS (low voltage system) is below a minimum acceptable threshold of 250kohms. Possible causes for an AMS fault to occur include battery over temp, welded relay on the high current path, loss of communication with the BMS (battery management system), etc. The TS cockpit indicator lights up green whenever the vehicle side of the high current path is less than 60V and all the TS relays that activate the high current path are open. The RTD speaker sounds when the TS and inverter are fully activated and the motor is capable of delivering torque.

![image](https://user-images.githubusercontent.com/82067858/210156005-d9320707-2465-4def-a347-71048b5216cb.png)
 
Shown above are three Mosfet LED LSD (low side drive) circuits and a Mosfet RTD speaker LSD circuit which are controlled by the Teensy 4.1. Each circuit includes indicator LEDs on the board that are actuated at the same time as the controlled endpoint (LED or speaker). The Mosfets include a current limiting resistor and a discharge resistor to account for the gate capacitance. 
 
![image](https://user-images.githubusercontent.com/82067858/210156007-7c4adaba-f859-442a-bd3f-a78bfe68b0d1.png)
https://www.pjrc.com/store/display_ili9341_touch.html

The Teensy 4.1 also interfaces with the dash LCD screen shown above. The Teensy communicates with the dash LCD via SPI communication and some additional digital and power connections. The LCD screen allows us to present customizable information to our driver to maximize the safety of our system beyond the minimum requirements of the formula student rules.

![image](https://user-images.githubusercontent.com/82067858/210156010-d04f14e9-0d68-4aee-93ff-bde3fbf3e526.png)
 
The safety critical information is communicated to the Front Control Module via CAN bus. CAN bus is a robust communication protocol developed by Bosch which has become industry standard in the automotive industry and many other industries for its ability to resist and diagnose failures that occur. See our Accumulator Control Module article for more information about CAN bus as it relates to our electrical system: https://github.com/UTFR/UT23-Accumulator-Control-Module-v.1.2. Shown below is the UT23 CAN architecture diagram.

![image](https://user-images.githubusercontent.com/82067858/210156016-07d12071-6ed7-4ffc-8136-203345e28255.png)
 
Another drive and safety critical signal on our car that is communicated over CAN bus is the accelerator pedal position sensor (APPS) signals. We use Each EV FSAE and FSG car is required to have at least two APPS installed with two distinct non-intersecting transfer functions. The APPS signals are read on the Front Control Module and a plausibility check is done to ensure that both signals agree. A plausibility bit will be incorporated in the CAN message forwarding the APPS signal to the Rear Controller where a second plausibility check will be done with the signals and the plausibility bit to ensure that the data was not corrupted during communication. This is all done on top of the CRC (cyclic redundancy check) that is standard in the CAN communication protocol.
PCB Layout

![image](https://user-images.githubusercontent.com/82067858/210156017-7909f6c7-4115-465f-8a65-d9893aa4a744.png)

The Front Control Module is a four-layer board designed using Altium Designer as shown in the figure below. The four layers include a high-frequency digital signal and power top layer, a power and ground second layer, a grounded third layer, and an analog and low-frequency signal bottom layer. Differential signals were routed near each other, and other high-frequency signals were routed in a manner that considered adequate spacing and reducing trace length. A substantial effort was placed in making a user-friendly silkscreen to ensure the correct and safe fabrication and operation of the board.
 
## JLCPCB

This PCB is a key component of a rapid and thorough design cycle aimed at maximizing driver safety. The technical driver safety development within our team was only possible thanks to the help of JLCPCB (https://jlcpcb.com/RAT). Founded in 2006, JLCPCB (https://jlcpcb.com/RAT) is a leading global PCB manufacturer with over 16 years of experience providing the best customer experience through the rapid production of highly reliable and cost-effective PCBs.

![image](https://user-images.githubusercontent.com/82067858/210156022-70873878-5b80-4122-a691-4b0cda5d814c.png)
 
The Front Control Module was ordered from JLCPCB (https://jlcpcb.com/RAT) for quick, reliable, and cost-effective manufacturing. Ordering PCBs from JLCPCB (https://jlcpcb.com/RAT) is extremely simple and consists of the following four quick steps:

1.	Export Gerber files from Altium Designer
2.	Click on the instant quote button on the JLCPCB website (https://jlcpcb.com/RAT)
3.	Add your Gerber files and double-check the uploaded PCB in the Gerber viewer
4.	JLCPCB automatically detects all the general PCB specifications and fills out the order form for you. Review and modify these to ensure they match your PCB requirements.
5.	Click save to cart, fill out your address, select your shipping method, and pay

Furthermore, JLCPCB (https://jlcpcb.com/RAT) also provides many other manufacturing services including an SMT service which fulfills the money and time saving needs of customers at only an $8.00 setup fee ($0.0017 per joint). It is a one stop online platform where you can also track the progress of your PCBA manufacturing process, which is completed within a day, in real time. Through the platform you can conveniently source thousands of components from JLCPCB (https://jlcpcb.com/RAT) and its reliable component partners like Digikey and Mouser. The quality and reliability are always superb with professionally trained engineers, X-ray inspection, automated optical inspection, automated component placement, and various soldering techniques. 
