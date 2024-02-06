# Raw Media Devices
## อะไรคือ Raw Media Device

&nbsp;&nbsp;&nbsp;&nbsp;<b>Raw Media Device</b> คือประเภทของ <b>Raw Device</b> ที่สามารถใช้ในการเชื่อมกับ Block Device ที่มีอยู่ (เช่น Harddisk หรือ SSD) และดำเนินการ I/O แบบดิบกับ Block Device นั้นโดยการใช้งาน I/O แบบดิบจะข้าม Cache ที่ปกติเชื่อมโยงกับ Block Device ทำให้โปรแกรมสามารถควบ Timings ของ I/O ไปยัง Physical Device นั้นได้มากขึ้น ที่กล่าวมาข้างต้นทำให้ Raw Device เหมาะสำหรับแอปพลิเคชันที่ซับซ้อน เช่น ระบบจัดการฐานข้อมูล ซึ่งมักจะมีระบบแคชของตัวเอง (เช่น Oracle DBMS)

## การนำ Raw Media Device มาใช้งาน
&nbsp;&nbsp;&nbsp;&nbsp;ก่อนอื่น ให้รันคำสั่ง `lsblk` เพื่อแสดงรายการดิสก์ในระบบและค้นหาไดร์ฟใหม่ของคุณ หากข้อมูลที่แสดงไม่เพียงพอ ให้ใช้คำสั่ง lsblk -o +MODEL,SERIAL เพื่อแสดงรายละเอียดรุ่นและหมายเลขประจำเครื่อง เมื่อคุณทราบชื่ออุปกรณ์ที่อ้างถึงไดร์ฟใหม่ (สมมติว่าเป็น /dev/sdb) ให้ติดตั้งตารางพาร์ติชันบนไดร์ฟ มีเครื่องมือและคำสั่งหลายอย่างที่สามารถทำได้ เช่น `parted`, `gparted`, `fdisk`, `cfdisk` และ `sfdisk` คุณเลือกใช้ตัวไหนก็ได้ขอเพียงรองรับตารางพาร์ติชันแบบ GPT `gparted` น่าจะเป็นตัวเลือกที่ง่ายที่สุดสำหรับระบบที่มีส่วนติดต่อผู้ใช้แบบกราฟิก สำหรับระบบที่ไม่มี GUI เราจะแนะนำวิธีใช้ fdisk ซึ่งเป็นคำสั่งที่ใช้งานได้กับระบบ Linux ทุกรุ่น (บางระบบยังคงใช้ parted เวอร์ชันเก่าที่ไม่รองรับ GPT)

<!-- First, run lsblk to list the system’s disks and identify the new drive. If that output
doesn’t give you enough information to conclusively identify the new drive, you
can list model and serial numbers with lsblk -o +MODEL,SERIAL.
Once you know which device file refers to the new disk (assume it’s /dev/sdb), install
a partition table on the disk. Several commands and utilities can do this, including
parted, gparted, fdisk, cfdisk, and sfdisk; it doesn’t matter which one you use, as
long as it understands GPT-style partition tables. gparted is probably the easiest
option on a system with a graphical user interface. Below, we show the fdisk recipe,
which works on all Linux systems. (Some systems still ship a version of parted
that doesn’t understand GPT.) -->

## คำสั่งที่ควรทราบ
### `lsblk`
&nbsp;&nbsp;&nbsp;&nbsp;คำสั่ง `lsblk` แสดงข้อมูลเกี่ยวกับอุปกรณ์บล็อกทั้งหมดที่มีหรือที่ระบุ โดยคำสั่งจะอ่านระบบไฟล์ sysfs และฐานข้อมูล udev เพื่อรวบรวมข้อมูล หากฐานข้อมูล udev ไม่พร้อมใช้งาน หรือ lsblk ถูกคอมไพล์โดยไม่รองรับ udev คำสั่งจะพยายามอ่าน LABELs, UUIDs และประเภทระบบไฟล์จากอุปกรณ์บล็อกโดยตรง ซึ่งในกรณีนี้จำเป็นต้องมีสิทธิ์ระดับ root

### `fdisk`
&nbsp;&nbsp;&nbsp;&nbsp;`fdisk` เป็นโปรแกรม Text User Interface ที่ใช้สำหรับการสร้างและจัดการ Partition Table โดย
รองรับ Partition Table แบบ GPT, MBR, Sun, SGI และ BSD

&nbsp;&nbsp;&nbsp;&nbsp;Block Device สามารถแบ่งออกเป็น Logical Disk ตั้งแต่หนึ่งดิสก์ขึ้นไป เรียกว่า Partition การแบ่งนี้จะถูกบันทึกไว้ใน Partition Table ซึ่งโดยทั่วไปจะอยู่ที่เซกเตอร์ 0 ของดิสก์ (ใน BSD จะเรียกว่า "disk slices" และ "disklabel")

### `mkfs`
&nbsp;&nbsp;&nbsp;&nbsp;คำสั่ง `mkfs` ใช้ในการสร้าง File System บนอุปกรณ์จัดเก็บข้อมูล เช่น ฮาร์ดดิสก์ พาร์ติชัน ชื่ออุปกรณ์หรือ mountpoint ระบุถึงอุปกรณ์ที่ต้องการสร้างระบบไฟล์ ตัวเลือก -t ระบุชนิดของระบบไฟล์ ตัวอย่างเช่น ระบบไฟล์ Linux Second Extended Filesystem ใช้คำสั่ง mkfs.ext2 (เหมือนกับคำสั่ง `mke2fs`) ส่วนระบบไฟล์ MS-DOS ใช้คำสั่ง `mkfs.msdos` ตัวเลือก fs-options คือตัวเลือกเฉพาะสำหรับแต่ละชนิดของระบบไฟล์ ส่วน blocks คือขนาดของระบบไฟล์เป็นหน่วยบล็อก 1024 ไบต์

## อ้างอิงจาก
* https://tldp.org/HOWTO/SCSI-2.4-HOWTO/rawdev.html
* https://docs.oracle.com/cd/A83908_02/NT816EE/DOC/nt.816/a73025/ch3.htm
* https://man7.org/linux/man-pages/man8/lsblk.8.html
* https://man7.org/linux/man-pages/man8/fdisk.8.html