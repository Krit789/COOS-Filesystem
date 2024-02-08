# Archiver Tool

### ตัวอย่าง คำสั่ง Archiver ของ  Linux ได้แก่:

| Command | Action   |
|----|---------------|
| [bzip2](#bzip2)	|บีบอัดไฟล์เพื่อเพิ่มพื้นที่ว่าง|
|[cpio](#cpio)	|สร้างและแตกไฟล์เก็บถาวร|
|[zcat](#zcat)	|แสดงเนื้อหาของไฟล์บีบอัด|
|[zforce](#zforce) |	บังคับให้ไฟล์ gzip มีนามสกุล .gz|
| [Tar](#Tar)|คัดลอกไฟล์ไปยังหรือกู้คืนไฟล์จากสื่อเก็บถาวร|
| [Gzip](#Gzip)|บีบอัดไฟล์เพื่อเพิ่มพื้นที่ว่าง|
| [Zip](#Zip)|ใช้เพื่อสร้าง จัดการ และแตกไฟล์|

# bzip2

bzip2: เครื่องมือบีบอัดไฟล์ที่มีประสิทธิภาพสำหรับชุดข้อมูลขนาดใหญ่
## คำสั่งหลัก
- bzip2: บีบอัดไฟล์และเพิ่มนามสกุล .bz2
- bunzip2: คลายการบีบอัดไฟล์ .bz2
- bzcat: ดูเนื้อหาของไฟล์ .bz2 โดยไม่ต้องคลายการบีบอัด
- bzip2recover: พยายามกู้คืนข้อมูลจากไฟล์ .bz2 ที่เสียหาย

#### คำสั่ง option ต่างๆ ของ bzip2

| option | Description          |
|-----------|---------------|
| dig  | ตั้งค่าขนาดบล็อกสำหรับการบีบอัด   |
| c | ส่งออกไปยังเอาต์พุตมาตรฐาน   |
|d | บังคับการคลายการบีบอัด|
| f  | เขียนทับไฟล์เอาต์พุต    |
| k  |เก็บไฟล์ต้นฉบับไว้     |
| l,V  | แสดงข้อมูลใบอนุญาตและเวอร์ชัน |
| q |แสดงเฉพาะข้อความสำคัญ |
|s| ใช้หน่วยความจำน้อยลง (ลดความเร็ว)|
|t| ตรวจสอบความสมบูรณ์ของไฟล์|
|v| โหมดแสดงรายละเอียด (แสดงอัตราส่วนการบีบอัด)|
|z| บังคับการบีบอัด|

## ตัวอย่าง

```shell
bzip2 large_dataset.txt
```
- บีบอัดไฟล์ large_dataset.txt
```shell
bunzip2 data.bz2
```
- คลายการบีบอัดไฟล์ data.bz2

```shell
bzcat compressed.bz2
```
- ดูเนื้อหาของไฟล์ compressed.bz2

# cpio

cpio: เครื่องมือคัดลอกไฟล์สำหรับเก็บถาวรและการถ่ายโอน

## mode
- -i, --extract: ดึงไฟล์ออกจากไฟล์เก็บถาวร
- -o, --create: สร้างไฟล์เก็บถาวร
- -p, --pass-through: คัดลอกไฟล์ภายในระบบเดียวกัน

## option ของ cpio

- -d, --make-directories: สร้างไดเรกทอรีที่จำเป็น
- -f, --nonmatching: คัดลอกไฟล์ที่ไม่ตรงกับรูปแบบที่กำหนด
- -F file, --file=file: ใช้ไฟล์ที่ระบุเป็นไฟล์เก็บถาวร
- - -H type, --format=type: กำหนดรูปแบบไฟล์เก็บถาวร
- -l, --link: สร้างลิงก์สัญลักษณ์แทนการคัดลอกไฟล์
- -m, --preserve-modification-time: รักษาเวลาแก้ไขไฟล์เดิม
- -t, --list: แสดงรายการไฟล์ในไฟล์เก็บถาวรโดยไม่สร้างไฟล์
- -u, --unconditional: เขียนทับไฟล์ที่มีอยู่
- -v, --verbose: แสดงรายละเอียดการทำงาน

## ตัวอย่าง

```shell
find . -name "*.old" | cpio -ocBv > archive.cpio
```
- เก็บถาวรไฟล์ที่ลงท้ายด้วย .old

```shell
cpio -icdv "*save*" < /dev/tape
```
- กู้คืนไฟล์จากเทปที่ชื่อมีคำว่า save

```shell
find . -depth | cpio -padm /destination/dir
```
- ย้ายไดเรกทอรี



# zcat

zcat: ดูเนื้อหาของไฟล์บีบอัดโดยไม่ต้องคลายการบีบอัด
ตัว
### Syntax zcat
```shell
zcat [options] [files]
```
### ตัวอย่าง 

```shell
zcat compressed_file.gz
```
- เพื่อดูเนื้อหาของไฟล์ compressed_file.gz

```shell
zcat file1.gz file2.gz
```
- เพื่อดูเนื้อหาของไฟล์บีบอัดหลายไฟล์
```shell
cat compressed_file.gz | zcat
```
- เพื่ออ่านข้อมูลจากstdin

# zforce
zforce: บังคับให้ไฟล์ gzip มีนามสกุล .gz

### Syntax zforce
```shell
zforce [files]
```
### ตัวอย่าง 
```shell
zcat compressed_file.gzzforce file1 file2
```
- เปลี่ยนชื่อไฟล์ file1 และ file2 เป็น file1.gz และ file2.gz ตามลำดับ

```shell
zforce *
```
- เปลี่ยนชื่อไฟล์ gzip ทั้งหมดในไดเรกทอรีปัจจุบัน

# Tar
tar: ใช้เพื่อสร้าง จัดการ และแยกไฟล์เก็บถาวร tar Tar ย่อมาจาก "tape archive" ซึ่งหมายถึงต้นกำเนิดของมันในฐานะเครื่องมือสำหรับการสำรองข้อมูลไปยังเทปไดรฟ์

#### Syntax of the tar Command
```shell
tar [options] [archive-file] [file or directory to be archived]
```

### Tars command
```shell
tar -cvf file.tar /path/to/folder 
```
- คำสั่งนี้สร้างไฟล์เก็บถาวร tar ชื่อ file.tar จากเนื้อหาของไดเร็กทอรีที่อยู่ที่ /path/to/folder
```shell
tar -xvf file.tar
```
 - คำสั่งนี้แยกเนื้อหาของไฟล์เก็บถาวร file.tar ลงในไดเร็กทอรีปัจจุบัน

```shell
tar -tvf file.tar 
```
- คำสั่งนี้แสดงรายการไฟล์และไดเร็กทอรีที่อยู่ในไฟล์เก็บถาวร file.tar

```shell
tar -rvf file.tar /path/to/new/file 
```
- คำสั่งนี้จะเพิ่มไฟล์ใหม่ชื่อ /path/to/new/file ต่อท้ายไฟล์ file.tar

```shell
tar -czvf file.tar.gz /path/to/folder 
```
- คำสั่งนี้สร้างไฟล์เก็บถาวร gzip ที่บีบอัดชื่อ file.tar.gz จากเนื้อหาของไดเร็กทอรีที่อยู่ที่ /path/to/folder

```shell
tar -cjvf file.tar.bz2 /path/to/folder 
```
- คำสั่งนี้สร้างไฟล์เก็บถาวร bzip2 ที่บีบอัดชื่อ file.tar.bz2 จากเนื้อหาของไดเร็กทอรีที่อยู่ที่ /path/to/folder

```shell
tar -xjvf file.tar.bz2 
```
- คำสั่งนี้จะแยกเนื้อหาของไฟล์เก็บถาวร file.tar.bz2 ที่บีบอัดลงในไดเร็กทอรีปัจจุบัน



#### คำสั่ง option ต่างๆ ของ Tar 
| option | Description          |
|-----------|---------------|
| -c  | สร้าง archive   |
| -x | แตกไฟล์จาก archive    |
|-f|Specifies the filename of the archive to be created or extracted. |
| -t  | แสดงรายการของไฟล์ที่มีทั้งหมดใน tar file       |
| -u  | เพิ่มเฉพาะไฟล์ที่มีการแก้ไขเข้าไปใน tar file     |
| -v  | สำหรับการแสดงข้อความ verbose ขณะทำงาน |
| -A | เพิ่มไฟล์ใหม่เข้าไปใน archive    |
|-z|ใช้การบีบอัด gzip เมื่อสร้างไฟล์ tar ส่งผลให้เกิดไฟล์บีบอัดที่มีนามสกุล '.tar.gz'|
|-j|ใช้การบีบอัด bzip2 เมื่อสร้างไฟล์ tar ส่งผลให้มีไฟล์บีบอัดที่มีนามสกุล '.tar.bz2'|
|-w|ตรวจสอบความสมบูรณ์ของไฟล์เก็บถาวร เพื่อให้แน่ใจว่าเนื้อหาจะไม่เสียหาย|
| -r  | เพิ่มไฟล์ใหม่โดยนำเอาไปต่อท้ายรายการของไฟล์ใน archive  |
|-d | หาความแตกต่างระหว่างไฟล์ใน archive และในไฟล์ต้นฉบับ|

# Gzip
gzip: ใช้เพื่อบีบอัดและขยายขนาดไฟล์โดยใช้อัลกอริธึมการบีบอัด gzip โดยทั่วไปแล้ว Gzip จะใช้ร่วมกับ tar เพื่อสร้าง tarball ซึ่งเป็นไฟล์เก็บถาวร tar ที่ถูกบีบอัดด้วย gzip


#### Syntax of the gzip Command
```shell
 gzip [Options] [filenames]
```
#### คำสั่ง option ต่างๆ ของ gzip 
| option | Description          |
|-----------|---------------|
| -f  | สร้าง archive   |
| -k | แตกไฟล์จาก archive    |
|-l | หาความแตกต่างระหว่างไฟล์ใน archive และในไฟล์ต้นฉบับ|
| -r  | เพิ่มเฉพาะไฟล์ที่มีการแก้ไขเข้าไปใน tar file     |
| -v  | แสดงรายการของไฟล์ที่มีทั้งหมดใน tar file       |
| -d  | ขยายขนาดไฟล์ที่ถูกบีบอัดโดยใช้คำสั่ง gzip |

## Gzip Commands:

```shell
gzip file.txt
```
- คำสั่งนี้จะบีบอัดไฟล์ชื่อ file.txt และแทนที่ด้วยไฟล์บีบอัดชื่อ file.txt.gz

```shell
gzip -k file.txt 
```
- คำสั่งนี้จะบีบอัดไฟล์ชื่อ file.txt และเก็บไฟล์ต้นฉบับไว้ โดยสร้างไฟล์บีบอัดใหม่ชื่อ file.txt.gz

```shell
gzip -d file.txt.gz 
```
- คำสั่งนี้จะแตกไฟล์ชื่อ file.txt.gz และแทนที่ด้วยไฟล์แตกไฟล์ที่เรียกว่า file.txt

```shell
gzip -c file.txt > file.txt.gz 
```
- คำสั่งนี้จะแตกไฟล์ชื่อ file.txt.gz และแทนที่ด้วยไฟล์แตกไฟล์ที่เรียกว่า file.txt

```shell
gunzip file.txt.gz 
```
- คำสั่งนี้จะบีบอัดไฟล์ชื่อ file.txt และสร้างไฟล์บีบอัดใหม่ชื่อ file.txt.gz โดยที่ยังคงเก็บไฟล์ต้นฉบับไว้

```shell
zcat file.txt.gz 
```
- คำสั่งนี้แสดงเนื้อหาของไฟล์บีบอัดชื่อ file.txt.gz บนเทอร์มินัลโดยไม่ต้องแตกไฟล์

```shell
zless file.txt.gz 
```
- คำสั่งนี้ช่วยให้คุณดูเนื้อหาของไฟล์บีบอัดที่เรียกว่า file.txt.gz ในลักษณะที่เป็นมิตรต่อผู้ใช้มากขึ้น

# Zip
zip: ใช้เพื่อสร้าง จัดการ และแตกไฟล์ zip Zip เป็นรูปแบบไฟล์เก็บถาวรยอดนิยมที่ได้รับการสนับสนุนโดยระบบปฏิบัติการหลายระบบ
## Syntax Zip
```shell
zip [options] [file_name.zip] [files_names]
```
### The syntax for Creating a zip file:
```shell
zip [file_name.zip] [file_name]
```

## Zip Commands:

```shell
zip archive.zip file1.txt file2.txt
```
- คำสั่งนี้สร้างไฟล์ zip ใหม่ชื่อ archive.zip และเพิ่มสองไฟล์ชื่อ file1.txt และ file2.txt เข้าไป


```shell
zip -r archive.zip folder/
```
- คำสั่งนี้สร้างไฟล์ zip ใหม่ที่เรียกว่า archive.zip และเพิ่มไฟล์และไดเร็กทอรีย่อยทั้งหมดที่อยู่ในโฟลเดอร์/ ไดเร็กทอรีลงไป

```shell
unzip archive.zip
```
- คำสั่งนี้แยกเนื้อหาของไฟล์ archive.zip ลงในไดเร็กทอรีปัจจุบัน

```shell
unzip archive.zip -d /path/to/directory
```
- คำสั่งนี้แยกเนื้อหาของไฟล์ archive.zip ลงในไดเร็กทอรีที่ระบุซึ่งอยู่ที่ /path/to/directory

```shell
zip -d archive.zip file.txt
```
- คำสั่งนี้จะลบไฟล์ชื่อ file.txt ออกจากไฟล์ archive.zip

```shell
zip -u archive.zip file.txt
```
- คำสั่งนี้อัปเดตไฟล์ archive.zip ที่มีอยู่โดยการเพิ่มหรือแทนที่ไฟล์ชื่อ file.txt

```shell
zip -l archive.zip
```
- คำสั่งนี้แสดงรายการเนื้อหาของไฟล์ archive.zip

#### คำสั่ง option ต่างๆ ของ Tar 

| option | Description    |
|----|---------------|
| -d  |ลบไฟล์ออกจากไฟล์เก็บถาวร|
| -u| แตกไฟล์จาก archive    |
|-m|Specifies the filename of the archive to be created or extracted. |
| -r  | แสดงรายการของไฟล์ที่มีทั้งหมดใน tar file       |
| -x  | เพิ่มเฉพาะไฟล์ที่มีการแก้ไขเข้าไปใน tar file     |
| -v  |เปิดใช้งานโหมด verbose โดยละเอียดเกี่ยวกับโครงสร้างไฟล์ zip เมื่อใช้เพียงอย่างเดียว  |

### Reference:
```
 https://medium.com/@cuncis/linux-archiving-commands-a-beginners-guide-to-efficient-file-management-8448f7403e95 
 https://www.geeksforgeeks.org/tar-command-linux-examples/ 
 https://www.geeksforgeeks.org/gzip-command-linux/ 
 https://www.geeksforgeeks.org/zip-command-in-linux-with-examples/

```


