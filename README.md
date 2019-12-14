# T2000SXe-BIOS
Toshiba T2000SXe BIOS markup / reverse engineering - Goal is to add HDD options

Use of IDA Pro software

This is licensed as - pay it forward, don't charge for it!

Markup is fairly complete especially in the relevant areas.

The T2000SXe seems to store a limited number of HDD options. There is space for just 6 HDD types to do a string compare. But there is space for at least 14 types of cylinders/etc. to set.... The limit should be 14 types.

I suppose some types could be removed to save space?

The plan is if the HDD compare fails to set it to type undefined/USER, which has the maximum sectors etc possible.
The laptop will then at least boot with a HDD set, and you can use ANYHDD or similar to make it work.
Also the BIOS menu could be modified to cycle through options rather than none, but it may be overwritten by the bios detection...?

As only 14 types can be set and I have no idea how to add a detection system.

What currently seems to happen is if no hdd is detected (as in an exact string match of 6 HDD types such as CP2064) it won't try and boot one.

If anyone knows how to do something more complex than this feel free!

I am not entirely sure how the bios checksum works but the code has been found and marked. I may just bypass it.

Currently it has no modifications


Read the Toshiba TC57H1024D-85 as a M27C1024 in TL-866 programmer
uncheck "check ID" as it's not really this chip
Now you have a backup of your BIOS.

To flash the eeprom
(Or Buy a m27c1024-10f1 by ST and keep the original)
Blank check - OK
VPP 12.5V
VDD write 6.25V
Puls Delay 100us
VCC verify 5.00V

Open file .BIN with defaults
Ascii looks like crap - "A.llR gith seResvrde" - this looks the same as the original chip. It's normal.

Press program (takes 30 sec)
Verify - success

Now try and boot T2000SXe with original bios and new M27C1024-10F1
It boots - so the bios is being read successfully from the EEPROM.


NB 
5A6D: CP2024 and other HDD identifier list

: Capacity displayed in bios menu "No Drive" "20" etc.

E401: Cylinder-head-sector info for each HDD type


MAX: 255 heads (dos limit)

Sectors / track 63 or 255? <8GB should be 63  max


ATTEMPT #1
MK1722FCV 120mb hdd  - set as type 1
Heads   8 
Cyl     842 =34Ah
Sect    38 = 26h
WP comp 0
LZ      842
Capacity 131MB

Use edit-> patch program -> change byte to change the HDD info

Change to MK1722FCV drive
E401 - 34Ah,8,ff00,ff,0,0,34ah,26h disk type 10 = new
E401 00FFFF000008034A
E409 0026034A00000000

5A5A change offset JD-E2825P to equal CP2044 to free up space
& add MK1722FCV with length of "10" aka Ah

5A5E 5A85405A7E305A7E (5A7E twice)
5A66 0A085A96605A8F50 (...)
5A6E 4346323237314B4D (MK1722FC)
5A76 0000000000000056 (V.......)

Change BIOS to read MK instead of 20 (2 character limit :-(

4378 004B4D0065766972 (rive.MK.)

Checksum????? 52A4? 62F1?
5E38 display bad checksum CMOS.
62F1 - POST 9 call ???




Use script http://www.openrce.org/downloads/details/57/PE_Scripts
to write edits from IDA pro :-( 
use the write script. Or not
Edit -> Patch Program -> Apply patches to input file.

Result - does not even display splash page - just a blinking underline type cursor - should give an error!

Try bypassing checksum - 7960 - jnz memory verify fail - set to nop

7960 153926FFFEBF3975 -> 75 -> 90 = NOP  & 796B

Try reducing length from 10 to 5 for hdd string compare- no difference

Must be another checksum somewhere.

62F1 possible checksum on start
stc - set carry = fail
set 6314 f9->90 to NOP

SUCCESS!!!!!!! it now shows text on start
No drive detected however..

Try setting length back to 10 from 5.



