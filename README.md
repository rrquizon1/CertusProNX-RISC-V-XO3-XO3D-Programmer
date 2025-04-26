This is an I2C internal flash programming examples for MachXO2/MachXO3 and MachXO3D using Lattice Certus Pro NX.

The MachXO3 and MachXO2 devices feature internal flash memory that supports single boot, eliminating the need for an external flash and reducing component count. However, an external flash can still be used to store a golden image.For more information
please check FPGA-TN-02155 ([MachXO2 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=39085)) and FPGA-TN-02055 ([MachXO3 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=50123)).

MachXO3D have two sectors for internal flash programming namely CFG0 and CFG1. These sectors can be used for dual-boot instead of using an external flash for golden image. FPGA-TN-02069-1.8 ([MachXO3D Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=52591)) shows more information regarding MachXO3D dualboot and other features.

This example is very similar to the [STM32 I2C programming](https://github.com/rrquizon1/MachXO2-XO3-and-MachXO3D-I2C-internal-flash-Programming-using-STM32) I did before but now we are also using a Lattice Device to Program other Lattice Device!

The Lattice CertusPro™-NX is a high-performance, low-power FPGA family built on Lattice’s Nexus platform. Designed for compute- and connectivity-intensive applications, it offers advanced features typically found in larger FPGAs, while maintaining the power efficiency and small form factor that Lattice is known for.

In this example, we’ll use a RISC-V processor and configure I2C programming similarly to how it’s done on a microcontroller. This approach is designed to help microcontroller users transition smoothly to FPGA-based SoC development.

For this project we will be using three Lattice Tools!
* Lattice Propel Builder 2024.2
* Radiant Software 2024.2
* Lattice Propel 2024.2

Take note that Lattice Propel Builder and Lattice Propel are installed together via Lattice Propel installer package.

First off is we use Lattice Propel Builder to build our SoC. Lattice have multiple templates using RISCV core. See below for the SoC I built with Lattice Propel Builder
![image](https://github.com/user-attachments/assets/a5cdef92-d354-451b-a20e-35604b888d34)

Most templates have the following relevant parts:

* OSC and PLLs used as clock source for the CPU and its peripherals
* RISC V processor
* UART peripheral- This peripheral is used for printf debugging
* GPIO peripheral- For simple GPIO control

For this example, I added two additional peripherals

* I2C Master Peripheral- this perihperal will be used to program MachXO2/XO3/XO3D via I2C
* SPI Master- This peripheral is currently not used. We will be using it for future projects

After building the circuit, you have to validate then generate the design:
![image](https://github.com/user-attachments/assets/52127f50-ac47-47a7-98d5-da3a37df6ea9)

There should be no errors in the design but there could be some warning but it should be fine for this example:
![image](https://github.com/user-attachments/assets/f7327e37-0ab2-4274-afdd-275118f4e9a8)





  


  
