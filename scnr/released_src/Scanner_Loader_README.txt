                HP48/HP49/HP50 Scanner Loader v1.0 (02/21/09)
                      (developed on HPUserEdit5)
                     ===============================
                        by: Pepin Torres, P.E.
                            Waltham, MA USA


Intro:
------

This UserRPL program implements a one-way serial interface to Radio Shack scanners
made by GRE that use the "Radio Shack Scanner Control Protocol". Scanners that
adhere to this protocol are the Pro-76, Pro-79, Pro-82, Pro-2016, Pro-2017 and 
Pro-2018. Other models/brands may also work. Source code is provided. 

You are free to use and modify this software to your liking. If distributing,
please make sure to properly credit the author.  While this program doesn't try
to do anything risky on the calculator, you are using this software at your own 
risk and the author is not liable for data loss or damage of any kind to your
calculator or scanner.

Rationale:
----------

While there are various options available to load scanner data using
a computer and a serial cable, updating or restoring frequencies means moving
the scanner from its usual listening spot to the PC which may sit elsewhere. By writing 
this software I wanted to use the portability of the HP calculator to update/restore 
frequency data at the scanner's location or while on the road where a PC may not be available.

Requirements:
-------------

o   Any HP48, HP49 or HP50 that has a serial port interface.
    Calculator must be set to connect via wire using ASCII at 4800 baud 
    with the "Xlat" field set to "Newl".  

o   A serial cable from HP calculator to male DB-9 connector (for HP50Gs, this cable is 
    available at http://commerce.hpcalc.org/serialcable.php for $20 + shipping)

o   A store-bought or home-made serial cable that goes from Female DB-9 to 1/8" stereo plug.
   Cable specs can be found here: http://www.glyff.net/software/prolink/datacable-gre1.php
   (see bottom of this document for cable specs.)

Please refer to the user's manual on how to set your scanner to accept data.

Installation:
-------------

Copy the provided files in the directory of your choice. 

SCNR, the main program, expects a matrix on the first level of the stack that has the following
format:

[ [ CHANNEL   FREQUENCY   DELAY   LOCKOUT   PRIORITY] 
  [ CHANNEL   FREQUENCY   DELAY   LOCKOUT   PRIORITY]
      :           :         :        :         :
  [ CHANNEL   FREQUENCY   DELAY   LOCKOUT   PRIORITY] ]

This matrix can be anywhere from 1 row by 5 columns up to 20 rows by 5 columns.

where:   CHANNEL is any number from 1 to 200.
	 Most Radio Shack scanners have 10 banks of 20 channels each for a total of 200.
	 Since my scanner (Pro-2018, Part No. 20-424) only suports this format, this is the only one
	 available in the software. I have provided 10 templates files (BANK0-BANK9).
	 (http://support.radioshack.com/support_tutorials/communications/20-424.htm)

	 FREQUENCY is any number of the form xxx.xxxx which corresponds to a frequency (in MHz)
	 available to the scanner. If you send invalid frequencies, the scanner will stop
	 the transfer and go back to normal operation. The software will zero-pad the frequency field
         accordingly to comply with the xxx.xxxx number format.

	 DELAY is either 0 or 1.
		0 means no 2 sec. delay happens before resumming scanning.
		1 means a 2 sec. delay happens before resumming scanning.

	 LOCKOUT is either 0 or 1.
		0 means use this frequency while scanning
		1 means do not use this frequencywhile scanning

	 PRIORITY is either 0 or 1.
		0 means this is not priority frequency
		1 means this is a priority frequency

REST, a companion application, loads all 10 bank files automatically to the scanner.
There are no input arguments. Note: This will overwrite all of your current frequencies
in the scanner.  Also, this process may take up to 10 minutes to finish on a HP48G.

For your convinience, it is recommended that you open the matrix writer on your calculator,
build the matrix with the format above and save it to a local variable for later editing.

It is suggested that you copy the 10 Frequency Bank template files provided (BNK0-BNK9).  
These templates consist of a 20x5 numerical matrix which are the input to the SCNR program. 
For scanners like the Pro-2018, the 200 frequency memory is broken up into 10 banks of 20 
frequencies each.  To customize these bank files, just put them on the stack and press the EDIT 
softkey (this should bring you to the matrix writer). You may modify the matrix as you wish, but 
remember that SCNR will only read the first 5 columns and the first 20 rows.


LISTING OF FILES IN PACKAGE:

SCNR   : Main loader program
REST   : Automatic restore of all frequency banks
BNK0   : Frequency bank 0, channels 1-20
BNK1   : Frequency bank 0, channels 21-40
BNK2   : Frequency bank 0, channels 41-60
BNK3   : Frequency bank 0, channels 61-80
BNK4   : Frequency bank 0, channels 81-100
BNK5   : Frequency bank 0, channels 101-121
BNK6   : Frequency bank 0, channels 121-140
BNK7   : Frequency bank 0, channels 141-160
BNK8   : Frequency bank 0, channels 161-180
BNK9   : Frequency bank 0, channels 181-200

(all banks come with frequency field equal to 140.000 MHz)

Troubleshooting:
----------------

Problem:			Cause/Solution:
Scanner does not accept data    Verify your connection speed on the HP calculator is 4800 ASCII with Newl checked. 
				Your homebrew scanner cable has the data lines lines wired incorrectly.
				(see excerpt from glyff.net below for cable specs)

Scanner give error message      You entered an incorrect channel number or an out-of-bounds frequency value
				for your scanner, or an unexpected checksum error or timeout occured.
				Please verify that the matrix has legal values, and try again.
				For checksum errors, try again. For timeouts, make sure you send data as soon
				as the scanner is ready to receive it. 


Excerpt from glyff.net:
-----------------------
Data Cable Information Radio Shack Pro-76, Pro-79, Pro-82, Pro-89, Pro-2016, Pro-2017, Pro-2018

The scanner end of the cable is a standard 3.5mm (1/8") Phone Connector. The computer end of the cable is a standard female DB9 connector. The sleeve (bottom section) of the phone connector is the ground and is connected to pin 5 (GND) of the DB9 connector. The ring (middle section) of the phone connector is receive data (RXD) for the scanner and is connected to pin 3 of the DB9 connector which is transfer data (TXD) for the computer. The tip (top section) of the phone connector is mono audio, for when headphones are connected, and is not connected to the computer end of the cable. 

Note:  These scanners can only listen to data and do not talk back.


(ptg 02/21/09)