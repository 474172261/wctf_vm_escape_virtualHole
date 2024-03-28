# WCTF QEMU escape practice: VirtualHole
This is a ctf challenge for qemu escape, it's basic and simple, but there is a thinking trap in it, you may need some time(more than 1 hour or 6 hours) to find out the bug. (Unless you really can't find it, don't ask others or search online! If you know the answer, the thinking trap will disapear)

## basic info
	1. check "qemu/v4.0.0/source/hw/scsi/megasas.c", you need to replace qemu's same file with mine, and you have to build a virtual machine by your self.
	2. start the vm by: `qemu-system-x86_64 -m 2048 -hda Centos7.img --enable-kvm -device megasas`
	3. you can read https://v-v.space/otherfile/qemu%E5%8F%8Akvm%E8%BD%AF%E4%BB%B6%E7%AE%80%E4%BB%8B.pdf if you don't know much about VM escape.


## qemu compile from source:
If you don't know how to compile it, you may need to check this [qemu build from source](https://v-v.space/2022/06/17/qemu-build/)
You can use any qemu source version, remember change the code of that version's `megasas` file.
But if you are not familiar with Linux and QEMU, I advice you build `qemu 4.0` on Ubuntu 16.04.

A simple way to build enviroment:
1. Install libs</br>
	Ubuntu 16.04</br>
	`$ sudo apt-get install -y zlib1g-dev`</br>
	`$ sudo apt-get install -y libglib2.0-dev`</br>
	`$ sudo apt-get install -y autoconf`</br>
	`$ sudo apt-get install -y libtool`</br>
	`$ sudo apt-get install -y libgtk2.0-dev`</br> 
	`$ sudo apt install qemu-kvm`</br>
	
2. compile qemu </br>
	`$ ./configure  --enable-kvm --target-list=x86_64-softmmu`</br>
	`$ make && make install`</br>

3. build VM </br>
	`$ qemu-img create -f qcow2 centos.img 10G`</br>
	`$ /usr/local/bin/qemu-system-x86_64 -m 256 -hda centos.img -cdrom YourCentosISO.iso -enable-kvm`

after build VM, stop VM, then start test</br>
	`$ /usr/local/bin/qemu-system-x86_64 -m 2048 -hda Centos7-Guest.img --enable-kvm -device megasas`
	
## Build your test driver
```c
#include <linux/init.h>
#include <linux/module.h>
#include <asm/io.h>

#define PHYS_ADDR 0xfeb80000
#define MAP_PHYS_LEN 0x1000

static int my_module_init(void)
{
    printk("module init\n");
    void *mapped_addr = ioremap(PHYS_ADDR, MAP_PHYS_LEN);
    writel(0x200, mapped_addr+1*4);
    iounmap(mapped_addr);
    return 0;
}

static void my_module_exit(void)
{
    printk("module exit\n");
}

module_init(my_module_init );
module_exit(my_module_exit );
```

Makefile
```
obj-m += test.o
KDIR:=/lib/modules/$(shell uname -r)/build
MAKE:=make  
default:  
	$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules  
clean:  
	$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) clean 
```
