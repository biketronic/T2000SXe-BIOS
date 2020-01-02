# T2000SXe-BIOS
Toshiba T2000SXe BIOS markup / reverse engineering - Goal is to add HDD options. 

The BIOS is a removable 1MBit flash chip you can buy for $1 off aliexpress (so you can easily keep the original bios if you want). It is in a socket under the keyboard and easily replacable. But you need flashing hardware. If I get a working version I may consider shipping flashed chips if you can figure out how to contact me :-)

Use of IDA Pro software to reverse engineer.

This is licensed as - pay it forward, don't charge for it!

Markup is fairly complete especially in the relevant areas. Int 13 does not make sense to me yet but it has been located AFAIK.

The T2000SXe seems to store a limited number of HDD options. There is space for just 6 HDD types to do a string compare. But there is space for 15 types of cylinders/etc. to set.... The limit should be 15 types.

What happens with the normal BIOS is if no hdd is detected (as in an exact string match of 6 HDD types such as CP2064) it won't try and boot one. There are 2 compares that prevent an attempt, it checks the bios setting matches the detected type, the bios setting is not "no hdd" and that a hdd is detected.

See notes for progress and all the trials and errors, and bios edit locations.

There are 3 parts to this work:

1. HDD Setup - you need to format the hdd to match the CHS setting
2. BIOS mods - byte edits in the bios to enable the HDD CHS settings
3. Flashing info - how to flash the BIN image to an EPROM
4. FDD notes - replacement drive band info for FDD - hard to set up hdd without a working fdd but possible.

Currently the simplest option has been to:
Disable the checksum so it boots
Change BIOS menu to cycle through options rather than HDD/NO HDD (increased to 4 digit hdd sizes)
Change HDD detection to ignore detected HDD, and use BIOS menu selection.
Set HDD parameters manually by editing the BIOS.

Modified version -mod.BIN:
Has some options 40MB CP2044, 60MB CP2064, 120MB MK1722FCV, 504MB (1024,16,63) probable limit

So now there are various options in the BIOS menu for testing CHS limitations and a 504MB option (CHS 1024,16,63).
So far I have a dying 120MB Toshiba MK1722FCV hdd working without anydrive using updated hardcoded CHS info in bios.
Getting it working in virtualbox and the T2000SXe was not straightforward. It would often boot in virtualbox but not the T2000SXe.
So far Dos 6.22 works, not 7.10.
2GB CF card working with DD of 120MB hdd BIOS setting 120MB
2GB CF card working with 500MB partition BIOS setting 504MB

So this appears to be a fully working 120-504MB solution so far. More testing required for larger sizes but seems so far 504MB is a hard limit. Known BUG - Bios options show 2030 and 8500MB not 2000 and 8000MB.
