![RAWMedia](/Assets/Raw%20Media%20Device.jpg)
## เนื้อหาในหัวข้อนี้
* [อะไรคือ Raw Media Device](#อะไรคือ-raw-media-device)
* [การนำ Raw Media Device มาใช้งาน](#การนำ-raw-media-device-มาใช้งาน)
* [คำสั่งที่ควรทราบ](#คำสั่งที่ควรทราบ)
  * [`lsblk`](#lsblk)
  * [`fdisk`](#fdisk)
  * [`mkfs`](#mkfs)
* [อ้างอิง](#อ้างอิงจาก)
## อะไรคือ Raw Media Device

&nbsp;&nbsp;&nbsp;&nbsp;<b>Raw Media Device</b> คือประเภทของ <b>Raw Device</b> ที่สามารถใช้ในการเชื่อมกับ Block Device ที่มีอยู่ (เช่น Harddisk หรือ SSD) และดำเนินการ I/O แบบดิบกับ Block Device นั้นโดยการใช้งาน I/O แบบดิบจะข้าม Cache ที่ปกติเชื่อมโยงกับ Block Device ทำให้โปรแกรมสามารถควบ Timings ของ I/O ไปยัง Physical Device นั้นได้มากขึ้น ที่กล่าวมาข้างต้นทำให้ Raw Device เหมาะสำหรับแอปพลิเคชันที่ซับซ้อน เช่น ระบบจัดการฐานข้อมูล ซึ่งมักจะมีระบบแคชของตัวเอง (เช่น Oracle DBMS)

## การนำ Raw Media Device มาใช้งาน
&nbsp;&nbsp;&nbsp;&nbsp;เริ่มจากการรันคำสั่ง `lsblk` เพื่อแสดงรายการดิสก์ในระบบและระบุไดร์ฟ หากต้องการแสดงข้อมูลเพิ่มเติมให้ใช้คำสั่ง `lsblk -o +MODEL,SERIAL` เพื่อแสดงรายละเอียดรุ่นและหมายเลขประจำเครื่อง เมื่อทราบชื่อไดร์ฟใหม่ที่ต้องการเข้าถึง (สมมติว่าเป็น /dev/sdb) ให้ตั้ง Partition Table บนไดร์ฟ โดยสามารถตั้งได้ด้วยคำสั่ง เช่น `parted`, `gparted`, `fdisk`, `cfdisk` และ `sfdisk` โดยสามารถเลือกใช้ตัวไหนก็ได้ขอเพียงให้มันรองรับ Partition Table แบบ GPT แต่ `gparted` น่าจะเป็นตัวเลือกที่ง่ายที่สุดสำหรับระบบที่มี GUI และสำหรับระบบที่ไม่มี GUI ควรใช้ `fdisk` ซึ่งเป็นคำสั่งที่ใช้งานได้กับระบบ Linux ทุกรุ่น


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
### `lsblk`  - list block devices
&nbsp;&nbsp;&nbsp;&nbsp;คำสั่ง `lsblk` แสดงข้อมูลเกี่ยวกับ Block Device ทั้งหมดที่มีหรือที่ระบุ โดยคำสั่งจะอ่านระบบไฟล์ `sysfs` และฐานข้อมูล `udev` เพื่อรวบรวมข้อมูล หากฐานข้อมูล `udev` ไม่พร้อมใช้งาน หรือ `lsblk` ถูกคอมไพล์โดยไม่รองรับ `udev` คำสั่งจะพยายามอ่าน `LABELs`, `UUIDs` และประเภทระบบไฟล์จากอุปกรณ์บล็อกโดยตรง ซึ่งในกรณีนี้จำเป็นต้องมีสิทธิ์ระดับ root

#### การใช้งานแบบระบบุทั้งหมด
```sh
jarukrit@coosdemo:~$ lsblk
```
ผลลัพธ์
```
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0                       7:0    0  63.9M  1 loop /snap/core20/2105
loop1                       7:1    0    87M  1 loop /snap/lxd/26881
loop4                       7:4    0 114.4M  1 loop /snap/lxd/26741
loop6                       7:6    0  63.5M  1 loop /snap/core20/2015
loop7                       7:7    0  40.9M  1 loop /snap/snapd/20290
loop8                       7:8    0  40.4M  1 loop /snap/snapd/20671
sda                         8:0    0 111.8G  0 disk
├─sda1                      8:1    0     1G  0 part /boot/efi
├─sda2                      8:2    0     2G  0 part /boot
└─sda3                      8:3    0 108.7G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 100.7G  0 lvm  /
sdb                         8:16   0   3.6T  0 disk
└─sdb1                      8:17   0   3.6T  0 part /mnt/data
```

#### การใช้งานแบบเจาะจง Block Device
```sh
jarukrit@coosdemo:~$ lsblk /dev/sda
```
ผลลัพธ์
```
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda                         8:0    0 111.8G  0 disk
├─sda1                      8:1    0     1G  0 part /boot/efi
├─sda2                      8:2    0     2G  0 part /boot
└─sda3                      8:3    0 108.7G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 100.7G  0 lvm  /
```

#### การใช้งานแบบเจาะจง Block Device และ แสดงข้อมูลเพิ่มเติม
```sh
jarukrit@coosdemo:~$ lsblk /dev/sdb -o +MODEL,SERIAL
```
ผลลัพธ์
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS MODEL           SERIAL
sdb      8:16   0  3.6T  0 disk             TOSHIBA HDWD240 7191S4G5S5HH
└─sdb1   8:17   0  3.6T  0 part /mnt/data
```
<hr />

### `fdisk` - manipulate disk partition table
&nbsp;&nbsp;&nbsp;&nbsp;`fdisk` เป็นโปรแกรม Text User Interface ที่ใช้สำหรับการสร้างและจัดการ Partition Table โดย
รองรับ Partition Table แบบ GPT, MBR, Sun, SGI และ BSD

&nbsp;&nbsp;&nbsp;&nbsp;Block Device สามารถแบ่งออกเป็น Logical Disk ตั้งแต่หนึ่งดิสก์ขึ้นไป เรียกว่า Partition การแบ่งนี้จะถูกบันทึกไว้ใน Partition Table ซึ่งโดยทั่วไปจะอยู่ที่เซกเตอร์ 0 ของดิสก์ (ใน BSD จะเรียกว่า "disk slices" และ "disklabel")
<hr />

### `mkfs` - build a Linux filesystem
&nbsp;&nbsp;&nbsp;&nbsp;คำสั่ง `mkfs` ใช้ในการสร้าง File System บนอุปกรณ์จัดเก็บข้อมูล เช่น ฮาร์ดดิสก์ พาร์ติชัน ชื่ออุปกรณ์หรือ mountpoint ระบุถึงอุปกรณ์ที่ต้องการสร้างระบบไฟล์ ตัวเลือก -t ระบุชนิดของระบบไฟล์ ตัวอย่างเช่น ระบบไฟล์ Linux Second Extended Filesystem ใช้คำสั่ง mkfs.ext2 (เหมือนกับคำสั่ง `mke2fs`) ส่วนระบบไฟล์ MS-DOS ใช้คำสั่ง `mkfs.msdos` ตัวเลือก fs-options คือตัวเลือกเฉพาะสำหรับแต่ละชนิดของระบบไฟล์ ส่วน blocks คือขนาดของระบบไฟล์เป็นหน่วยบล็อก 1024 ไบต์

## อ้างอิงจาก
* https://tldp.org/HOWTO/SCSI-2.4-HOWTO/rawdev.html
* https://docs.oracle.com/cd/A83908_02/NT816EE/DOC/nt.816/a73025/ch3.htm
* https://man7.org/linux/man-pages/man8/lsblk.8.html
* https://man7.org/linux/man-pages/man8/fdisk.8.html