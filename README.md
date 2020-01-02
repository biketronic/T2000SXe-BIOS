# T2000SXe-BIOS
Toshiba T2000SXe BIOS markup / reverse engineering - Goal is to add HDD options & support larger hdds. 

Current status: working.

This is licensed as - pay it forward, don't charge for it!

The BIOS is a removable 1MBit flash chip you can buy for $1 off aliexpress (so you can easily keep the original bios if you want). It is in a socket under the keyboard and easily replacable. But you need flashing hardware.

Use of IDA Pro software to reverse engineer.

The T2000SXe original BIOS seems to store a limited number of HDD options. There is space for just 6 HDD types to do a string compare. But there is space for 15 types of cylinders/etc. to set.... The limit should be 15 types. If no hdd is detected (as in an exact string match of 6 HDD types such as CP2064) it won't try and boot one. There are 3 compares that prevent an attempt, it checks the bios setting matches the detected type, the bios setting is not "no hdd" and that a hdd is detected.

There are 3 parts to this work:

1. HDD Setup - you need to format the hdd to match the CHS setting. There are DD images.
2. BIOS mods - byte edits in the bios to enable the HDD CHS settings. There is a original and modded BIN for flashing.
3. Repairs - replacement drive band info for FDD - hard to set up hdd without a working fdd but possible.

Currently the simplest option has been to:
Disable the checksum so it boots
Change BIOS menu to cycle through options rather than HDD/NO HDD (increased to 4 digit hdd sizes)
Change HDD detection to ignore detected HDD, and use BIOS menu selection.
Set HDD parameters manually by editing the BIOS.

Markup covers large parts of the bios especially in the relevant areas. Int 13 has been located, but modifying it is beyond me at this point.

There are various options in the BIOS menu for testing CHS limitations and a 504MB option (CHS 1024,16,63).
So far I have a dying 120MB Toshiba MK1722FCV hdd working without anydrive using updated hardcoded CHS info in bios.
Getting it working in virtualbox and the T2000SXe was not straightforward. It would often boot in virtualbox but not the T2000SXe. So I have made DD images.
So far Dos 6.22 works, not 7.10.
2GB CF card working with DD of 120MB hdd BIOS setting 120MB
2GB CF card working with 500MB partition BIOS setting 504MB

So far it is a fully working solution. 504MB seems to be the limit.
Known BUG - Bios options show testing options 2030MB and 8500MB not 2000MB and 8000MB.
