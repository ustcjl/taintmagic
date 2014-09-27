note：实验室的Android代码是用repo(git的封装)管理的，它存放在ip地址为219.219.220.211服务器上，这台服务器的用户名和密码为
salve1.

使用的Linux系统为Ubuntu14.04，64位系统

第一步：安装git客户端
$ sudo apt-get install git-core

第二步：下载repo文件，在这个仓库下有这个文件，已经帮你改好，若想熟悉repo client可参考

http://source.android.com/source/downloading.html#installing-repo

第三步：修改该reop文件使其有可执行权限，然后执行下面的命令，进行下载Android源码的初始化工作

$ ./repo init -u git://219.219.220.211/platform/manifest  

第四步： 开始下载Android源码，这个得要一二个小时左右

$ ./repo sync -j4   //4 process run at the same time 

下载完成后就可以build该Android源码了，由于下载的是所有版本的Android源码，你得进行版本的切换下面的命令可查看有多少个Andorid的版本

$ .repo/manifests
$ git branch -a | cut -d / -f 3  

当你选定你要编译的版本后，就可以开始切换到该版本去
$ ./repo init -b "branch-name"  
$ ./repo sync    //执行切换到该版本的动作

这个切换版本得花一个小时左右的时间，切换好了就可以开始真正的build了。note：最好切换到Android2.3.4版本，因为这得和TaintDroid的版本符合。


