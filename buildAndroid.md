第一步：Android2.3.4版本buid的话，得先安装jdk1.6版本，自行百度 linux jdk1.6版本的安装。

第二步：gcc，g++降版，不然编译会不通过
$ sudo apt-get install gcc-4.4
$ sudo apt-get install g++-4.4
$ sudo rm -rf /usr/bin/gcc /usr/bin/g++
$ sudo ln -s /usr/bin/gcc-4.4 /usr/bin/gcc
$ sudo ln -s /usr/bin/g++-4.4 /usr/bin/g++

第三步：build过程中会出现很多错误，很多是因为缺少相应的软件导致的，所以预先装这些软件
$ sudo apt-get install flex
$ sudo apt-get install lsb-core
$ sudo apt-get install zlib1g-dev 
$ sudo apt-get install lib32z1-dev
$ sudo apt-get install libncurses5-dev
$ sudo apt-get install lib32ncurses5-dev
$ sudo apt-get install libx11-dev
$ sudo apt-get install gperf

第四步： 开始build Android源码了,最好用root权限build.

$ sudo make


第五步：若build成功的话，会在源码根目录下生成一个out目录，里面存放了你build的system.img等系统文件，这些就是你编译的ROM文件

启动已经编译好的android img，这个emulator可执行文件是别的SDK已经编译好的工具。

$ sudo ~/adt-bundle-linux-x86-20131030/sdk/tools/emulator -kernel ~/android2.3/prebuilt/android-arm/kernel/kernel-qemu -sysdir ~/android2.3/out/target/product/generic/ -system system.img -data userdata.img -ramdisk ramdisk.img 



参考：若出现问题，可参考下面的解决方案


Q1: /bin/bash: prebuilt/linux-x86/toolchain/arm-eabi-4.4.3/bin/arm-eabi-gcc: 没有那个文件或目录
A1 : sudo apt-get install lsb-core

Q2: find: `frameworks/base/frameworks/base/docs/html': 没有那个文件或目录
find: `out/target/common/docs/gen': 没有那个文件或目录
find: `frameworks/base/frameworks/base/docs/html': 没有那个文件或目录
find: `out/target/common/docs/gen': 没有那个文件或目录
find: `frameworks/base/frameworks/base/docs/html': 没有那个文件或目录
find: `out/target/common/docs/gen': 没有那个文件或目录
find: `frameworks/base/frameworks/base/docs/html': 没有那个文件或目录
find: `out/target/common/docs/gen': 没有那个文件或目录
find: `frameworks/base/frameworks/base/docs/html': 没有那个文件或目录
find: `out/target/common/docs/gen': 没有那个文件或目录
A2 : 于是按照路径新建：frameworks/base/frameworks/base/docs/html   out/target/common/docs/gen

Q3: host C: acp <= build/tools/acp/acp.c
<command-line>:0:0: warning: "_FORTIFY_SOURCE" redefined [enabled by default]
build/tools/acp/acp.c:1:0: note: this is the location of the previous definition
 /*
 ^
In file included from /usr/include/stdlib.h:24:0,
                 from build/tools/acp/acp.c:11:
/usr/include/features.h:374:25: fatal error: sys/cdefs.h: 没有那个文件或目录
 #  include <sys/cdefs.h>

#sudo apt-get install libswitch-perl
#sudo apt-get install libx32gcc-4.8-dev
#sudo apt-get install libc6-dev-i386
A3:解决办法



Q4: host C++: libhost <= build/libs/host/pseudolocalize.cpp
host C: libhost <= build/libs/host/CopyFile.c
host StaticLib: libhost (out/host/linux-x86/obj/STATIC_LIBRARIES/libhost_intermediates/libhost.a)
echo out/host/linux-x86/obj/STATIC_LIBRARIES/libhost_intermediates/pseudolocalize.o out/host/linux-x86/obj/STATIC_LIBRARIES/libhost_intermediates/CopyFile.o | xargs ar crsP  out/host/linux-x86/obj/STATIC_LIBRARIES/libhost_intermediates/libhost.a
host Executable: acp (out/host/linux-x86/obj/EXECUTABLES/acp_intermediates/acp)
g++: selected multilib '32' not installed
make: *** [out/host/linux-x86/obj/EXECUTABLES/acp_intermediates/acp] 错误 1

A4: gcc 降版之后会出现 g++: selected multilib '32' not installed 这个问题，然后使用sudo apt-get install g++-4.4-multilib


Q5： external/clearsilver/cgi/cgi.c:22:18: fatal error: zlib.h: 没有那个文件或目录  
A5 : sudo apt-get install zlib1g-dev  
  
Q6: make: *** [out/host/linux-x86/obj/EXECUTABLES/aapt_intermediates/aapt] 错误 1
A6: sudo apt-get install lib32z1-dev

Q7: 警告：编码 ascii 的不可映射字符
A7: 将jdk1.6的版本变成jdk1.5

#vi frameworks/base/libs/utils/Android.mk
#Add '-fpermissive' to line 64:
#LOCAL_CFLAGS += -DLIBUTILS_NATIVE=1 $(TOOL_CFLAGS) -fpermissive 


Q8: make: *** 没有规则可以创建“out/target/common/docs/doc-comment-check-timestamp”需要的目标“Please-install-JDK-6.0,-which-you-can-download-from-java.sun.com”。 停止。
A8: http://blog.csdn.net/wbw1985/article/details/7009260


Q9: /bin/bash: 行 2: javadoc: 未找到命令
A9: 这个问题产生的根源，就是没有链接到javadoc
解决给问题的办法是：sudo update-alternatives --install /usr/bin/javadoc javadoc {YOUR_JDK_PATH}/bin/javadoc 300

Q10: make: *** [out/host/linux-x86/obj/EXECUTABLES/adb_intermediates/adb] 错误 1
A10: sudo apt-get install libncurses5-dev
sudo apt-get install lib32ncurses5-dev

Q11: make: *** [out/host/linux-x86/obj/SHARED_LIBRARIES/libdvm_intermediates/native/dalvik_system_Zygote.o] 错误 1
所以，这个问题只能修改源代码来解决

在dalvik/vm/native/dalvik_system_Zygote.cpp中间增加一个头文件定义#include <sys/resource.h>
#include "Dalvik.h"
#include "native/InternalNativePriv.h"
#include <sys/resource.h>


Q12: prebuilt/linux-x86/sdl/include/SDL/SDL_syswm.h:55: fatal error: X11/Xlib.h: 没有那个文件或目录
A12:  apt-get install libx11-dev

Q13: /usr/include/zlib.h:34: fatal error: zconf.h: 没有那个文件或目录
A13: zconf.h放到了/usr/include/x86_64-linux-gnu/，所以将其拷贝到/usr/include/下即可了

Q14: sh: 1: gperf: not found
A14: sudo apt-get install gperf
