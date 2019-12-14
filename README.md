# T2000SXe-BIOS
Toshiba T2000SXe BIOS markup / reverse engineering - Goal is to add HDD options

Use of IDA Pro software

This is licensed as - pay it forward, don't charge for it!

Markup is fairly complete especially in the relevant areas.

The T2000SXe seems to store a limited number of HDD options. There is space for just 6 HDD types to do a string compare. But there is space for at least 14 types of cylinders/etc. to set....

The plan is if the HDD compare fails to set it to type undefined/USER, which has the maximum sectors etc possible.
The laptop will then at least boot with a HDD set, and you can use ANYHDD or similar to make it work.

What currently seems to happen is if no hdd is detected (as in an exact string match of 6 HDD types such as CP2064) it won't try and boot one.

If anyone knows how to do something more complex than this feel free!

I am not entirely sure how the bios checksum works but the code has been found and marked. I may just bypass it.

Currently it has no modifications


Read the Toshiba TC57H1024D-85 as a M27C1024

To flash the eeprom
Buy a m27c1024-10f1 by ST
VPP 12.5V

