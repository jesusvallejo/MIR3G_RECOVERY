------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------
					    WARNING
------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------

THIS METHOD WILL ONLY WORK ON MI ROUTER 3G (ONLY GIGABIT VERSION , DONT FLASH THIS IN MI ROUTER 3)

-------------------------------------------------------------------------------------------------


This method would not be possible without NORway project
https://github.com/hjudges/NORway
This made possible to dump the full.bin and to flash the bricked one
Some parts of this guide are owned by hjudges.
Also special thanks to WAY-launchers,for making it easier
https://github.com/littlebalup/WAY-launchers

Software required:
-linux, mac, windows(recommended)

-fullNandClone.zip

-NORway.zip

-this README.TXT

-WAY-LAUNCHER(only for windows, recommended)

-Prerequisites for Windows:
	Python 2.7.2 (http://www.python.org/ftp/python/2.7.2/python-2.7.2.msi)
	pyserial 2.5 (http://pypi.python.org/packages/any/p/pyserial/pyserial-2.5.win32.exe)


Hardware required:

-teensy++ 2.0 (6$ aliexpress , ebay , etc)
	¡¡¡¡3.3v is required so be sure to use the voltage regulator , cut the 5v trace and bridge 3.3 pad!!!!!!
	
-tsop48 adapter(10$ ebay)
-mi router 3g 
-solder iron , tin , flux
-jumpers or wire to connect teensy and tsop48 adapter

/////////////////////////////////////////////////

for ease you could buy a solderless tsop adapter
ususally kown as 360-Clip, solderless IC adapter, 
360 tsop(30-35$ aliexpress)

////////////////////////////////////////////////






Now that you've been warned, KEEP READING CLOSELY, this method will replace the whole Nand Flash contents, 
this method involves some risks i'll enumerate:

1. Nand flash is susceptible of having bad sectors(most likely it will have bad sectors), this means that 
the provided image could not suit your nand(this will be addresed in this tutorial a bit forward).
2. This method requires some soldering skills as well as soldering equipment to remove the Nand flash chip 
from the board(another method is provided but never tried)


Any way if you are reading this you probably have a nice brick with an odd form of a router so this risks wont 
make any harm to a brick, brick being a completly fucked up nand,ie:

No bootloader( if you still have a working bootloader dont follow this guide), broken nand(suddenly nand gets 
corrupted ), and i think this are the only cases you should take the risk.

if you have no bootloader , youll have to desolder the nand flash(nandflash.img for reference), make sure all pins 
are free an there are no solder bridges anywhere.Once you have got a good looking nand flash you can keep reading 
on flashing section.



Flashing a nand is complicated, nands in contrary to nors have bad sectors, this is why
a 128mb nand is in reality 138 mb , this difference makes possible to address those bad sectors
to a working one, this is done by the bootloader,this means that there are two memory addreses
one physical and on virtual, if the bootloader in full.bin has a physical address that is bad
in your nand it may not work, this could be addressed by NORway , dont know if it already does, and
jump those bad sectors.this means there is a posibility of not proper recovery, then you should buy
a new nand of the same size.

WARNING, removing tsop48 packages(nand flash format) is no joke and ripping pads is pretty easy, 
be gentle when raising the chip from the pcb, also be careful to not overheat the pads, it is recommended using an
airsoldering station, if not avaible a soldering iron , enough tin wire, flux is also recommended, this is the original
method: add a lot , i mean a lot , of tin to cover all pins , keep an eye on heat , on both pin sites.
when all pins are covered , heat one side from pin1 to pin 24 , up down , up down , when the solder is flowing,raise it ,
yes dont pull , raise it with a razor being careful not to damage the pcb traces, when a side is in the air, then
heat the site that is still attached to the pcb and grabity will do the rest. now you have a mess, check that no pads 
have been ripped of, clean the pads on the pcb preparing it for after the flashing.now clean all pins in the nand flash
there cant be any bridges.

if this looks pretty difficult, mate you are a lucky boy , there are adapters (originally used for ps3 and xbox360)
for tsop48 that require no desoldering, but they are not cheap.(360-Clip, solderless IC adapter, 48-pin, TSOP48)

the used enviroment will be easyWAY directory, the tools one is only for reference and includes more files than the needed


Now that we have the nand ready, we will prepare the hardware

1. Flash Teensy with "NORway\teensyNAND\default\NANDway.hex" (you can do it the factory's way)
	- Rename NANDway.hex to NANDWay_SignalBoosterEdition.hex
	- Run WAY-launcher.exe
	- Click on nand , click on the teensy picture, select NANDWay_SignalBoosterEdition.hex, when displayed by the console press the ony button on the teensy


2. Wiring the adapter(follow provided wiring picture)



	NAND Hardware connections:
		NAND		Teensy 	
	*	IO0-7		PF0-7				
	7	RY/BY#		PB6			
	8	RE#			PB1			
	9	CE#			PB0		
	12	Vcc			3.3v - Vcc		
	13	GND			GND		
	16	CLE			PB2		
	17	ALE			PB3			
	18	WE#			PB5			
	19	WP#			PB4		

	


3. Flash fullNandClone

	1. check checksum
	2. run Way-Launchers
	3. select nand, info , start, it should output the 138 size of the nand
	4. select write , select on Dump file full.bin and start it will take around 30 minutes

4. solder back the nand



Now leds will work again and will boot, but it will have a diferent mac and name that it previously had , 
this mac and name is alredy in use, so i encourage you to keep following the guide.

MAC is an enviroment variable that is written only once , when nand is flashed and is write protected in the original bootloader,
so in order to change this we will have to flash breed bootloader.

1. gain ssh access in mir3g
2. flash breed bootloader 
3. change mac , wifimac , name , wifiname parameters(this are writen on the sticker)
4. now , flash on partition 2 , without reboot, miwifi.bin from miwifi.com webpage
5. select bootloader flash , and select mtdblock1.bin, select flash partition 1 with miwifi.bin , select reboot and flash

now two things may happen, it will boot with proper parameters , nothing else to do , or a softbrick , a red still led wich is pretty easy solved,
format a pendrive in fat32 , get miwifi.bin inside this pendrive, it has to be named "miwifi.bin", insert usb in mir3g  press reset button until it blinks 
orage, wait until led is blue and it should be ready to use with its correct parameters.



FAQ


1. will this work on a mir3 

no it wont , if you have a working mir3 you could make a dump of the working one and flash it into the bricked, 
to change parameters , flash breed for mir3 , and original mir3 bootloader , both are easy to find online


2.will happen anything if i dont chage the parameters

really... this backup is from a working and in use mir3g without it you would not be able to revive it for 16 bucks as you would need a working one 36$
also i dont know if miwifi app will let you use it like that as is a clone of mine :)


3.where can i find everything




https://github.com/hjudges/NORway (master has images of wiring, release 0.7 nandway.hex for the teensy)

https://github.com/littlebalup/WAY-launchers

https://github.com/jesusvallejo

https://cloud.mail.ru/public/2UJd/EcditxKmw  (CRC-32: 2edbfadc MD5: efdd4d3fe0a5876243ac5a0e990134e4 you can flash this via Breed (to Bootloader partition)) 

http://4pda.ru/forum/lofiversion/index.php?t837667-2760.html

https://breed.hackpascal.net/breed-mt7621-xiaomi-r3g.bin



4. way launcher does not work

windows may have flagged it as a virus, it is not and if you want you can check it at https://github.com/littlebalup/WAY-launchers and compile it your self

























