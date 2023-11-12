# IoT-ESP32-LabSheet-02
# โปรแกรมไฟวิ่ง 8 ดวง
## [(กลับไปหน้าที่แล้ว)](./Chasing-LED-V3.md

## 6. Challenge

### เขียนโปรแกรมไฟวิ่ง ให้มีลักษณะที่ต่างออกไปจากตัวอย่าง เช่น
   
*วิ่งไปสุดปลายทางแล้ววิ่งกลับ (ping-pong)*
```
 1    -------0
 2    ------0-
 3    -----0--
 4    ----0---
 5    ---0----
 6    --0-----
 7    -0------
 8    0-------
 9    -0------
10    --0-----
11    ---0----
12    ----0---
13    -----0--
14    ------0-
15    -------0
16    ------0-
...
```

*ยืด-หด*
```
 1    -------0
 2    ------00
 3    -----000
 4    ----0000
 5    ---00000
 6    --000000
 7    -0000000
 8    00000000
 9    -0000000
10    --000000
11    ---00000
12    ----0000
13    -----000
14    ------00
15    -------0
16    ------00
```

*ขยายออก*
```
 1    ---00---
 2    --0--0--
 3    -0----0-
 4    0------0
 5    -0----0-
 6    --0--0--
 7    ---00---
 8    --0--0--
```


*ยุบเข้า*
```
 1    0------0
 2    -0----0-
 3    --0--0--
 4    ---00---
 5    --0--0--
 6    -0----0-
 7    0------0
 8    -0----0-
```

*แบบอื่น ๆ ตามจินตนาการ*

---

### การส่งงาน
- Fork repo ของ Lab ไปที่ account ส่วนตัว
- เพิ่ม folder ใน repo ที่ fork ไป โดยตั้งชื่อ folder เป็นรหัสนักศึกษา
- ใน folder ที่สร้างขึ้น ให้เพิ่มไฟล์สำหรับส่งงานโดยใช้ ชื่อ <รหัสนักศึกษา>-Lab-02.md
- ถ้ามีรูปภาพ ให้สร้าง folder ย่อยชื่อ Pictures แล้วใส่ภาพไว้ในนั้น
- ถ้ามีวิดีโอแสดงการทำงานให้ upload ขึ้น youtube โดยใส่ Link ในไฟล์ md
- เมื่อทำงานเสร็จ ให้ส่งทาง  pull request 

รูปแบบ folder ของ repo ที่สามารถทำ pull request

```
    [Lab-repo-name]
    +-[6XXXXXXX]             (สร้าง folder ด้วยรหัสนักศึกษา เพื่อไม่ให้ไฟล์ทับกับเพื่อน)
      |
      +--6XXXXXXX-Lab-02.md  (ไฟล์รายงาน ต้องมีทุกคน) 
      +--[Pictures]          (รูปภาพ option)
      |  |
      |  +--pic1.png
      |  +--pic2.png 
      |  +--...
      |    
      +--others-files

```
# 64030071 Tanapat Pluemchai
วิ่งไปสุดปลายทางแล้ววิ่งกลับ (ping-pong)
```
 1    -------0
 2    ------0-
 3    -----0--
 4    ----0---
 5    ---0----
 6    --0-----
 7    -0------
 8    0-------
 9    -0------
10    --0-----
11    ---0----
12    ----0---
13    -----0--
14    ------0-
15    -------0
16    ------0-
...
```
```c++
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h"

uint32_t ON_time = 200000;
uint32_t OFF_time = 200000;

void app_main(void)
{
    gpio_reset_pin(23);
    gpio_reset_pin(22);
    gpio_reset_pin(1);
    gpio_reset_pin(3);
    gpio_reset_pin(21);
    gpio_reset_pin(19);
    gpio_reset_pin(18);
    gpio_reset_pin(5);

    gpio_set_direction(23, GPIO_MODE_OUTPUT);
    gpio_set_direction(22, GPIO_MODE_OUTPUT);
    gpio_set_direction(1, GPIO_MODE_OUTPUT);
    gpio_set_direction(3, GPIO_MODE_OUTPUT);
    gpio_set_direction(21, GPIO_MODE_OUTPUT);
    gpio_set_direction(19, GPIO_MODE_OUTPUT);
    gpio_set_direction(18, GPIO_MODE_OUTPUT);
    gpio_set_direction(5, GPIO_MODE_OUTPUT);

    while (true)
    {
        gpio_set_level(23, 1);
        usleep(ON_time);
        gpio_set_level(23, 0);
        usleep(OFF_time);
        gpio_set_level(22, 1);
        usleep(ON_time);
        gpio_set_level(22, 0);
        usleep(OFF_time);
        gpio_set_level(1, 1);
        usleep(ON_time);
        gpio_set_level(1, 0);
        usleep(OFF_time);
        gpio_set_level(3, 1);
        usleep(ON_time);
        gpio_set_level(3, 0);
        usleep(OFF_time);
        gpio_set_level(21, 1);
        usleep(ON_time);
        gpio_set_level(21, 0);
        usleep(OFF_time);
        gpio_set_level(19, 1);
        usleep(ON_time);
        gpio_set_level(19, 0);
        usleep(OFF_time);
        gpio_set_level(18, 1);
        usleep(ON_time);
        gpio_set_level(18, 0);
        usleep(OFF_time);
        gpio_set_level(5, 1);
        usleep(ON_time);
        gpio_set_level(5, 0);
        usleep(OFF_time);

        gpio_set_level(18, 1);
        usleep(ON_time);
        gpio_set_level(18, 0);
        usleep(OFF_time);
        gpio_set_level(19, 1);
        usleep(ON_time);
        gpio_set_level(19, 0);
        usleep(OFF_time);
        gpio_set_level(21, 1);
        usleep(ON_time);
        gpio_set_level(21, 0);
        usleep(OFF_time);
        gpio_set_level(3, 1);
        usleep(ON_time);
        gpio_set_level(3, 0);
        usleep(OFF_time);
        gpio_set_level(1, 1);
        usleep(ON_time);
        gpio_set_level(1, 0);
        usleep(OFF_time);
        gpio_set_level(22, 1);
        usleep(ON_time);
        gpio_set_level(22, 0);
        usleep(OFF_time);
    }
}
```
ยืด-หด
```
 1    -------0
 2    ------00
 3    -----000
 4    ----0000
 5    ---00000
 6    --000000
 7    -0000000
 8    00000000
 9    -0000000
10    --000000
11    ---00000
12    ----0000
13    -----000
14    ------00
15    -------0
16    ------00
```
```c++
while (true)
    {
        gpio_set_level(23, 1);
        usleep(ON_time);
        gpio_set_level(22, 1);
        usleep(ON_time);
        gpio_set_level(1, 1);
        usleep(ON_time);
        gpio_set_level(3, 1);
        usleep(ON_time);
        gpio_set_level(21, 1);
        usleep(ON_time);
        gpio_set_level(19, 1);
        usleep(ON_time);
        gpio_set_level(18, 1);
        usleep(ON_time);
        gpio_set_level(5, 1);
        usleep(ON_time);
        gpio_set_level(5, 0);
        usleep(OFF_time);
        gpio_set_level(18, 0);
        usleep(OFF_time);
        gpio_set_level(19, 0);
        usleep(OFF_time);
        gpio_set_level(21, 0);
        usleep(OFF_time);
        gpio_set_level(3, 0);
        usleep(OFF_time);
        gpio_set_level(1, 0);
        usleep(OFF_time);
        gpio_set_level(22, 0);
        usleep(OFF_time);
        gpio_set_level(23, 0);
        usleep(OFF_time);
    }
```
ขยายออก
```
 1    ---00---
 2    --0--0--
 3    -0----0-
 4    0------0
 5    -0----0-
 6    --0--0--
 7    ---00---
 8    --0--0--
```
```c++
    while (true)
    {
        gpio_set_level(21, 1);
        gpio_set_level(3, 1);
        usleep(ON_time);
        gpio_set_level(21, 0);
        gpio_set_level(3, 0);
        usleep(OFF_time);
        gpio_set_level(1, 1);
        gpio_set_level(19, 1);
        usleep(ON_time);
        gpio_set_level(1, 0);
        gpio_set_level(19, 0);
        usleep(OFF_time);
        gpio_set_level(22, 1);
        gpio_set_level(18, 1);
        usleep(ON_time);
        gpio_set_level(22, 0);
        gpio_set_level(18, 0);
        usleep(OFF_time);
        gpio_set_level(23, 1);
        gpio_set_level(5, 1);
        usleep(ON_time);
        gpio_set_level(23, 0);
        gpio_set_level(5, 0);
        usleep(OFF_time);
        gpio_set_level(22, 1);
        gpio_set_level(18, 1);
        usleep(ON_time);
        gpio_set_level(22, 0);
        gpio_set_level(18, 0);
        usleep(OFF_time);
        gpio_set_level(1, 1);
        gpio_set_level(19, 1);
        usleep(ON_time);
        gpio_set_level(1, 0);
        gpio_set_level(19, 0);
        usleep(OFF_time);
    }
```
ยุบเข้า
```
 1    0------0
 2    -0----0-
 3    --0--0--
 4    ---00---
 5    --0--0--
 6    -0----0-
 7    0------0
 8    -0----0-
```
```c++
    while (true)
    {
        gpio_set_level(23, 1);
        gpio_set_level(5, 1);
        usleep(ON_time);
        gpio_set_level(23, 0);
        gpio_set_level(5, 0);
        usleep(OFF_time);
        gpio_set_level(22, 1);
        gpio_set_level(18, 1);
        usleep(ON_time);
        gpio_set_level(22, 0);
        gpio_set_level(18, 0);
        usleep(OFF_time);
        gpio_set_level(1, 1);
        gpio_set_level(19, 1);
        usleep(ON_time);
        gpio_set_level(1, 0);
        gpio_set_level(19, 0);
        usleep(OFF_time);
        gpio_set_level(21, 1);
        gpio_set_level(3, 1);
        usleep(ON_time);
        gpio_set_level(21, 0);
        gpio_set_level(3, 0);
        usleep(OFF_time);
        gpio_set_level(1, 1);
        gpio_set_level(19, 1);
        usleep(ON_time);
        gpio_set_level(1, 0);
        gpio_set_level(19, 0);
        usleep(OFF_time);
        gpio_set_level(22, 1);
        gpio_set_level(18, 1);
        usleep(ON_time);
        gpio_set_level(22, 0);
        gpio_set_level(18, 0);
        usleep(OFF_time);
    }
```
