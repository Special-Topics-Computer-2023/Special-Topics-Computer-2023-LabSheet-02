# IoT-ESP32-LabSheet-02
# โปรแกรมไฟวิ่ง 8 ดวง


### 000. การส่งงาน
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
---

## 1. ก่อนการทดลอง

จากการทดลองที่ 1 เราสามารถทำไฟกระพริบ 1 ดวงได้ ในการทดลองนี้ เราจะนำไฟกระพริบหลาย ๆ ดวง ให้กระพริบต่อเนื่องในรูปแบบต่าง ๆ แต่ก่อนอื่น จะแนะนำฟังก์ชันที่ใช้ในการเปลี่ยนเวลาในการกระพริบให้ผู้เรียนได้ทราบก่อน

### ฟังก์ชันสำหรับการเปลี่ยนเวลา delay 

จาก source code  ในใบงานที่แล้ว เราจะเห็นบรรทัดหนึ่งที่ใช้ในการกำหนดเวลาในการกระพริบ นั่นคือ 

``` c
    sleep(1); 
``` 

หากเราตามไปดู source code ของฟังก์ชันดังกล่าว (โดยการกด Ctrl และคลิกที่ชื่อฟังก์ชัน) ก็จะพบกับตัวฟังก์ชันซึ่งมีรายละเอียดดังนี้

``` c
unsigned int sleep(unsigned int seconds)
    {
        usleep(seconds*1000000UL);
        return 0;
    }
```
ซึ่งจะเห็นได้ชัดว่าเราสามารถกำหนดเวลาในการ delay ได้ต่ำสุดเป็นวินาทีเท่านั้น โดยสั่งเกตุจากพารามิเตอร์ของฟังก์ชัน  และถ้าหากต้องการ delay ให้ต่ำกว่า 1 วินาทีก็ต้องใช้ฟังก์ชันอื่นมาช่วย

ภายในฟังก์ชัน `unsigned int sleep(unsigned int seconds)` นั้นได้เรียกใช้ฟังก์ชัน ```int usleep(useconds_t us)``` ซึ่งจะมีคาบเวลาในการ delay เท่ากับ 1/1000000 วินาที (1 microsecond) โดยฟังก์ชันดังกล่าวมีรายละเอียดดังนี้

``` c
    int usleep(useconds_t us)
    {
        const int us_per_tick = portTICK_PERIOD_MS * 1000;
        if (us < us_per_tick) {
            esp_rom_delay_us((uint32_t) us);
        } else {
            /* since vTaskDelay(1) blocks for anywhere between 0 and portTICK_PERIOD_MS,
            * round up to compensate.
            */
            vTaskDelay((us + us_per_tick - 1) / us_per_tick);
        }
        return 0;
    }
```

ซึ่งเราสามารถนำมาใช้แทนฟังก์ชัน `sleep()` โดยระบุพารามิเตอร์เป็นไมโครวินาที เช่น

``` c
    usleep(500000);  // 500000 ไมโครวินาที  = 0.5 วินาที
```

## 2. การทดลองปรับเปลี่ยนเวลาสำหรับไฟกระพริบ

1. ประกอบวงจรบนบอร์ดทดลองตามใบงานที่ 1
2.  เปิด project ของใบงานที่ 1
3. แก้ source code บรรทัด `sleep(1);` เป็น `usleep(500000);` ทั้งสองที่ จะได้ source code ดังนี้

```c
    #include <stdio.h>
    #include <stdbool.h>
    #include <unistd.h>
    #include "driver/gpio.h"                        // เพื่อการใช้งาน digital output (GPIO)

    void app_main(void)
    {
        gpio_reset_pin(22);                         // รีเซ็ตสถานะของขาหมายเลข 22
        gpio_set_direction(22, GPIO_MODE_OUTPUT);   // กำหนดให้ขาหมายเลข 22 เป็น digital output

        while (true)                                // while (true) = วนรอบไม่มีที่สิ้นสุด
        {
            gpio_set_level(22, 1);                  // สั่งให้ LED ติด
            usleep(500000);                         // หน่วงเวลา 0.5 วินาที
            gpio_set_level(22, 0);                  // สั่งให้ LED ดับ
            usleep(500000);                         // หน่วงเวลา 0.5 วินาที
        }
    }
```


4. Build และ Run โปรแกรม (อาจจะกด Run ในคราวเดียวก็ได้) 
5. สังเกตุการกระพริบของหลอดไฟ
6. ทดลองเปลี่ยนค่าตัวเลข _500000_ ใน `usleep(500000);` เป็นค่าอื่น ๆ  หรือกำหนดให้แต่ละที่มีค่าที่แตกต่างกัน



## การทดลอง

เมื่อ เปลี่ยน sleep(1); เป็น usleep(500000); โดยใช้โค้ดไฟกระพริบจากใบงานที่ 1 จะทำให้ไฟกระพริบ ติด - ดับ สลับกันเร็จขึ้นโดยจะเปลี่ยนจาก ติดหรือดับค้างไวh 1 วินาทีเป็น 0.5 วินาที

## โปรแกรมไฟวิ่ง 8 ดวงรุ่น 1
เมื่ออัพโหลดโปรแกามนี้ลงบอร์ดจะทำให้ไปวิ่งจากดวงซ้ายสุดไปขวาสุดของบอร์ด โดนมีเวลาดีเลย์ 1 วินาที
```css
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h" // เพื่อการใช้งาน digital output (GPIO)

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
        sleep(1);
        gpio_set_level(23, 0);
        sleep(1);
        gpio_set_level(22, 1);
        sleep(1);
        gpio_set_level(22, 0);
        sleep(1);
        gpio_set_level(1, 1);
        sleep(1);
        gpio_set_level(1, 0);
        sleep(1);
        gpio_set_level(3, 1);
        sleep(1);
        gpio_set_level(3, 0);
        sleep(1);
        gpio_set_level(21, 1);
        sleep(1);
        gpio_set_level(21, 0);
        sleep(1);
        gpio_set_level(19, 1);
        sleep(1);
        gpio_set_level(19, 0);
        sleep(1);
        gpio_set_level(18, 1);
        sleep(1);
        gpio_set_level(18, 0);
        sleep(1);
        gpio_set_level(5, 1);
        sleep(1);
        gpio_set_level(5, 0);
        sleep(1);
    }
}
```
## โปรแกรมไฟวิ่ง 8 ดวงรุ่นที่ 2

ในโปรแกรมไฟวิ่ว 8 ดวงรุ่นที 2 จะได้ผลลัพธ์เหมือนกันกับรุ่นแรกแต่เราจะสามารถเปลี่ยนเวลาติด - ดับของไฟได้จากตัวแปร ON_time และ OFF_time ที่เก็บค่าเวลาไว้และถูกเรียกใช้ใน usleep ดังโค้ดด้านล่าง

```css
    #include <stdio.h>
    #include <stdbool.h>
    #include <unistd.h>
    #include "driver/gpio.h"

    uint32_t ON_time = 1000000;
    uint32_t OFF_time = 1000000;


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
        }
    }
```

## โปรแกรมไฟวิ่ง 8 ดวงรุ่นที่ 3

ในโปรแกรมไฟวิ่งรุ่นที 3 จะเปลี่นการกำหนด direction การ reset pin และการ set level ของ pin ด้วย loop ทำให้โค้ดสั่นลงและอ่านง่ายึ้นและได้ผลเหมือนกันกับรุ่นที 1 และ 2

```css
    #include <stdio.h>
    #include <stdbool.h>
    #include <unistd.h>
    #include "driver/gpio.h"  

    uint32_t ON_time = 500000;
    uint32_t OFF_time = 0;

    uint8_t LEDIO[8] = { 23, 22, 1, 3, 21, 19, 18, 5 };

    void app_main(void)
    {
        for (uint8_t i = 0; i < 8; i++)
        {
            gpio_reset_pin(LEDIO[i]);
        }

        for (uint8_t i = 0; i < 8; i++)
        {
            gpio_set_direction(LEDIO[i], GPIO_MODE_OUTPUT);
        }
        
        uint8_t ledNum = 0;

        while (true)
        {
            gpio_set_level(LEDIO[ledNum], 1);
            usleep(ON_time);
            gpio_set_level(LEDIO[ledNum], 0);
            usleep(OFF_time);
            ledNum++;
            if (ledNum == 8)
                ledNum = 0;
        }
    }
```


## [3. วงจรไฟวิ่ง 8 ดวง (คลิกเพื่อเข้าสู่การทดลอง)](./chasing_led.md)





