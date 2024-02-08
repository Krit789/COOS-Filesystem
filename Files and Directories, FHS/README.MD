![Cabinet](/Assets/Files%20and%20Directories,%20FHS.jpg)
# Root Directory
* Linux มีโครงสร้าง directory เป็นแบบลำดับชั้นเดียว (Single hierarchical directory) นั้นก็คือทุกอย่างเริ่มต้นจาก root directory หรือก็คือเครื่องหมาย `/` จากนั้นขยายไปเป็น directory รองต่างๆ ซึ่งจะ ต่างจากระบบปฏิบัติการ Window ที่จะมี "ไดรฟ์" ต่างๆ เช่น C, D, E เป็นต้น และ Linux จัดเรียง directory ที่ถัดจาก root directory ตามลำดับความสำคัญในกระบวนการบูธจากมากไปน้อย

โครงสร้างมาตรฐานของ root directory (`/`) ที่กำหนดโดยมาตรฐาน Filesystem Standard (FSSTND) จะมี directory ที่จำเป็นดังต่อไปนี้ :

| Directory  | คำอธิบาย                            |
|-------|---------------------------------------|
| `/bin`   | เก็บ library คำสั่งพื้นฐานที่จำเป็นสำหรับการทำงานของระบบเบื้องต้น |
| `/boot`  | เก็บไฟล์ที่เกี่ยวข้องกับการบูธเครื่อง                     |
| `/dev`   | เก็บไฟล์พิเศษที่มีความเกี่ยวข้องกับ device ที่เชื่อมต่อกับระบบ                 |
| `/etc`   | เก็บไฟล์ system config ของ host                  |
| `/lib`   | รวม library ที่ใช้ร่วมกันที่จำเป็นและโมดูลเคอร์เนล              |
| `/media` | ไลบรารีสำหรับสื่อแบบถอดได้ เช่น ไดรฟ์ USB               |
| `/mnt`   | เป็น directory สำหรับเชื่อมต่ออุปกรณ์เก็บข้อมูลแบบชั่วคราวหรือแบบ removable เช่น USB เป็นต้น                   |
| `/opt`   | ใช้เก็บโปรแกรมส่วนขยายอื่นๆ หรือโปรแกรมประเภท third party software ที่ไม่ได้ถูกติดตั้งมาตั้งแต่แรกกับระบบ                           |
| `/sbin`  | สำหรับเก็บ library หรือ command line tool ที่ใช้ได้เฉพาะ root user                        |
| `/srv`   | เก็บข้อมูลสำหรับ service ที่ให้บริการโดยระบบ                        |
| `/tmp`   | เก็บไฟล์ชั่วคราว                                       |
| `/usr`   |ลำดับชั้นรอง  (Secondary hierarchy) สำหรับไฟล์และโปรแกรมที่เกี่ยวข้องกับผู้ใช้ส่วนใหญ่     |
| `/var`   | เก็บไฟล์ที่มีการเปลี่ยนแปลงไฟล์ชั่วคราวที่สร้างโดยโปรแกรมหรือผู้ใช้ เช่น log หรือ spool files                       |

ไดเร็กทอรีเพิ่มเติม :
| Directory  | คำอธิบาย                             |
|-------|---------------------------------------|
| `/home` | Home directory ของผู้ใช้                     |
| `/lib`  | เก็บ library สำหรับการทำงานของโปรแกรม |
| `/root` | Home directory สำหรับ root user              |


* โดยไดเร็กทอรีอื่นๆใน root directory ทั้งหมดถือว่าเป็น "children" ของ root directory (`/`) ซึ่งพาร์ติชันที่มีร root file system จะถูกโหลดเป็นอันดับแรกในตอนบูตระบบ และระบบจะไม่สามารถบูตได้หากไม่พบพาร์ติชันนี้

# /bin
คำสั่งพื้นฐานที่จำเป็นสำหรับการทำงานของระบบ 
ไดเร็กทอรี /bin สามารถใชัได้โดยทั้ง super user และ ผู้ใช้ปกติ โดยจะมีคำสั่ง shell bash ใช้ทั่วไป เช่น cp, mv, rm, cat, ls เป็นต้น

| คำสั่ง | คำอธิบาย |
|---|---|
| `chmod`| เปลี่ยนแปลงสิทธิ์การเข้าถึงไฟล์ |
| `chown` | เปลี่ยนแปลงเจ้าของและกลุ่มของไฟล์ |
| `cp` | คัดลอกไฟล์และไดเร็กทอรี |
| `date` | แสดงวันที่และเวลา |
| `dd` | แปลง(convert) และคัดลอก(copy) ไฟล์ |
| `df` | แสดงการใช้พื้นที่ดิสก์ของระบบไฟล์ |
| `dmesg` | เป็นการให้แสดงผลเหมือนตอน Boot |
| `echo` | แสดงข้อความออกทางหน้าจอ |
| `false` | คืนค่าสถานะทำงานผิดพลาด |
| `hostname` | แสดงหรือตั้งชื่อโฮสต์ของระบบ |
| `kill` | คำสั่งสำหรับยกเลิก Process |
| `ln` | สร้างลิงก์ระหว่างไฟล์ |
| `login` | เริ่มเซสชันบนระบบ |
| `ls` | แสดงรายการเนื้อหาของไดเร็กทอรี |
| `mkdir` | สร้างไดเร็กทอรี |
| `mknod` | สร้างไฟล์พิเศษแบบบล็อกหรืออักขระ |
| `more` | แสดงข้อความทีละหน้า |
| `mount` | ใช้ในการเมาท์ระบบไฟล์จากอุปกรณ์จัดเก็บข้อมูล เช่น ฮาร์ดดิสก์ หรือดิสก์ USB |
| `mv` | ย้าย/เปลี่ยนชื่อไฟล์ |
| `ps` | แสดงสถานะของกระบวนการ |
| `pwd` | แสดงชื่อไดเร็กทอรีทำงานปัจจุบัน |
| `rm` | ลบไฟล์หรือไดเร็กทอรี |
| `rmdir` | ลบไดเร็กทอรี |
| `sed` | ตัวแก้ไขข้อความแบบสตรีม |
| `sh` | คำสั่งเรียกใช้ Bourne again shell ของระบบ Unix,Linux |
| `stty` | แก้ไขการตั้งค่าของการแสดงผลทาง terminal หรือ print การตั้งค่าของ terminal ณ ปัจจุบัน|
| `su` | เปลี่ยน user ID |
| `sync` | flush บัฟเฟอร์เขียนลง disk เพื่อให้ค่าใน cache กับค่าในที่เก็บข้อมูลตรงกัน |

# /boot
&nbsp;&nbsp;&nbsp;&nbsp;boot directory นั้นเก็บไฟล์ทั้งหมดที่เกี่ยวข้องและใช้งานในกระบวนการบูธ โดย /boot จะถูกใช้ก่อนที่ kernel จะ execute user-mode program แต่ก็จะมีไฟล์บางไฟล์ที่ไม่ได้ใช้ในตอนบูธ เช่น configuration file (GRUB) และ map installer

&nbsp;&nbsp;&nbsp;&nbsp;ตามที่กล่าวไปข้างต้นที่ /boot จะเก็บสิ่งที่ใช้งานก่อนที่ kernel จะ execute user-mode program ซึ่งไฟล์ที่ถูกเรียกใช้เหล่านี้อาจรวมไปถึงไฟล์ backup ของคำสั่งที่ใช้ในกระบวนการ boot หลัก, Sector/system map files (ไฟล์ข้อมูลเกี่ยวกับ disk layout), คำสั่งหลักของระบบปฏิบัติการ และ boot ไฟล์อื่นๆที่เราไม่สามารถแก้ไขได้โดยตรง คำสั่งที่ทำการจัดเรียงลำดับสำหรับ boot loader เพื่อให้ boot ได้นั้นจัดเก็บไว้ใน /sbin, ไฟล์ config สำหรับ boot loader จัดเก็บไว้ใน /etc และ system kernel จะอยู่ใน / หรือไม่ก็ใน /boot (ตาม Debian จะเป็น /boot แต่จริงๆแล้วเป็น symbolic link ไปที่ / ตามหลักมาตรฐาน FSSTND)  

ถ้าหากเราลองใช้คำสั่ง `ls` แสดงรายการของสิ่งที่อยู่ใน directory ก็จะมีไฟล์ประมาณนี้ : 

![boot_dir](/Assets/boot_dir_ls.png)

 * `config−kernel−version`
จัดเก็บข้อมูลการ config ของ kernel ซี่งเป็นไฟล์ที่มีความสำคัญมากเมื่อ compile kernel บนระบบหรือ device module อื่น โดยไฟล์นี้ถูกสร้างขึ้นมาอัตโนมัติและเราไม่ควรไปแก้ไขมันโดยตรงๆ
สี่งที่อยู่ในไฟล์ config ก็จะมีประมาณนี้ : 
```
CONFIG_DM_ERA=m
CONFIG_DM_CLONE=m
CONFIG_DM_MIRROR=m
CONFIG_DM_LOG_USERSPACE=m
CONFIG_DM_RAID=m
CONFIG_DM_ZERO=m
CONFIG_DM_MULTIPATH=m
CONFIG_DM_MULTIPATH_QL=m
CONFIG_DM_MULTIPATH_ST=m
CONFIG_DM_MULTIPATH_HST=m
CONFIG_DM_MULTIPATH_IOA=m
CONFIG_DM_DELAY=m
# CONFIG_DM_DUST is not set
CONFIG_DM_INIT=y
CONFIG_DM_UEVENT=y
CONFIG_DM_FLAKEY=m
CONFIG_DM_VERITY=m
...
```
 * `System.map-VERSION` :
เป็นเหมือน look-up table สำหรับ matching ค่าและ function กับพื้นที่ใน memory ซี่ง content ที่อยู่ด้านในจำเป็นต้องเป็น superuser ที่สามารถเห็นได้
```
ffffffff839217f0 d __event_enter__openat2
ffffffff839217f8 d __event_exit__openat
ffffffff83921800 d __event_enter__openat
ffffffff83921808 d __event_exit__open
ffffffff83921810 d __event_enter__open
ffffffff83921818 d __event_exit__fchown
ffffffff83921820 d __event_enter__fchown
ffffffff83921828 d __event_exit__lchown
ffffffff83921830 d __event_enter__lchown
ffffffff83921838 d __event_exit__chown
ffffffff83921840 d __event_enter__chown
ffffffff83921848 d __event_exit__fchownat
ffffffff83921850 d __event_enter__fchownat
ffffffff83921858 d __event_exit__chmod
...
```
 * `vmlinuz−kernel−version` :
 โดยปกติจะเป็น kernel หรือ symbolic link ไปที่ kernel
 * `/grub` :
 เป็น subdirectory ที่มีไฟล์ GRUB configuration รวมถึง boot-up image<br>&nbsp;&nbsp;&nbsp;&nbsp;
 GNU GRand Unified Boot loader (GRUB) คือ เป็นระบบ boot ที่ทำโดย GNU project ซี่งเป็น program ที่ทำให้สามารถเลือกระบบปฏิบัติการที่ลงไว้หรือ kernel เพื่อทำการโหลดตอนกระบวนการ boot และทำให้ user สามารถส่ง argument ไปยัง kernel ได้

# /dev

`/dev` จัดเก็บไฟล์พิเศษที่เกี่ยวข้องกับ device หรืออุปกรณ์ต่อพ่วงอื่นๆ โดยไฟล์ใน directory นี้จะเป็นเหมือน interface เพื่อให้สามารถ interact กับอุปกรณ์ hardware เช่น harddrive, CD-ROM, speakers, printers หรือ modems ได้

โดย `/dev` มี subdirectory อยู่หลายอัน ซึ่งแต่ละอันก็จัดเก็บไฟล์ device ที่เกี่ยวข้องกับ device บางแค่ประเภท อย่างเช่น `/dev/disk/id-by-uuid` จัดเก็บไฟล์ device สำหรับ harddisk ที่มีชื่อตาม universally unique identifier (UUID) ซึ่งไฟล์ device พวกนี้ใน subdirectory เป็น symbolic link ไปยังไฟล์ device ใน `/dev` ทำให้เราสามารถเข้าถึง device ได้โดยใช้ไฟล์ใน `/dev` หรือไฟล์ที่อยู่ใน `/dev/disk/id-by-uuid` เป็นต้น

ถ้าเราลอง ls -l เพื่อแสดงรายการไฟล์ใน /dev มา จะได้ผลลัพธ์ดังนี้ : 
```
crw-r--r-- 1 root root     10, 235 Feb  8 19:09 autofs
drwxr-xr-x 2 root root         320 Feb  8 19:14 block
drwxr-xr-x 2 root root          80 Feb  8 19:09 bsg
crw-rw---- 1 root disk     10, 234 Feb  8 19:09 btrfs-control
drwxr-xr-x 3 root root          60 Feb  8 19:09 bus
lrwxrwxrwx 1 root root           3 Feb  8 19:09 cdrom -> sr0
drwxr-xr-x 2 root root        3760 Feb  8 19:09 char
crw--w---- 1 root tty       5,   1 Feb  8 19:09 console
lrwxrwxrwx 1 root root          11 Feb  8 19:09 core -> /proc/kcore
drwxr-xr-x 6 root root         120 Feb  8 19:09 cpu
crw------- 1 root root     10, 124 Feb  8 19:09 cpu_dma_latency
crw------- 1 root root     10, 203 Feb  8 19:09 cuse
drwxr-xr-x 7 root root         140 Feb  8 19:09 disk
brw-rw---- 1 root disk    253,   0 Feb  8 19:09 dm-0
drwxr-xr-x 2 root root          60 Feb  8 19:09 dma_heap
drwxr-xr-x 3 root root         100 Feb  8 19:09 dri
crw------- 1 root root     10, 126 Feb  8 19:09 ecryptfs
crw-rw---- 1 root video    29,   0 Feb  8 19:09 fb0
lrwxrwxrwx 1 root root          13 Feb  8 19:09 fd -> /proc/self/fd
crw-rw-rw- 1 root root      1,   7 Feb  8 19:09 full
crw-rw-rw- 1 root root     10, 229 Feb  8 19:09 fuse
crw------- 1 root root    238,   0 Feb  8 19:09 hidraw0
crw------- 1 root root     10, 228 Feb  8 19:09 hpet
drwxr-xr-x 2 root root           0 Feb  8 19:09 hugepages
crw------- 1 root root     10, 183 Feb  8 19:09 hwrng
crw------- 1 root root     89,   0 Feb  8 19:09 i2c-0
lrwxrwxrwx 1 root root          12 Feb  8 19:09 initctl -> /run/initctl
drwxr-xr-x 4 root root         340 Feb  8 19:09 input
crw-r--r-- 1 root root      1,  11 Feb  8 19:09 kmsg
lrwxrwxrwx 1 root root          28 Feb  8 19:09 log -> /run/systemd/journal/dev-log
brw-rw---- 1 root disk      7,   0 Feb  8 19:09 loop0
brw-rw---- 1 root disk      7,   1 Feb  8 19:09 loop1
brw-rw---- 1 root disk      7,   2 Feb  8 19:09 loop2
brw-rw---- 1 root disk      7,   3 Feb  8 19:09 loop3
...
```
จะเห็นว่ามีบางไฟล์ที่ขึ้นว่าเป็นประเภท b (block) หรือ c (character) โดย device เหล่านี้จะมีเลขที่เกี่ยวข้องกับมันอยู่แทนที่จะเป็นขนาดไฟล์ ซึ่งเลขเหล่านี้เป็นตัวระบุ device ให้กับระบบ

 # Reference
 * https://access.redhat.com/documentation/th-th/red_hat_enterprise_linux/5/html/installation_guide/s1-grub-whatis
 * https://docs.oracle.com/en/operating-systems/oracle-linux/6/admin/ol_about_devices.html