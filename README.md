# wctf_vm_escape_virtualHole
This is a ctf challenge for qemu escape, it's basic and normal, but there is a thinking trap in it, you may need some time to find out the bug ;)

## basic info
	1. 在megasas里面找点有意思的东西吧.
	2. qemu启动参数:qemu-system-x86_64 -m 2048 -hda Centos7.img --enable-kvm -device megasas
	3. 以下资料可能对你测试有帮助 https://bbs.pediy.com/thread-224371.htm


## qemu编译:
1. 安装库
	Ubuntu系统
	$ sudo apt-get install -y zlib1g-dev
	$ sudo apt-get install -y libglib2.0-dev
	$ sudo apt-get install -y autoconf
	$ sudo apt-get install -y libtool
	$ sudo apt-get install -y libgtk2.0-dev 
	$ sudo apt install qemu-kvm
	> 安装qemu-kvm库的时候会把qemu也安装到/usr/bin/目录下,使用时注意移除它
2. 编译
	$ ./configure  --enable-kvm --target-list=x86_64-softmmu
	$ make && make install
3. 启动
	$ /usr/local/bin/qemu-system-x86_64 -m 2048 -hda Centos7-Guest.img --enable-kvm -device megasas
