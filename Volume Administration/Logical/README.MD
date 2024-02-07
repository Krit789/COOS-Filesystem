# Logical Volume Administration

พื้นที่จัดเก็บข้อมูลเสมือนที่สร้างขึ้นบนอุปกรณ์จัดเก็บข้อมูลทางกายภาพ เช่น ฮาร์ดดิสก์หรือ SSD โดยใช้ Logical Volume Manager (LVM) 

> [!NOTE]
>LVM ทำหน้าที่เหมือนซอฟต์แวร์ abstraction layer ช่วยให้ผู้ใช้สามารถรวมอุปกรณ์จัดเก็บข้อมูลหลายตัวเข้าด้วยกันเป็นกลุ่มวอลุ่ม (Volume Group) และแบ่งกลุ่มวอลุ่มนั้นออกเป็นลอจิคัลโวลุ่มย่อย ๆ 

**คุณสมบัติของ Logical Volume:**

- **ขนาด:** สามารถกำหนดขนาดได้ตามต้องการ ไม่จำเป็นต้องตรงกับขนาดของอุปกรณ์จัดเก็บข้อมูลทางกายภาพ
-  **ความยืดหยุ่น:** สามารถปรับขนาด เพิ่ม หรือลด พื้นที่จัดเก็บข้อมูลของลอจิคัลโวลุ่มได้แบบไดนามิก
- **การจัดการ:** จัดการง่ายเหมือนกับดิสก์ทั่วไป
- **ประสิทธิภาพ:** เพิ่มประสิทธิภาพการใช้งานพื้นที่จัดเก็บข้อมูล
- **ความปลอดภัย:** รองรับ RAID เพื่อป้องกันข้อมูลสูญหาย

**ประโยชน์ของการใช้ Logical Volume:**
- **จัดการพื้นที่จัดเก็บข้อมูลได้อย่างมีประสิทธิภาพ:** ผู้ใช้สามารถจัดสรรพื้นที่จัดเก็บข้อมูลได้อย่างลงตัวตามความต้องการ
- **เพิ่มความยืดหยุ่น:** ปรับขนาดพื้นที่จัดเก็บข้อมูลได้โดยไม่ต้องลงทุนซื้ออุปกรณ์ใหม่
- **เพิ่มความปลอดภัย:** ข้อมูลมีความปลอดภัยมากขึ้นด้วย RAID
- **ง่ายต่อการจัดการ:** 
จัดการลอจิคัลโวลุ่มได้ง่ายเหมือนกับดิสก์ทั่วไป

![image](https://dextutor.com/wp-content/uploads/2021/05/image-34.png)

รูปจาก: [https://dextutor.com/logical-volume-management-in-linux/](https://dextutor.com/logical-volume-management-in-linux/)

---
## การสร้าง Linear Logical Volumes

`lvcreate` ใช้สำหรับสร้างลอจิคัลโวลุ่ม (Logical Volume) ในระบบ LVM 

```
lvcreate -n my_logical_volume my_volume_group
```

**ตัวอย่างการใช้งาน**
```
lvcreate -n my_lv my_vg 10G
```
- จะสร้างลอจิคัลโวลุ่มชื่อ `my_lv` ขนาด 10GB ภายในกลุ่มวอลุ่ม `my_vg`
- ตัวเลือก `-n` ระบุชื่อลอจิคัลโวลุ่ม 
- หากไม่ระบุชื่อ ลอจิคัลโวลุ่มจะถูกตั้งชื่อเป็น `lvol#` โดย `#` คือหมายเลขภายใน



**สร้าง logical volume ขนาด 10GB ใน volume group ชื่อ `vg1`:**
```
lvcreate -L 10G vg1
```
**สร้าง logical volume ชื่อ `testlv` ขนาด 1500MB ใน volume group ชื่อ `testvg`:**
```
lvcreate -L 1500 -n testlv testvg
```
**สร้าง logical volume ชื่อ `gfslv` ขนาด 50GB ใน volume group ชื่อ `vg0`:**
```
lvcreate -L 50G -n gfslv vg0
```

**ตัวเลือกในการกำหนดขนาด logical volume:**
- ใช้ตัวอักษร 'L' ตามด้วยขนาด: ระบุขนาดเป็น MB, GB, TB (เช่น `-L 10G`)
- ใช้ตัวเลือก '-l' ตามด้วยจำนวน extent: ระบุขนาดเป็นจำนวน extent (หน่วยจัดเก็บข้อมูลย่อยใน LVM)
- ใช้ตัวเลือก '-l' ตามด้วยเปอร์เซ็น: ระบุขนาดเป็นเปอร์เซ็นของ volume group หรือ physical volume เช่น
    - `-l 60%VG`: ใช้ 60% ของพื้นที่ทั้งหมดใน volume group
    - `-l 100%FREE`: ใช้พื้นที่ว่างทั้งหมดใน volume group

หนึ่งในการสร้าง logical volume ที่ใช้ทั้งหมดของ volume group คือใช้ `vgdisplay` เพื่อหาขนาด "Total PE" และนำผลลัพธ์เหล่านั้นมาใช้เป็นอินพุตให้กับคำสั่ง `lvcreate`
```
# vgdisplay testvg | grep "Total PE"
Total PE              10230
# lvcreate -l 10230 -n mylv testvg
```
- ใช้คำสั่ง `vgdisplay testvg` เพื่อแสดงข้อมูลของกลุ่มวอลุ่ม `testvg`
- ใช้ตัวกรอง `grep "Total PE"` เพื่อดึงเฉพาะบรรทัดที่แสดงขนาดทั้งหมด (Total PE)
- ใช้คำสั่ง `lvcreate -l 10230 -n mylv testvg`
ตัวเลือก `-l 10230` ระบุขนาดเท่ากับค่า "Total PE"


**การระบุ physical volume สำหรับสร้าง logical volume**

ระบุ physical volume ต่อท้ายคำสั่ง lvcreate เช่น
```
lvcreate -L 1500 -n testlv testvg /dev/sdg1
```
- สร้างโลจิคัลวอลุ่มชื่อ `testlv` ใน volume group `testvg` ที่จัดสรรจาก physical volume `/dev/sdg1`

ระบุ extent ที่ต้องการใช้บน physical volume เช่น
```
lvcreate -l 100 -n testlv testvg /dev/sda1:0-24 /dev/sdb1:50-124
```
- สร้างขอบเขต 0 ถึง 24 ของ physical volume /dev/sda1 และขอบเขต 50 ถึง 124 ของ physical volume /dev/sdb1 volume group `testvg`
> [!TIP]
> การจัดสรร extent:
>- กำหนดด้วยตัวเลือก '-alloc' เช่น `-alloc anywhere` (จัดสรร extent บน physical volume ใดก็ได้), `-alloc contiguous` (จัดสรร extent ต่อเนื่องกัน)
>- เปลี่ยนนโยบายภายหลังด้วยคำสั่ง `lvchange`

---
## การสร้าง Striped Volumes
การสร้าง Striped Logical Volume สำหรับการอ่านและเขียนข้อมูลขนาดใหญ่แบบต่อเนื่อง (Sequential I/O) ช่วย เพิ่มประสิทธิภาพ การเข้าถึงข้อมูลของ LVM 

![Image](https://wadhahdaouehi.tn/wp-content/uploads/2013/10/LVM-Striping.png)

รูปจาก: [https://wadhahdaouehi.tn/2013/10/lvm-striping-improve-the-efficiency-of-the-data-io/](https://wadhahdaouehi.tn/2013/10/lvm-striping-improve-the-efficiency-of-the-data-io/
)

**หลักการทำงาน:**
-  Logical Volume แบบ striped แบ่งข้อมูลออกเป็นชิ้นเล็กๆ (stripes) และกระจายไปยัง physical volume ต่างๆ ภายใน Volume Group
- เมื่ออ่านหรือเขียนข้อมูล จะทำการเข้าถึงหลาย physical volume พร้อมกัน ช่วยเพิ่มความเร็วในการเข้าถึงข้อมูล โดยเฉพาะการอ่านและเขียนขนาดใหญ่

**ข้อดีของ Logical Volume แบบ striped:**
- เพิ่มความเร็วในการอ่านและเขียนข้อมูลขนาดใหญ่: เนื่องจากสามารถเข้าถึงหลาย physical volume พร้อมกัน
- ปรับปรุงอัตราการส่งข้อมูล (Throughput): ช่วยให้ระบบรองรับปริมาณข้อมูลที่เข้าออกได้มากขึ้น
- ปรับสมดุลการใช้งาน physical volume: ช่วยกระจายการใช้งานไปยัง physical volume ต่างๆ ลดภาระของ physical volume ใดๆ เครื่องเดียว

> [!TIP]
> - ไม่เหมาะสำหรับการเข้าถึงข้อมูลแบบสุ่ม (Random I/O) เพราะการกระจายข้อมูลทำให้การค้นหาช้าลง
>- ประสิทธิภาพขึ้นอยู่กับ จำนวน physical volume ในกลุ่มวอลุ่ม ยิ่งมีจำนวนมาก ยิ่งเร็ว
>- ความซับซ้อนในการจัดการระบบเพิ่มขึ้นเล็กน้อย

**เหมาะสำหรับสถานการณ์ที่:**
- ต้องการอ่านและเขียนข้อมูลขนาดใหญ่เป็นหลัก
- มี physical volume หลายตัวในกลุ่มวอลุ่ม
- ไม่เน้นการเข้าถึงข้อมูลแบบสุ่ม


### การสร้าง Logical Volume แบบ striped ใน LVM
- ใช้คำสั่ง `lvcreate` พร้อมตัวเลือก `-i` ระบุจำนวน stripes สำหรับแบ่งข้อมูล
- จำนวน stripes กำหนดจำนวน physical volume ที่ใช้ในการกระจายข้อมูล
- โดยปกติ จำนวน stripes ต้องไม่เกินจำนวน physical volume ในกลุ่มวอลุ่ม (ยกเว้นกรณีใช้ `--alloc anywhere`)

**ขนาดสูงสุด Logical Volume แบบ striped**
- ขึ้นอยู่กับขนาดของ physical volume ที่เล็กที่สุด
- ตัวอย่าง:
    - Stripe 2 legs: ขนาดสูงสุด = 2 * ขนาด physical volume ที่เล็กที่สุด
    - Stripe 3 legs: ขนาดสูงสุด = 3 * ขนาด physical volume ที่เล็กที่สุด

**ตัวอย่างการใช้งาน**
```
lvcreate -L 10G -i 3 -n mylv vg1
```
- สร้าง logical volume ชื่อ mylv ขนาด 10GB
- แบ่งข้อมูลเป็น 3 stripes (กระจายไป 3 physical volume)

```
lvcreate -L 50G -i 2 -I 64 -n gfslv vg0
```
- สร้าง logical volume ชื่อ `gfslv` ขนาด 50GB
- แบ่งข้อมูลเป็น 2 stripes (กระจายไป 2 physical volume)
- ขนาด stripe 64 กิโลไบต์
```
lvcreate -l 100 -i 2 -n stripelv testvg /dev/sda1:0-49 /dev/sdb1:50-99
```
- สร้าง logical volume ชื่อ `stripelv` ขนาด 100 extent
- แบ่งข้อมูลเป็น 2 stripes (กระจายไป 2 physical volume)
- ใช้เฉพาะ sector 0-49 ของ /dev/sda1 และ sector 50-99 ของ /dev/sdb

>[!TIP]
>- ตัวเลือก `-i` ระบุจำนวน stripes
>- ตัวเลือก `-I` ระบุขนาด stripe (ค่าดีฟอลต์คือ 64KB)
>- ตัวเลือก `-n` ระบุชื่อลอจิคัลโวลุ่ม
>- สามารถระบุ extent หรือ sector ของ physical volume สำหรับการสร้าง stripe
---

## RAID Logical Volumes

เป็นวิธีการสร้าง Logical Volume บนระบบ LVM โดยใช้เทคโนโลยี RAID เพื่อเพิ่มประสิทธิภาพ ความทนทาน หรือทั้งสองอย่างให้กับระบบจัดเก็บข้อมูลของคุณ

> [!NOTE]
> โปรแกรม RAID logical volumes ไม่สามารถรับรู้ถึงการทำงานแบบคลัสเตอร์ได้ หรือคือ RAID logical volumes สามารถสร้างและเปิดใช้งานได้เฉพาะบนเครื่องเดียวเท่านั้น แต่ไม่สามารถเปิดใช้งานพร้อมกันบนเครื่องมากกว่าหนึ่งเครื่องได้ หากคุณต้องการมีโปรแกรม mirrored volumes ที่ไม่จำเป็นต้องใช้งานแบบเฉพาะเครื่อง คุณต้องสร้างโปรแกรมด้วยชนิดของ mirror segment

**หลักการทำงาน:**

- RAID Logical Volumes แบ่งข้อมูลออกเป็นส่วนเล็กๆ (stripes) กระจายไปยัง Physical Volume หลายตัว
- การกระจายข้อมูลช่วยเพิ่มความเร็วในการอ่านเขียนข้อมูล (สำหรับ RAID ประเภท Stripe)
- RAID Logical Volumes ยังมีรูปแบบอื่นๆ ที่เน้นความทนทานต่อความเสียหายของอุปกรณ์ (เช่น RAID Mirrored)
- ผู้ใช้สามารถเลือกประเภท RAID ที่เหมาะสมกับความต้องการใช้งาน

**ข้อดี:**

- **เพิ่มความเร็ว:** RAID ประเภท Stripe เพิ่มความเร็วในการอ่านเขียนข้อมูล
- **เพิ่มความทนทาน:** RAID ประเภท Mirrored และ RAID 5/6 ช่วยให้กู้คืนข้อมูลได้กรณีอุปกรณ์เสียหาย
- **เพิ่มความยืดหยุ่น:** ผู้ใช้สามารถเลือกประเภท RAID ที่เหมาะสมกับความต้องการ
- **เพิ่มประสิทธิภาพการใช้พื้นที่:** RAID 5/6 ช่วยให้ใช้พื้นที่จัดเก็บข้อมูลได้อย่างมีประสิทธิภาพ

**ข้อเสีย:**

- **ความซับซ้อน:** การจัดการ RAID Logical Volumes ซับซ้อนกว่า Logical Volume ทั่วไป
- **ค่าใช้จ่าย:** RAID Logical Volumes อาจใช้ Physical Volume มากกว่า Logical Volume ทั่วไป


>[!TIP]
>- Mirrored volumes with mirror segment type: ประเภทนี้จะสร้างสำเนาของข้อมูลไว้บน physical volume อีกเครื่องหนึ่งภายในคลัสเตอร์ ช่วยให้สามารถเข้าถึงข้อมูลได้จากหลายเครื่อง
>- ตัวอย่างระบบที่ใช้ mirrored volumes: ระบบฐานข้อมูลแบบคลัสเตอร์, ระบบไฟล์แชร์แบบคลัสเตอร์
>- **ข้อควรระวัง:** การใช้ mirrored volumes ต้องมีการกำหนดคอนฟิกuration และการจัดการระบบคลัสเตอร์ที่เหมาะสม

คำสั่ง `lvcreate` ใน LVM สามารถสร้างประเภท RAID ได้ โดยระบุประเภท RAID ที่ต้องการด้วยตัวเลือก `--type` 

ตาราง 4.1 "RAID Segment Types" ในเอกสารอ้างอิง LVM อธิบายรายละเอียดเกี่ยวกับประเภท RAID ที่รองรับ

ประเภท Segment  | คำอธิบาย          |         |
----------------| ---------------- |--------|
raid0/raid0_meta| แบ่งข้อมูลลอจิคัลโวลุ่มออกเป็นชิ้นเล็กๆ (stripes) กระจายไปยัง subvolume ข้อมูลหลายตัว ช่วยเพิ่มประสิทธิภาพการอ่านเขียนข้อมูล| ![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/RAID_0.svg/300px-RAID_0.svg.png)|
raid1           | การทำมิเรอร์ข้อมูลบน RAID1 นี่เป็นค่าเริ่มต้นของตัวเลือก `--type` ในคำสั่ง `lvcreate` เมื่อคุณระบุ `-m` แต่ไม่ได้ระบุ striping1 | ![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/RAID_1.svg/300px-RAID_1.svg.png)| 
raid4 | RAID4 ใช้ดิสก์พิเศษสำหรับข้อมูล parity เพื่อกู้คืนข้อมูลกรณีมีอุปกรณ์เสียหาย 1 ตัว|![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ad/RAID_4.svg/600px-RAID_4.svg.png) |
raid5 | เหมือนกับ raid5_ls | ![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/6/64/RAID_5.svg/600px-RAID_5.svg.png)|
raid5_la | RAID5 แบบไม่สมมาตรด้านซ้าย หมุน parity 0 ด้วยการต่อเนื่องของข้อมูล |
raid5_ra	|RAID5 แบบไม่สมมาตรด้านขวา หมุน parity N ด้วยการต่อเนื่องของข้อมูล|
raid5_ls	|RAID5 แบบสมมาตรด้านซ้าย หมุน parity 0 ด้วยการเริ่มต้นข้อมูลใหม่|
raid5_rs|	RAID5 แบบสมมาตรด้านขวา หมุน parity N ด้วยการเริ่มต้นข้อมูลใหม่|
raid6	|เหมือนกับ raid6_zr| ![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/RAID_6.svg/600px-RAID_6.svg.png)|
raid6_zr	|RAID6 เริ่มต้นที่ศูนย์ หมุน parity ศูนย์ (จากซ้ายไปขวา) ด้วยการเริ่มต้นข้อมูลใหม่|
raid6_nr	|RAID6 เริ่มต้นที่ N หมุน parity N (จากซ้ายไปขวา) ด้วยการเริ่มต้นข้อมูลใหม่|
raid6_nc	|RAID6 ต่อเนื่องที่ N หมุน parity N (จากซ้ายไปขวา) ด้วยการต่อเนื่องของข้อมูล|
raid10 | Striped mirrors นี่เป็นค่าเริ่มต้นของตัวเลือก `--type` ในคำสั่ง `lvcreate` ถ้าคุณระบุ -m และระบุจำนวน stripe มากกว่า 1


รูปจาก: [https://en.wikipedia.org/wiki/Standard_RAID_levels](https://en.wikipedia.org/wiki/Standard_RAID_levels)

ผู้ใช้ส่วนใหญ่ ไม่จำเป็นต้องใช้ประเภท RAID ที่ซับซ้อน เพียงแค่เลือกหนึ่งใน 5 ประเภทหลัก (`raid1`, `raid4`, `raid5`, `raid6`, `raid10`) ก็เพียงพอ

LVM สร้าง subvolume เมตาดาตา เมื่อสร้างลอจิคัลโวลุ่มแบบ RAID จำนวน  metadata subvolume จะเท่ากับจำนวน subvolume ข้อมูลหรือ parity ในอาร์เรย์

**ตัวอย่างการใช้งาน:**

สร้าง RAID1 แบบ 2 ทาง จะได้ metadata subvolume จ 2 ตัว (`lv_rmeta_0 และ lv_rmeta_1`) และ subvolume ข้อมูล 2 ตัว (`lv_rimage_0 และ lv_rimage_1`)

คำสั่งตัวอย่าง: สร้าง RAID1 แบบ 2 ทาง ชื่อ `my_lv` ขนาด 1GB ในกลุ่มวอลุ่ม `my_vg`

```
lvcreate --type raid1 -m 1 -L 1G -n my_lv my_vg
 ```
- สร้าง RAID1 ด้วยจำนวนสำเนาต่างๆ: ใช้ตัวเลือก `-m`ระบุจำนวนสำเนา
- สร้าง RAID4/5/6 ด้วยจำนวน stripes: ใช้ตัวเลือก `-i` ระบุจำนวน stripes
- ระบุขนาด stripe: ใช้ตัวเลือก `-I` ระบุขนาด stripe

ตัวอย่าง:
สร้าง RAID5 (3 stripes + 1 parity) ขนาด 1GB ชื่อ `my_lv`

```
lvcreate --type raid5 -i 3 -L 1G -n my_lv my_vg
```

ตัวอย่าง: สร้างอาร์เรย์ RAID6 ชื่อ `my_lv` ในกลุ่มวอลุ่ม `my_vg` ขนาด 1 กิกะไบต์ โดยมี stripes 3 สไตรป์และไดรฟ์ parity 2 ไดรฟ์ (คำนวณโดยอัตโนมัติ)

```
lvcreate --type raid6 -i 3 -L 1G -n my_lv my_vg
```
>[!TIP]
>- LVM จะคำนวณจำนวนดิสก์ parity ที่เหมาะสมโดยอัตโนมัติ
>- จำนวน stripes และขนาด stripe ส่งผลต่อประสิทธิภาพและการใช้พื้นที่จัดเก็บ
>- ควรเลือกค่าที่เหมาะสมกับความต้องการใช้งาน

**การจัดการ RAID Logical Volumes in LVM:**

- หลังจากสร้าง RAID logical volume แล้ว คุณสามารถใช้งานได้เหมือนกับ LVM logical volume ทั่วไป เช่น เปิดใช้งาน (activate) เปลี่ยนแปลง ลบ แสดงข้อมูล และใช้งานข้อมูลภายใน

>[!WARNING]
>การสร้าง RAID10 logical volume หลายชุดพร้อมกัน อาจส่งผลต่อประสิทธิภาพของการอัพเดทเมตาดาตาของกลุ่มวอลุ่ม เนื่องจากต้องใช้ทรัพยากร I/O พื้นหลัง (background I/O) จำนวนมากในการซิงค์ข้อมูลเบื้องต้น

### ควบคุมอัตราการเริ่มต้นใช้งาน RAID Logical Volumes ด้วย Recovery Throttling:

- ใช้ตัวเลือก `--minrecoveryrate` และ `--maxrecoveryrate` ในคำสั่ง `lvcreate`
- `--maxrecoveryrate`: กำหนดอัตราสูงสุดของการซิงค์ข้อมูล (ไม่ให้กระทบกับการใช้งานปกติ) หน่วยเป็น KiB/วินาที/อุปกรณ์
- `--minrecoveryrate`: กำหนดอัตราต่ำสุดของการซิงค์ข้อมูล (การันตีความเร็วขั้นต่ำ) หน่วยเป็น KiB/วินาที/อุปกรณ์


>[!TIP]
>Recovery Throttling: ช่วยควบคุมการใช้ทรัพยากร I/O สำหรับการเริ่มต้นใช้งาน RAID logical volume

**ตัวอย่างการใช้งาน:**

สร้าง RAID10 ขนาด 10GB (2 stripes, 1 mirror) อัตราสูงสุด 128 KiB/วินาที/อุปกรณ์
```
lvcreate --type raid10 -i 2 -m 1 -L 10G --maxrecoveryrate 128 -n my_lv my_vg
```
---
## การสร้าง Mirrored Volumes

 คือ เทคโนโลยีการจัดเก็บข้อมูลใน LVM (Logical Volume Manager) ที่ทำสำเนาข้อมูลของคุณไปยังดิสก์อย่างน้อยสองตัว  เปรียบเสมือนการ "ทำกระจก" ข้อมูลของคุณไว้

 - Mirrored Volumes ไม่รองรับการใช้งานแบบคลัสเตอร์ (cluster-aware)
- LVM รองรับการสร้าง Mirrored Volumes แบบ RAID 1/4/5/6/10
- สามารถปรับแต่งขนาด, จำนวนสำเนา, และอื่นๆ ด้วยคำสั่ง lvcreate


> [!NOTE]
Mirrored Volume เหมาะสำหรับ storing ข้อมูลสำคัญที่ต้องการความทนทานต่อความผิดพลาดสูง เช่น ฐานข้อมูล ระบบปฏิบัติการ ไฟล์สำคัญ

![Image](https://www.easeus.com/images/en/data-recovery/drw-pro//raid-1-image.png)

รูปจาก: [https://www.easeus.com/images/en/data-recovery/drw-pro//raid-1-image.png](https://www.easeus.com/images/en/data-recovery/drw-pro//raid-1-image.png)

**วิธีการทำงาน:**

- ข้อมูลทั้งหมดที่เขียนลงใน Mirrored Volume จะถูกเขียนไปยังดิสก์ทั้งสองตัวพร้อมกัน
- ดิสก์แต่ละตัวใน Mirrored Volume เรียกว่า "Mirror"
- ในกรณีที่ดิสก์หนึ่งตัวเกิดความเสียหาย ข้อมูลจะยังคงสามารถอ่านได้จากดิสก์อีกตัว

**ข้อดี:**

- เพิ่มความทนทานต่อความผิดพลาด: Mirrored Volume ช่วยให้มั่นใจได้ว่าข้อมูลของคุณจะปลอดภัย even though ดิสก์หนึ่งตัวอาจเสียหาย
- เพิ่มประสิทธิภาพการอ่าน: การอ่านข้อมูลสามารถทำได้จากดิสก์ทั้งสองตัวพร้อมกัน

**ข้อเสีย:**

- ใช้พื้นที่จัดเก็บข้อมูลมากขึ้น: Mirrored Volume จำเป็นต้องใช้พื้นที่ดิสก์สองเท่าของข้อมูลจริง
- ค่าใช้จ่ายเพิ่มเติม: จำเป็นต้องมีดิสก์เพิ่มเติมสำหรับ Mirrored Volume


### การสร้าง Mirrored Volumes:

- ระบุประเภท `mirror` ด้วย `--type mirror`
- กำหนดขนาดด้วย `-L` (หน่วยเป็น GB, MB, KiB)
- กำหนดจำนวนสำเนาด้วย `-m` (1 สำเนา = Mirrored Volume 2 ดิสก์)
- ตั้งชื่อด้วย `-n`
- ระบุกลุ่มวอลุ่มด้วย `vg0`

**ตัวอย่างการใช้งาน:**
```
lvcreate --type mirror -L 50G -m 1 -n mirrorlv vg0
```

> [!TIP]
>- LVM แบ่งข้อมูลเป็นส่วนย่อย (regions) ขนาด 512KB (ปรับแต่งได้ด้วย `-R`)
>- การสร้าง Mirrored Volumes ใหม่ ไม่จำเป็นต้องซิงค์ข้อมูลข้ามดิสก์ ใช้ `--nosync`
>- LVM บันทึกสถานะการซิงค์ข้อมูลด้วย log บนดิสก์หรือในหน่วยความจำ
>- สามารถกำหนดดิสก์และตำแหน่งที่ใช้สร้าง Mirrored Volumes ได้
>- รวมการแบ่งข้อมูล (striping) กับการทำมิเรอร์ในลอจิคัลโวลุ่มเดียวได้ (`--mirrors` และ `--stripes`)

