# Introduction
This is an I2C internal flash programming examples for MachXO2/MachXO3 and MachXO3D using Lattice Certus Pro NX.

The MachXO3 and MachXO2 devices feature internal flash memory that supports single boot, eliminating the need for an external flash and reducing component count. However, an external flash can still be used to store a golden image.For more information
please check FPGA-TN-02155 ([MachXO2 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=39085)) and FPGA-TN-02055 ([MachXO3 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=50123)).

MachXO3D have two sectors for internal flash programming namely CFG0 and CFG1. These sectors can be used for dual-boot instead of using an external flash for golden image. FPGA-TN-02069-1.8 ([MachXO3D Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=52591)) shows more information regarding MachXO3D dualboot and other features.

This example is very similar to the [STM32 I2C programming](https://github.com/rrquizon1/MachXO2-XO3-and-MachXO3D-I2C-internal-flash-Programming-using-STM32) I did before but now we are also using a Lattice Device to Program other Lattice Device!

The Lattice CertusPro™-NX is a high-performance, low-power FPGA family built on Lattice’s Nexus platform. Designed for compute- and connectivity-intensive applications, it offers advanced features typically found in larger FPGAs, while maintaining the power efficiency and small form factor that Lattice is known for.

# Main Project creation
In this example, we’ll use a RISC-V processor and configure I2C programming similarly to how it’s done on a microcontroller. This approach is designed to help microcontroller users transition smoothly to FPGA-based SoC development.

We will be using three Lattice Boards for this example:
* CertusPro-NX Evaluation Board- Main RISC V Processor
* MachXO3 Starter Kit- I2C Slave to be programmed
* MachXO3D Breakout board- I2C Slave to be programmed

![image](https://github.com/user-attachments/assets/77c7a1b3-7803-42f9-9e72-4f8437a97020)

For this project we will be using Four Lattice Tools
* Lattice Propel Builder 2024.2
* Lattice Radiant Software 2024.2
* Lattice Propel 2024.2
* Lattice Radiant Programmer 2024.2

Take note that Lattice Propel Builder and Lattice Propel are installed together via Lattice Propel installer package.

First off is we use Lattice Propel Builder to build our SoC. Lattice have multiple templates using RISCV core. See below for the SoC I built with Lattice Propel Builder
![image](https://github.com/user-attachments/assets/a5cdef92-d354-451b-a20e-35604b888d34)

Most templates have the following relevant parts:

* OSC and PLLs used as clock source for the CPU and its peripherals
* RISC V processor
* UART peripheral- This peripheral is used for printf debugging
* GPIO peripheral- For simple GPIO control
* System Memory

For this example, I added two additional peripherals

* I2C Master Peripheral- this perihperal will be used to program MachXO2/XO3/XO3D via I2C
* SPI Master- This peripheral is currently not used. We will be using it for future projects

After building the circuit, you have to validate then generate the design:
![image](https://github.com/user-attachments/assets/52127f50-ac47-47a7-98d5-da3a37df6ea9)

There should be no errors in the design but there could be some warning but it should be fine for this example:
![image](https://github.com/user-attachments/assets/f7327e37-0ab2-4274-afdd-275118f4e9a8)

After generating your design, it's now time to open Lattice Radiant Software and Lattice Propel!
![image](https://github.com/user-attachments/assets/b45e44bb-5baa-46cf-86fc-376ae445add4)


In Lattice Radiant, you generate the bitstream for the CertusPro-NX FPGA, configuring it to function as a processor. Complementing this, Lattice Propel serves as your integrated development environment (IDE) for developing embedded firmware, similar to how STM32CubeIDE is used for STM32 microcontrollers.

Once you opened Radiant, the Radiant Project for the SoC will be generated. This will look just like a normal Radiant PRoject. You could can set the pins in the project and then run the whole flow:
 
![image](https://github.com/user-attachments/assets/e587cbcc-8f9c-422b-ae73-6f74d4033999)

Once you run the whole flow, you should now have a bitstream generated in your project, you should program it using Radiant Programmer.

After programming this to the CertusProNX Evaluation board, your device will now work as a RISCV controller:
![image](https://github.com/user-attachments/assets/01475db1-38fe-4bd7-af26-968b03b6b0b8)

Now Let's go to Lattice Propel, I already have the workspace attached in this Repo, you could just load it right after you program the bitstream:
![image](https://github.com/user-attachments/assets/b93a1078-ff32-4bb2-bfb5-79bcb94503ef)

Notice that it looks very similar to the STM32 IDE! You should fit right in on how to use it. As from here on, you could already use your microcontroller knowledge on implementing your project. Take note that when you generate the workspace only UART peripheral is properly initialized. In these peripheral I used GPIO and I2C so I had to initialize them. 

In this workspace I already had them initialized. If you want to learn more about this drivers you can go to src=>bsp. All the drivers of the peripherals used should be here. 

Aside from the auto generated .c and .h file I added some .c and .h files that contains the functions used for programmign the MachXO2 and MachXO3D devices:

i2c_program.c,i2c_program.h: Contains the functions used for programming MachXO2/MachXO3/MachXO3D

data.c,data.h: Contains the bitstream data

# Sample Debug Console Printing
You could use the Debug view of Lattice Propel to readout the console printing:

![image](https://github.com/user-attachments/assets/8285511e-13e8-4c00-8239-c862be1a70b1)
![image](https://github.com/user-attachments/assets/5e486bca-97bf-4075-991d-f3a86dbec794)


# Sample hardware set-up
![image](https://github.com/user-attachments/assets/42b1efa3-fd18-4aa7-8c3f-36a0fd4beba5)

![image](https://github.com/user-attachments/assets/92038356-c75d-4486-b21c-905e7a018a77)

# Some notes on this project:

* In this project, the bitstream data are stored in the system memory. This means you only have limited bitstream to store.
* In case, you need larger space using an external memory will be helpful. 







  


  
