UKM7EIN3 is a terminal and file transfer program for the Tatung Einstein. It is a rough hack of UKM7EIN2 to add RTS/CTS flow control on the incoming data stream.
When connected to tcpser on a Raspberry Pi it can run reliably at up to 4800 baud - I could only manage 300 baud without flow control.
So far I have only added flow control to the terminal mode - making the file transfer features essentially useless.

If you are thinking about using this, please be aware that I have very little idea what I am doing, and it is almost certainly broken in many ways.

From the source code ...
> UKM7(EIN2 replaces UKM7EIN1.COM/HEX/ZSM/DOC)
> This program was originally written in 1977 by Ward Christensen.
> Ward's comments were removed, Terminal File, Batch Mode
> and a lot of bugs were added in 1980 by Mark Zeiger and
> James Mills. The CRC checks were added by Paul Hansknecht
> in June 1981.
> 
> Improved terminal file facilities, menus and a general tidy
> up of a very untidy program, removal of all modem dependent
> features, many bugs and adaptation for the UK CP/M Users
> Group Library by David Back. 4 May 1983.

> The file exchange protocols have not been altered and are
> generally compatible with the MODEMX series of programs in
> the US CP/M UG Library.
> 
> UKM7 is compatible with CP/M 1.4 and CP/M 2.2.
> as it stands it will only run correctly on TATUNG EINSTEIN
> 
> EINSTEIN dependant routines added 10/5/87 Dave West/Neil Imrie
> Baud rate setting routine intended to include modem control in future ....

I built it on my Einstein using Z80ASMUK from the comms disk image at the wonderful [Mike's Retro Tech](https://mikesretrotech.co.uk/userfiles/tatung%20einstein/software/communications/Telecommunications%20%28Surrey%20Software%29/comms.zip)
and HEXTOCOM from Miguel Vis' excellent [repository](https://github.com/MiguelVis/zsm). I could probably use Miguel's Z80ASMUK, but I haven't tried yet.

As a tip for new players, the Zilog Z80 assembler, Z80ASMUK, has some "interesting" conventions ...
* The source file must have a .ZSM extension
* What looks like the file extension actually indicates the source drive, the output drive for the HEX file and control of the PRN file
* I used ...
```
Z80ASMUK UKM7EIN3.ABZ
```
... to read UKM7EIN3.ZSM from drive 0: , write the resulting .HEX file to to drive 1: , and not produce the *huge* PRN file.

Then it's a quick ...
```
HEXTOCOM UKM7EIN3
```
... and you have your executable.

Initial tips for UKM7EIN3 are :-
* Enter 'M' at the command prompt to get the menu
* Remember to use 'O' to enter "online mode" before using the 'T'erminal
* When online, Ctrl-E will return to the command prompt and Ctrl-D will give you the "Terminal menu"

My start_tcpser.sh file looks like this ...
```
#!/bin/bash

phonebook="/home/ian/phonebook.txt"
baud='4800'
dev='/dev/ttyUSB0'

for i in `cat $phonebook`; do
        n="$n -n$i "
done

/usr/local/bin/tcpser -l4 -s ${baud} -d ${dev} $n -i"K3&C0"
```

Happy BBSing.
