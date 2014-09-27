在你已经build了Android源码后，你就可以开始进行最后一步：taintdroid的编译了，可参考下面的URL来build了。

http://appanalysis.org/download_2.3.html

最后编译成功的话，你可以使用sdk的emulator来启动这个新的ROM，里面就有系统级的软件：taindroid，可以看看它的具体效果。

关于如何使用emulateor可去百度，也可以来CSS实验室拿<Android 系统源代码情景分析>这本书，参考它的第一章

ps：我还没编译成功，希望你们能build成功啊，加油。。

在build taintdroid可能会出现以下的错误，下面的解决方案可供参考

Q1:make: *** [out/target/product/generic/obj/SHARED_LIBRARIES/libdvm_intermediates/Atomic.o] 错误 1
A1:http://willsunforjava.iteye.com/blog/1744626

Q2: dalvik/vm/Atomic.h:57: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'dvmQuasiAtomi
A2: export TARGET_ARCH_VARIANT=armv7-a-neon

Q3: make: *** 没有规则可以创建“out/host/common/obj/JAVA_LIBRARIES/layoutlib_intermediates/javalib.jar”需要的目标“out/host/linux-x86/framework/layoutlib_api-prebuilt.jar”。 停止
A3: 这个问题还没解决


