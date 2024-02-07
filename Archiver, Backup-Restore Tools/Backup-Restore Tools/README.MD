# Backup/Restore Tools

## ประเภทของ Backup:

- Full backup: สำรองข้อมูลทั้งหมดในระบบ
- Incremental backup: สำรองข้อมูลเฉพาะส่วนที่เปลี่ยนแปลงหลังจาก full backup ครั้งล่าสุด
- Differential backup: สำรองข้อมูลทั้งหมดที่เปลี่ยนแปลงหลังจาก full backup ครั้งล่าสุด

## วิธีการ Backup:

- Local backup: สำรองข้อมูลไปยังอุปกรณ์เก็บข้อมูลภายใน เช่น ฮาร์ดดิสก์
- Remote backup: สำรองข้อมูลไปยังอุปกรณ์เก็บข้อมูลภายนอก เช่น เซิร์ฟเวอร์ FTP หรือ cloud storage

## เครื่องมือ Backup:
- tar: เครื่องมือพื้นฐานสำหรับการ backup และ restore ไฟล์
- cpio: เครื่องมือที่คล้ายกับ tar แต่มีประสิทธิภาพมากกว่า
- rsync: เครื่องมือสำหรับการสำรองข้อมูลแบบ incremental และ differential
- dump: เครื่องมือสำหรับการสำรองข้อมูลระบบไฟล์

## การ Restore ข้อมูล:

- การ Restore จาก full backup: คืนค่าระบบทั้งหมดจาก backup
- การ Restore จาก incremental backup: คืนค่าระบบจาก full backup และ incremental backup ล่าสุด
- การ Restore จาก differential backup: คืนค่าระบบจาก full backup และ differential backup ล่าสุด

## ตัวอย่างการใช้งาน 


```shell
tar -cf etc.tar /etc
```
- การ backup ไฟล์


```shell
tar -xf etc.tar
```
- การ restore ไฟล์


```shell
dump -0uf /dev/tape /
```
- การ backup ระบบไฟล์ทั้งหมด:


```shell
restore -rf /dev/tape
```
- การ restore ระบบไฟล์ทั้งหมด: