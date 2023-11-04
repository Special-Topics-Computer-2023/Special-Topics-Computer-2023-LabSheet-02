# 64030313 นางสาวสุดารัตน์ ฆ้องอินตะ

# โปรแกรมสมบูรณ์ของไฟวิ่ง 8 ดวง รุ่นที่ 1
## Video
https://drive.google.com/file/d/1gR6aTZbdnRiv-ODgAS1T5k3nNihqq00D/view?usp=sharing
## Code
```c
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h"

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
# โปรแกรมสมบูรณ์ของไฟวิ่ง 8 ดวง รุ่นที่ 2
## Video
https://drive.google.com/file/d/1o12kcmZR9P_X_JNhHb6TtuDUQLyQcguI/view?usp=sharing
## Code
```c
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
# โปรแกรมสมบูรณ์ของไฟวิ่ง 8 ดวง รุ่นที่ 3
## Video
https://drive.google.com/file/d/1ZNss961tIZl_m_WYT0sdcaSxWPEKUpag/view?usp=sharing
## Code
```c
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "driver/gpio.h"

uint32_t ON_time = 500000;
uint32_t OFF_time = 0;

uint8_t LEDIO[8] = {23, 22, 1, 3, 21, 19, 18, 5};

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
