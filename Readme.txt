/*==============================================================================
== FILENAME :
==		FAT file system for Atmega 32 
================================================================================
==
== FILE NAME :
==    Readme.txt
==
== Description :
==    This package encapsulates chaN's tiny Fat Filesystem. I found that one has to spend considerable amount of time in getting  the 
	Fat file system to work on Atmega32 due to the microcontroller's low memory. Hence, I prepared this package which makes life very 
	simple for the user.
==
== DATE         NAME              
== ----         ----             
== 13Jun2009	Sweta Yadav    
==============================================================================*/

NPL adaptation of chaN's  Fat Filesystem for atmega controllers

Here are the steps that were followed:

1) In main.c, the buffer size was reduced from 2048 bytes to 128 bytes to improve memory efficiency. RTC.h was replaced with STIME.h, 
and TIMER2_COMP_vect (1ms) was implemented for asynchronous timer operations, along with TIMER0_COMP_vect (10ms) for timer variable 
increment and disk status checks. Port configurations were adjusted for the ATmega32, removing elements specific to ATmega128, while 
enabling SPI functionality and adding the ASSR register for asynchronous timer operation. The f_mkfs function was integrated, allowing
 the filesystem to be formatted directly from the board, eliminating the need for a PC. Additionally, the linebuff size was reduced 
 from 120 bytes to 16 bytes, unnecessary variables from scan_files were removed, and put_dump was deleted along with cases related to 
 disk status display, data dumping, and file status checks. Low-level disk operations and certain file management functionalities, 
 such as truncation, renaming, directory creation, attribute changes, file copying, and timestamping, were also removed. Finally, all
  RTC-related functions and declarations were commented out for potential future revisions.  

2) In mmc.c, modifications were made to the FatFs Tiny FAT file system module to better suit the ATmega32 microcontroller. Macros were 
defined for chip select using PB4, as well as for detecting card insertion (PB1) and the write protection status (PB0) of the inserted 
card. The socket power switch was removed from the POWER ON and POWER OFF functions to simplify power management. Additionally, SPI port 
and PORTB initialization configurations were adjusted to align with the ATmega32 setup. Finally, the check power function was modified 
to always return 1, ensuring that the system recognizes the socket as powered when a card is inserted.  

3) In tffc.c, several modifications were made to optimize filesystem operations. The make filesystem function was defined while unnecessary 
elements, such as the check for multiple logical drives (_DRIVES) and the LD2PD function (which is only required for multipartition drives), 
were removed. Additionally, several file management functions that were not required were removed, including f_stat (file status check), 
f_truncate (file truncation), f_mkdir (directory creation), f_chmod (file attribute modification), f_utime (file timestamp update), and 
f_rename (file or directory renaming). These changes simplify the filesystem, making it more lightweight and better suited for the intended use case.  

4) In uart.c, the system clock and baud rate settings were modified to optimize communication performance. Additionally, the UART registers 
were reconfigured to align with the ATmega32 microcontroller, specifically replacing UCSR0B with UCSRB to match the register naming conventions 
of ATmega32. These changes ensure proper serial communication and improve compatibility with the ATmega32 architecture.


Enjoy Fat Filesystem on Atmel's Atmega32 !!

Sweta Yadav
SRF, NPL
2009
