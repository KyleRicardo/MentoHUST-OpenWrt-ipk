# MentoHUST-OpenWrt-ipk

这是一个积累了很长时间的项目。从2014年起，就陆续在折腾这些东西，期间还自己写了一个基于OpenWrt的神州数码客户端[802.1X Evasi0n](https://github.com/KyleRicardo/802.1X-Evasi0n)，服役了大半年的时间，表现良好，内存占用小，工作效率高。后来，学校所有神州数码服务器全部换成了锐捷，导致我的项目失去了光彩，我又翻出了大名鼎鼎的MentoHUST，在反复折腾交叉编译和功能性修补过程中，有了这样一个版本。该版本非常适合于在OpenWrt的SDK环境下编译出MentoHUST的ipk包。



## 特点

- 修复了原MentoHUST在shell下由于iconv库编译或工作不正常导致的反馈信息乱码问题
- 去除了libiconv库的依赖，加入了轻量级的strnormalize库，GBK to UTF-8转换良好
- 去除configure等冗余文件，仅保留核心src源码文件
- ./src/Makefile中使用通配符*指代libpcap版本，通用性更强
- 无需手动配置环境变量，无需使用automake和configure生成所需Makefile
- 重新完全手动编写./和./src/目录下的Makefile，保证编译的有效性
- 无--disable-notify --disable-encodepass等配置，保证原汁原味
- 无手动#define NO_DYLOAD补丁，使用动态加载库函数，ipk包更小，并且更容易编译



## 编译

编译前，需要配置好SDK，首先确定目标路由器的CPU架构，然后安装feeds，tool和Toolchain，在这里就不赘述了。在这里默认各位用户已经有基于OpenWrt SDK的交叉编译基本知识。

首先，进入trunk目录，然后执行下列命令：

```
pushd package
git clone https://github.com/KyleRicardo/MentoHUST-OpenWrt-ipk.git
popd
```

克隆该git库之后，就可以开始编译了：

 `make package/MentoHUST-OpenWrt-ipk/compile V=s`

如果顺利，编译完成之后就能在`trunk/bin/YourArchitecture/packages/base`中找到你的ipk包了。现在通过SCP拷贝到路由器/tmp/目录，然后用opkg安装即可。

安装好后可以立即使用，配置文件在/etc/mentohust.conf，可以自行编辑。

## 已知问题

- mentohust未能智能识别路由器WAN口对应的网卡，请手动在mentohust.conf的末尾DHCP脚本中添加自己WAN口对应的网卡。最终脚本类似`udhcpc -i eth1`
- 暂未加入init.d目录的mentohust脚本，可能下个版本加入。
- 后续可能加入只有一个Makefile，通过自动从git下载源码进行编译的版本

