# Archiver Tool

### ตัวอย่างทั่วไปของคำสั่งเก็บถาวรใน Linux ได้แก่:






rar: ใช้เพื่อสร้าง จัดการ และแตกไฟล์ rar Rar เป็นรูปแบบไฟล์เก็บถาวรที่เป็นกรรมสิทธิ์ซึ่งรองรับการบีบอัด การขยาย และการกู้คืนข้อผิดพลาด

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



<!-- https://medium.com/@cuncis/linux-archiving-commands-a-beginners-guide-to-efficient-file-management-8448f7403e95 -->
<!-- https://www.geeksforgeeks.org/tar-command-linux-examples/ -->
<!-- https://www.geeksforgeeks.org/gzip-command-linux/ -->
<!-- https://www.geeksforgeeks.org/zip-command-in-linux-with-examples/ -->