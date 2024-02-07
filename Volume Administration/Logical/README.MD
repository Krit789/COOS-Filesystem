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
