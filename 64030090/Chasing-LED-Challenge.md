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
```
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h"

void app_main(void)
{
    int pins[] = {23, 22, 1, 3, 21, 19, 18, 5};
    int numPins = sizeof(pins) / sizeof(pins[0]);

    for (int i = 0; i < numPins; ++i)
    {
        gpio_reset_pin(pins[i]);
        gpio_set_direction(pins[i], GPIO_MODE_OUTPUT);
    }

    while (true)
    {
        for (int i = 0; i < numPins; ++i)
        {
            gpio_set_level(pins[i], 1);
            usleep(500000); // Sleep for 0.5 seconds
            gpio_set_level(pins[i], 0);
        }

        for (int i = numPins - 2; i > 0; --i)
        {
            gpio_set_level(pins[i], 1);
            usleep(500000); // Sleep for 0.5 seconds
            gpio_set_level(pins[i], 0);
        }
    }
}

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
```
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h"

void app_main(void)
{
    int pins[] = {23, 22, 1, 3, 21, 19, 18, 5};
    int numPins = sizeof(pins) / sizeof(pins[0]);

    for (int i = 0; i < numPins; ++i)
    {
        gpio_reset_pin(pins[i]);
        gpio_set_direction(pins[i], GPIO_MODE_OUTPUT);
    }

    while (true)
    {
        for (int i = 0; i < numPins; ++i)
        {
            for (int j = 0; j <= i; ++j)
            {
                gpio_set_level(pins[j], 1);
            }

            usleep(500000); // Sleep for 0.5 seconds

            for (int j = 0; j <= i; ++j)
            {
                gpio_set_level(pins[j], 0);
            }
        }

        for (int i = numPins - 2; i > 0; --i)
        {
            for (int j = 0; j <= i; ++j)
            {
                gpio_set_level(pins[j], 1);
            }

            usleep(500000); // Sleep for 0.5 seconds

            for (int j = 0; j <= i; ++j)
            {
                gpio_set_level(pins[j], 0);
            }
        }
    }
}

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
```
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h"

void app_main(void)
{
    int pins[] = {23, 22, 1, 3, 21, 19, 18, 5};
    int numPins = sizeof(pins) / sizeof(pins[0]);

    for (int i = 0; i < numPins; ++i)
    {
        gpio_reset_pin(pins[i]);
        gpio_set_direction(pins[i], GPIO_MODE_OUTPUT);
    }

    while (true)
    {
        for (int i = 0; i < numPins; ++i)
        {
            gpio_set_level(pins[i], 0);
        }

        gpio_set_level(pins[0], 1);
        gpio_set_level(pins[4], 1);

        usleep(500000); // Sleep for 0.5 seconds

        for (int i = 1; i <= 3; ++i)
        {
            gpio_set_level(pins[i], 1);
            gpio_set_level(pins[8 - i], 1);

            usleep(500000); // Sleep for 0.5 seconds

            gpio_set_level(pins[i], 0);
            gpio_set_level(pins[8 - i], 0);
        }
    }
}

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
```
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h"

void app_main(void)
{
    int pins[] = {23, 22, 1, 3, 21, 19, 18, 5};
    int numPins = sizeof(pins) / sizeof(pins[0]);

    for (int i = 0; i < numPins; ++i)
    {
        gpio_reset_pin(pins[i]);
        gpio_set_direction(pins[i], GPIO_MODE_OUTPUT);
    }

    while (true)
    {
        for (int i = 0; i < numPins; ++i)
        {
            gpio_set_level(pins[i], 0);
        }

        gpio_set_level(pins[3], 1);
        gpio_set_level(pins[4], 1);

        usleep(500000); // Sleep for 0.5 seconds

        for (int i = 2; i <= 5; ++i)
        {
            gpio_set_level(pins[i], 1);
            gpio_set_level(pins[8 - i], 1);

            usleep(500000); // Sleep for 0.5 seconds

            gpio_set_level(pins[i], 0);
            gpio_set_level(pins[8 - i], 0);
        }
    }
}

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
