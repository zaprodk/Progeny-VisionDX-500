The VisionDX 500 is a Digital X-ray sensor unit from Progeny (Now MidMark).
Not much information is available online about this unit.

My unit is from 2007, but the firmware is dated 2006.

Internally the unit is comprised of a small Linux-computer in a 144-pin SD-DIMM format, uCdimm-MCF5272 Rev 1.3 - 4MB Flash, 16MB RAM from Arcturus Networks.
It's running Linux with a 2.4.17 Kernel.

The manual from Arcturus describes the memory layout. I have dumped:
Bootloader flash, OS Flash and dumped the RAM with the unit running.
Maybe there is a password hiding somewhere?

There's a Xilinx FPGA doing the heavy work, a Cirrus Logic WM8199 AFE (Analog FrontEnd) and a lot of power circuitry.

I've dumped the user firmware (OS_FLASH) from the module. It's a CRAMFS partition that you can mount with:
sudo mount -t cramfs -o loop OS_FLASH.bin /your/mountpoint

There is two serial ports on the module, brought out to headers J3 & J5.
Both ports are standard RS232 levels, not TTL.

Baudrate is 9600,8,N,1., can be switched into 115200 with "fast" command in the bootloader e.g. for faster dumping of firmware or flashing.
Pinout:
J5 ttyS0:
7	RXD1
8	RTS1
5	TXD1
6	CTS1

J3 ttyS1:
9	RTS2
10	TXD2
11	CTS2
12	RXD2
Note! Nothing happens on second serial port, so no reason to hook anything up and listen.
You don't need to hook up the handshaking lines.


How to change the IP/Gateway/Network settings:
I changed mine into this:
setenv HOST_IP 192.168.1.11
setenv GATEWAY 192.168.1.1
setenv HOST_ID VisionDX


There's a Telnet-server running on the device on port 23
User/password is unknown. The standard root/uClinux was tried, but didn't work.


Getting in, either trough the Serial port or Telnet could enable us to change parameters as:
the default Maximum Integration Time
the default Exposure Threshold
& the default CCD Configuration data

Can we recover the user/password from the firmware dump?
