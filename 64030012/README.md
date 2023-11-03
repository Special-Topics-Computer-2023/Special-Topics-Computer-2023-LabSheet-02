# การทดลอง 

เมื่อเปลี่ยน sleep เป็น usleep จะทำให้เปลี่ยนหน่วยเวลาจาก sec เป็น milisec

# โปรแกรมไฟวิ่งรุ่น 1
ไฟจะกระพริบตจากซ้ายสุดไปขวาสุดโดยใช้เวลา delay 1 วินาที

```c++
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

# โปรแกรมไฟวิ่งรุ่น 2

ผลลัพธ์เหมือนรุ่นแรกแต่เก็บเวลาดีเลย์ไว้ในตัวแปร ON_time และ OFF_time
```c++
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

# โปรแกรมไฟวิ่งรุ่น 3

 ใช้ลูปมาช่วยในการทำให้ไฟวิ่ง ทำมห้ไม่ต้องเขียนโค้ดซ้ำซ้อนกันและได้ผลเหมือนรุ่น 1 และ 2

 ```c++
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

# Challenge
วิ่งไปวิ่งกลับ
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
