# linux_reinstall
Something about tools


中文描述
脚本特色：

全自动无人值守安装；
支持各主流VPS商家；
重装前可预先指定 ssh 密码、端口、固件、镜像源等参数，执行重装命令时，如果未指定密码、端口。重装后的系统默认用户：root，默认端口：22，默认密码：LeitboGi0ro，首次 ssh 机器后请立即修改密码；
preseed 过程针对 Debian 做了大量优化，预置常用组件，永久更改 DNS 为 CloudFlare、Google（需进系统后手动安装 resolvconf：echo "N" | apt install resolvconf -y ），vim 支持鼠标终端复制，不同文件类型不同彩色显示，ssh 连接欢迎页面显示系统占用、IP 信息，软件数更新提示；
双栈（同时拥有 ipv6 和 ipv4 地址）机型默认优先配置 ipv4 网络，开机后请手动配置 ipv6 网络，针对纯 ipv6 机型的支持正在开发中；
对于 Debian 系统，安装时附带的固件源为国外，国内 VPS 连接速度很慢，长时间连接无速度往往会下载失败，可指定 --cdimage 'cn'，将源切换到国内中科大的，以提高下载速度；
安装时避免进入低内存模式（Debian 特有）后需要进行手动配置，导致无法自动化部署安装的内存量检测阈值，256M 以上机型即使安装时进入低内存模式，也可以自动化进行，这点对内存少于 1GB 的机型尤为重要。已在搬瓦工 512M 机型做过测试，萌咖原版脚本重装 Debian 11 时，会跳出低内存模式手动配置，自动化安装过程无法继续，首先必须手动选择需要加载的硬件驱动，项目多且复杂，不同机器的硬件各有差别，选择稍有错误，就会导致驱动安装不全，最后系统安装失败，本脚本可保证小内存 VPS 低内存模式自动化安装过程顺利进行，低于 768M 小内存机型安装前执行脚本时，不要附带“-firmware”或“-firmware --cdimage”参数，否则重启后无法进入低内存模式安装界面，导致安装失败。
由于 Ubuntu 22.04 官方移除了对“initrd.img”和“vmlinuz”两个网络引导安装文件的支持，导致目前并无很方便重装 Ubuntu 22.04 的方法，Ubuntu 母公司 Canonical 强推的 Cloudinit 自动部署方式对机器要求极高，必须有虚拟化支持，这是很多已经在母机上被虚拟化后的 VPS 所不具备的。目前仅甲骨文机器 CPU 仍支持虚拟化，所以市面上所有号称能重装成 Ubuntu 22.04 的一键脚本都是假的，无法完成安装，切勿相信。鉴于 Canonical 经常喜欢做焚烧自家亲妈的行为，未来不会对后续 Ubuntu 系统重装做任何支持。
CentOS 8 已被官方放弃，9 以后的 stream 版，成为一项供 Redhat Linux 测试 bug 的上游服务，不再具备 7 及以前版本可完备替代 Redhat Linux 的稳定成熟特性，后续也不再对 CentOS 进行支持。


下载：
wget --no-check-certificate -qO InstallNET.sh 'https://raw.githubusercontent.com/leitbogioro/Tools/master/Linux_reinstall/InstallNET.sh' && chmod a+x InstallNET.sh
复制代码


完整版可指定参数案例：
bash InstallNET.sh -d/u/c(系统种类，debian/ubuntu/centos) 11(系统版本) -v 64(系统位数，32 或 64 或 arm64) -port "ssh 登陆端口" -pwd "ssh 登录密码" -a(自动安装)/m(在 VNC 里手动安装) -mirror "系统镜像源，系统安装完成后的默认软件安装包源也是这个" -firmware(带固件) --cdimage "cn"(此项仅适用于在国内要重装成 Debian 的 VPS)
复制代码


快速上手：

Debian 8

bash InstallNET.sh -d 8 -v 64 -a
复制代码


Debian 9

bash InstallNET.sh -d 9 -v 64 -a
复制代码


Debian 10

bash InstallNET.sh -d 10 -v 64 -a
复制代码


Debian 11 (选择带固件安装时，推荐国内机器安装源的命令)

清华:

bash InstallNET.sh -d 11 -v 64 -a -mirror "https://mirrors.tuna.tsinghua.edu.cn/debian/" -firmware --cdimage "cn"
复制代码


网易:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://mirrors.163.com/debian/" -firmware --cdimage "cn"
复制代码


腾讯云:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://mirrors.cloud.tencent.com/debian/" -firmware --cdimage "cn"
复制代码


阿里云:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://mirrors.aliyun.com/debian/" -firmware --cdimage "cn"
复制代码


Debian 11 (如果你的机器在中国大陆以外，连接速度都不会差，就近选择官方源即可)

日本:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.riken.jp/Linux/debian/debian/" -firmware
复制代码


香港:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.hk.debian.org/debian/" -firmware
复制代码


新加坡:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.sg.debian.org/debian/" -firmware
复制代码


韩国:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://studenno.kugi.kyoto-u.ac.jp/debian/" -firmware
复制代码


台湾:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.tw.debian.org/debian/" -firmware
复制代码


美国:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://debian.csail.mit.edu/debian/" -firmware
复制代码


加拿大:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.ca.debian.org/debian/" -firmware
复制代码


英国:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.uk.debian.org/debian/" -firmware
复制代码


德国:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.de.debian.org/debian/" -firmware
复制代码


法国:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.fr.debian.org/debian/" -firmware
复制代码


俄罗斯:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.ru.debian.org/debian/" -firmware
复制代码


澳大利亚:

bash InstallNET.sh -d 11 -v 64 -a -mirror "http://ftp.au.debian.org/debian/" -firmware
复制代码


Debian 当地源的格式基本上都是“http://ftp.地区名称缩写.debian.org/debian/”，如果以上例子里不包含你 VPS 所在地，去 https://zh.wikipedia.org/zh-sg/%E5%9C%8B%E5%AE%B6%E5%9C%B0%E5%8D%80%E4%BB%A3%E7%A2%BC （国家地区代码 - Wiki） 找到对应的，替换掉上面链接里“地区名称缩写”位置，放浏览器里访问一下，如果出现一个文件服务器目录，即可使用。日本的 Debian 源来自日本理化学研究所，一个科研机构；韩国的 Debian 官方源 ftp.kr.debian.org 总是宕机，换成了京都大学的；美国的 Debian 源来自麻省理工学院，请放心使用。

通用的 Debian 源链接：http://ftp.debian.org/debian/

--------------------------------------------------------------------------

Ubuntu 16.04

bash InstallNET.sh -u 16.04 -v 64 -a
复制代码


Ubuntu 18.04

bash InstallNET.sh -u 18.04 -v 64 -a
复制代码


Ubuntu 20.04

bash InstallNET.sh -u 20.04 -v 64 -a
复制代码


--------------------------------------------------------------------------

Cent OS 6

bash InstallNET.sh -c 6.10 -v 64 -a
复制代码
