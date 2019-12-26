# T2000SXe-BIOS
Toshiba T2000SXe BIOS markup / reverse engineering - Goal is to add HDD options

The BIOS is a removable 1MBit flash chip you can buy for $1 off aliexpress. It is in a socket under the keyboard and easily replacable. But you need flashing hardware. If I get a working version I may consider shipping flashed chips if you can figure out how to contact me :-)

Use of IDA Pro software

This is licensed as - pay it forward, don't charge for it!

Markup is fairly complete especially in the relevant areas.

The T2000SXe seems to store a limited number of HDD options. There is space for just 6 HDD types to do a string compare. But there is space for 15 types of cylinders/etc. to set.... The limit should be 15 types.

What happens with the normal BIOS is if no hdd is detected (as in an exact string match of 6 HDD types such as CP2064) it won't try and boot one. There are 2 compares that prevent an attempt.

The plan is if the HDD compare fails to set it to type undefined/USER, which has the maximum sectors etc possible.
The laptop will then at least boot with a HDD set, and you can use ANYHDD or similar to make it work.
Also the BIOS menu could be modified to cycle through options rather than none, but it may be overwritten by the bios detection...?

See notes for progress.

Currently the simplest option has been to:
Disable the checksum so it boots
Change BIOS menu to cycle through options rather than HDD/NO HDD
Change HDD detection to ignore detected HDD, and use BIOS menu selection
Set HDD parameters manually.


Modified version -mod.BIN:
Can select any hdd in bios
Current 120MB test disk shows ANYDISK so it is reading it!
It then says "non system disk or disk error"...

