## STM32Fxx Blue Pill

This project covers the basics of designing and implementing a Blue Pill project in KiCad. 
It will cover schematic, PCB layout and manufacturing process to fully understand developing and shipping of a 
schematic project.

## Sources

1. The source for this tutorial can be found in: [Video](https://www.youtube.com/watch?v=aVUqaB0IMh4&t=274s)
2. MicroController Documentation: [STM32 Page](https://www.st.com/en/microcontrollers-microprocessors/stm32f103t6.html#documentation)
3. Additionally, the github page associated with this project: [GitHub](https://github.com/Jzuni97/STM32-Blue-Pill)

## STM32 Project Structure

### Pinout & Configuration

The following pinout configurations were made at the start of the project. These configurations follow
the structure and applicability specified in the overall project description.

1. **System Core**
	- RCC: 
		-> HSE -> Crystal/Ceramic Resonator
		-> LSE -> Disabled
		-> Master Clock Output -> Unchecked
	- SYS: 
		-> Debug -> Serial Wire
		-> System Wake-Up -> Unchecked
		-> Timebase Source -> SysTick
2. **Connectivity**
	-USB:
		-> Device (FS) -> Checked
3. **MiddleWare**
	-USB_DEVICE:
		-> Class for FS IP -> *Note: Pre defined Drivers

### Clock Configuration

TBD

## Self-Notes

This section covers notes about the overall project.  
The notes include useful tips and design choices of the project. 

### BOOT0: 

One of the most important design choice is how will the MicroController (MCU) be programmed/interfaced with.
For this specific project, the interfacing choice for bootloading will be via USB connection, as oppposed to a JTAG or Serial Wide Debug (SWD). 
Consequently we enable the BOOT0 pin to HIGH since MCU supports USB interfacing. 

### Crystal Oscillator:

The 16MHz crystal was denoted from parts available in JLCPCB parts list as well as the maximum range for
MCU Input Frequency. For connectivity and configuration, refer to document: AN28867 Application Note of STM32 Docs, Section 3.1.
Figure 5 Shows the internal and external components required/set for this project. Since we've chosen a 4-pin package 16MHz Crystal, JLCPCB Part#: C112972, 
the specified capacitance is 10pF load capacitance. To calculate the external capacitors:
	Subtract Load Capacitance from Stray Capacitance (typically 2-5pF) and multiply by 2.  
	
	[Equation](http://www.sciweavers.org/tex2img.php?eq=%20C_%7BL%7D%20%3D%20%20%28C_%7BLp%7D%20-%20C_%7Bs%7D%29%20%2A%202&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0)

For this project, the calculated oscillator load capacitance is (10pF - 5pF) * 2 = 10pF.

### USB Interface

Since the USB programming interface was chosen for this project, it was important to develop a capable circuit 
for the MCU. Reading the USB Hardware and PCB Guidelines document, Table 3, it specifies the supported USB types
and important footnotes about the data connections. It was found that STM32F1 series MCU support only USB type B 
and additionally requires a 1k5 Ohm pull-up resistor in D+ line.

### Power Supply

Regulator: AMS1117 // 3.3V fixed output with up to 1A output.
Considerations
 - VBUS from USB are typically very noisy and most often (also good practice) require additional filtering.
In our case the filtering isn't required as the capacitor and regulator will provide decent enough filtering.
