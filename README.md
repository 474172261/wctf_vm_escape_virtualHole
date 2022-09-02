# WCTF QEMU escape practice: VirtualHole
This is a ctf challenge for qemu escape, it's basic and simple, but there is a thinking trap in it, you may need some time(more than 1 hour or 6 hours) to find out the bug. Unless you really can't find it, don't ask others or search online.

## basic info
	1. 在megasas里面找点有意思的东西吧.(my change based on QEMU 4.0 source code)
	2. qemu启动参数:qemu-system-x86_64 -m 2048 -hda Centos7.img --enable-kvm -device megasas
	3. 以下资料可能对你测试有帮助 https://bbs.pediy.com/thread-224371.htm, https://v-v.space/2022/06/17/qemu-virtual-device-init/


## qemu compile from source:
If you don't know how to compile it, you may need to check this [qemu build from source](https://v-v.space/2022/06/17/qemu-build/)
You can use any qemu source version, remember change the code of that version's `megasas` file.
But if you are not familiar with Linux and QEMU, I advice you build `qemu 4.0` on Ubuntu 16.4.
> you should install a Guest system by yourself.

1. 安装库</br>
	Ubuntu系统</br>
	`$ sudo apt-get install -y zlib1g-dev`</br>
	`$ sudo apt-get install -y libglib2.0-dev`</br>
	`$ sudo apt-get install -y autoconf`</br>
	`$ sudo apt-get install -y libtool`</br>
	`$ sudo apt-get install -y libgtk2.0-dev`</br> 
	`$ sudo apt install qemu-kvm`</br>
	> 安装qemu-kvm库的时候会把qemu也安装到/usr/bin/目录下,使用时注意移除它
2. 编译</br>
	`$ ./configure  --enable-kvm --target-list=x86_64-softmmu`</br>
	`$ make && make install`</br>
3. 启动</br>
	`$ /usr/local/bin/qemu-system-x86_64 -m 2048 -hda Centos7-Guest.img --enable-kvm -device megasas`
