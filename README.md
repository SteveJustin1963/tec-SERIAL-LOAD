# tec-SERIAL-LOAD
TE-16, pg 24
- https://github.com/SteveJustin1963/tec-BOOKS/blob/master/TE/Mag/tec_times_1990_03.pdf

### SERIAL INPUT ROUTINE - RECEIVER
This is the routine I use when I wish to down-load a file from the IBM. It's a simple routine that converts a serial stream into bytes and stores them in RAM starting at the address provided at 0898. The routine also has an end address to allow a maximum file length. This is in case something goes wrong with the data transfer. Anything important can be protected by placing it above the end address.

No hand-shaking is needed as the TEC can cope with the speed of the data stream. It is up to you to ensure the TEC is ready before you send the data. The serial input is bit 0 of PORT 3. The DAT BOARD has provision for 2 diodes and a resistor at this input to clip an incoming RS232 signal. In the RS232 format, a logic 1 is represented by a negative voltage while a logic 0 is a positive voltage. The clipper on the DAT BOARD changes an RS232 logic 0 (positive voltage) into a digital logic 1 while an RS232 logic l is clipped to zero volts and becomes a digital logic 0.

This means that the inputted data must be inverted back into its true form. This is done with the CPL instruction at 092C. The format of the data is as follows: 2400 BAUD, NO PARITY, 8 BITS, STOP BITS OPTIONAL, TEC SPEED: 3.58/2

ser_in.z80; This program reads data from a serial port and stores it in memory. The program starts by reading a byte from the serial port. It then rotates the byte left by one bit and stores it in memory. If the byte is not equal to zero, the program reads another byte from the serial port and repeats the process. When the byte is equal to zero, the program stops reading from the serial port and increments the value in the memory location pointed to by HL.

ser_out.z80;

### SERIAL OUTPUT ROUTINE - SENDER
This is the complement routine of the serial receiver. It will send serial data through the TEC speaker bit. The data is taken from the latch side of the base resistor of the transistor inverter and inputted directly to an RS232 Rx input or the DAT BOARD serial input. Strictly speaking the data stream is not RS232 compatible but in practice it works ok, although the occasional error may creep in.

Oh yes, before sending data, the key press beep must be turned off. To do this, place FF at 0822 and put AA at 08FF. The serial sender uses the same start and end buffers as the receive described above with the same speed etc. Two stop bits are sent as this provides compatibility with all serial systems. 

### IBM SOFTWARE
The software I used for receiving the serial is PROCOMM. It is a public domain program and can be purchased from the Talking Electronics Shop. Cat S-449. The protocol to use is ASCII. The sending software poses a few difficulties. One big problem is that some packages won't send the IA character. Actually, I believe the problem is in the DOS serial interrupt and if the software uses it then it won't send the IA character. It is rare that I send anything back to the TEC and when I do, it's with a serial routine Craig wrote and probably won't work with all computers as it directly manipulates the hardware; not a recommended practice. It is up to you to experiment around and find something that works. I would like to hear from anyone who has found or written a good sending routine that doesn't have the IA character problem. Hardware wise, the CTS must be taken high before the IBM will send the data. This means that the IBM to TEC link consists of three wires: the ground, the serial data line and +5v. Only ground and the serial data are required for the TEC to IBM link. 

