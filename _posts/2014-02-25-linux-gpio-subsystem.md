---
author: Xiaojie Yuan
layout: post
title: "[Kernel] Linux GPIO Subsystem"
date: 2014-02-25
---

### Kernel GPIO Interface

```c
bool gpio_is_valid(int number);
 int gpio_request(unsigned gpio, const char *label);
void gpio_free(unsigned gpio);
 int gpio_direction_input(unsigned gpio);
 int gpio_direction_output(unsigned gpio, int value);
 int gpio_get_value(unsigned gpio);
void gpio_set_value(unsigned gpio, int value);
 int gpio_export(unsigned gpio, bool direction_may_change);
void gpio_unexport(unsigned gpio);
 int gpio_to_irq(unsigned gpio);
```

### Test Code

* gpio.c

```c
/*
 * test module for kernel gpio subsystem
 * module functions:
 *     set a certain pin to output, then set it as high
 *     set a specific pin to input, rising edge as external interrupt and
 *     interrupt handler will print some message to kernel log
 */

#include <linux/module.h>
#include <linux/init.h>
#include <linux/kernel.h>
#include <linux/interrupt.h>
#include <linux/gpio.h>
#include <linux/delay.h>
#include <linux/errno.h>

#define USR0_PIN        (32*1 + 21)
#define USR1_PIN        (32*1 + 22)
#define USR2_PIN        (32*1 + 23)
#define USR3_PIN        (32*1 + 24)
#define BUZZER_PIN      (32*1 + 28)

irqreturn_t gpio_interrupt(int irq, void *dev_id)
{
        printk(KERN_ALERT "irq-%d occurs\n", irq);

        return IRQ_HANDLED;
}

static int __init gpio_init(void)
{
        const char *pin_name = "buzzer";
        int ret = 0;
        int pin_val, pin_irq_no;

        if (gpio_is_valid(BUZZER_PIN) == false) {
                printk(KERN_ERR "Error: gpio%d[%d] invalid\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
                ret = -EINVAL;
                goto validate_failed;
        }

        ret = gpio_request(BUZZER_PIN, pin_name);
        if (ret == -EBUSY) {
                printk(KERN_ERR "Error: gpio%d[%d] busy\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
                goto request_failed;
        } else if (ret < 0) {
                printk(KERN_ERR "Error: gpio%d[%d] request failed\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
                goto request_failed;
        }

        ret = gpio_export(BUZZER_PIN, true);
        if (ret != 0) {
                printk(KERN_ERR "Error: gpio%d[%d] export failed\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
                goto export_failed;
        }

        ret = gpio_direction_output(BUZZER_PIN, 1);
        if (ret != 0) {
                printk(KERN_ERR "Error: gpio%d[%d] set direction failed\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
                goto direction_failed;
        }

        gpio_set_value(BUZZER_PIN, 1);
        mdelay(50);
        gpio_set_value(BUZZER_PIN, 0);

        ret = gpio_direction_input(BUZZER_PIN);
        if (ret != 0) {
                printk(KERN_ERR "Error: gpio%d[%d] set direction failed\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
                goto direction_failed;
        }

        pin_val = gpio_get_value(BUZZER_PIN);
        printk(KERN_ALERT "value of gpio%d[%d] is %d\n",
                        BUZZER_PIN/32, BUZZER_PIN%32, pin_val);

        pin_irq_no = gpio_to_irq(BUZZER_PIN);
        if (pin_irq_no < 0) {
                printk(KERN_ERR "Error: gpio%d[%d] has no irq\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
        }
        printk(KERN_ALERT "irq of gpio%d[%d] is %d\n",
                        BUZZER_PIN/32, BUZZER_PIN%32, pin_irq_no);

        ret = request_irq(pin_irq_no,
                          gpio_interrupt,
                          IRQF_TRIGGER_RISING,
                          "buzzer_pin",
                          NULL);

        printk(KERN_INFO "gpio.ko: init ok\n");
        return 0;

direction_failed:
        gpio_unexport(BUZZER_PIN);
export_failed:
        gpio_free(BUZZER_PIN);
request_failed:
validate_failed:
        return ret;
}

static void __exit gpio_exit(void)
{
        int pin_irq_no;

        pin_irq_no = gpio_to_irq(BUZZER_PIN);
        if (pin_irq_no < 0) {
                printk(KERN_ERR "Error: gpio%d[%d] has no irq\n",
                                BUZZER_PIN/32, BUZZER_PIN%32);
        }

        gpio_set_value(BUZZER_PIN, 0);
        gpio_unexport(BUZZER_PIN);
        gpio_free(BUZZER_PIN);
        free_irq(pin_irq_no, NULL);

        printk(KERN_INFO "gpio.ko: exit ok\n");
}

module_init(gpio_init);
module_exit(gpio_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Xiaojie Yuan <yxj0207@gmail.com>");
```

* Makefile

```makefile
obj-m += gpio.o
KERNELDIR := /usr/src/kernel
PWD := $(shell pwd)

all:
	make -C $(KERNELDIR) M=$(PWD) ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- modules
clean:
	make -C $(KERNELDIR) M=$(PWD) ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- clean
```

### Run Module
```
$ insmod gpio.ko
[  987.757562] value of gpio1[28] is 0
[  987.761259] irq of gpio1[28] is 204
[  987.765896] gpio.ko: init ok
$ ls /sys/class/gpio
export    gpio60    gpiochip0    gpiochip32    gpiochip64    gpiochip96    unexport
$ cat /proc/interrupts
204:         38      GPIO  28  buzzer_pin
$ rmmod gpio.ko
[ 1191.805627] gpio.ko: exit ok
$ ls /sys/class/gpio
export    gpiochip0    gpiochip32    gpiochip64    gpiochip96    unexport
```

# References
[1] Documentations/gpio.txt under kernel source
[2] https://github.com/CircuitCo/BeagleBone-Black/blob/master/BBB_SCH.pdf
