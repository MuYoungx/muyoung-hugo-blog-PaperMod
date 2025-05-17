# 安装



![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122306462.png)

点击 i agree进行下一步

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122309691.png)

选择自己要安装pve操作系统的硬盘(pve宿主机的位置不是虚拟机哦)

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122311800.png)

这一步如果当前网络环境有网络并且dhcp获取到ip地址会默认获取国家跟时区直接下一步，如果没有网络环境这一步需要手动输入时区以及国家

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122420062.png)

设置管理员密码以及管理员邮箱如果有事件会发邮件通知的哦！！

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122313256.png)

这一步默认会选择已经连接的管理口网卡设置想要的域名以及静态ip地址

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122435675.png)

最终确定然后点击install安装

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122315032.png)

耐心等待即可

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122445553.png)

安装完毕后请拔出u盘这时屏幕会显示管理的地址如果提示不是私密连接点击高级，继续访问即可

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122317089.png)

输入用户名root 密码为安装界面设置的密码

# 移除pve里的local-lvm分区

在我们安装好pve后我们会发现有一个local分区以及local-lvm分区吧我们的硬盘分开来，这样很不利于我们的空间使用所以我们要把local-lvm分区删除然后把空间全部合在local分区里让空间利用率最大化

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122319640.png)

点击节点然后点击shll输入如下命令

```bash
lvremove pve/data
lvextend -l +100%FREE -r pve/root
```

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122321488.png)

提示是否移除卷输入y

点击数据中心-存储

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122512648.png)

选中local-lvm点击移除然后选中local点击编辑

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122323616.png)

选上所有内容然后点击ok

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122325680.png)

这时我们会发现空间利用率最大化了

# 调整swap分区

如果有想要调整swap分区的小伙伴可以看如下操作默认系统安装好会有一部分的swap分区如果你不想使用或者觉得swap分区的空间不够可以跟着操作来调整

## 删除pve自带的swap分区(如果内存够大并且不想时常读写硬盘想保护硬盘的小伙伴可以按照以下部分设置)

进入Shell输入如下命令

```bash
swapoff -a
lvremove /dev/pve/swap
lvresize -l +100%FREE /dev/pve/root
```

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122328026.png)

这样swap分区就被移除了并且空间集合到了主空间里面去

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122330696.png)

## 创建swap分区

想自定义swap分区大小的小伙伴可以按照以下步骤操作

打开Shell输入如下命令

```bash
#创建一个16G的swap，bs * count =16G   count代表你想创建的swap分区的大小单位为g
dd if=/dev/zero of=/swapfile bs=1G count=16
#配置安全的权限
chmod 0600 /swapfile
#格式化成swap
mkswap /swapfile
#挂载swap
swapon /swapfile
#验证
free -h
```

![这样swap分区就创建好了](https://obj.muyoung.com/blogimg/20250517122332868.png)

## 开机自动挂载swap分区

打开shell继续输入如下命令 

```bash
nano /etc/fstab       
/swapfile  swap      swap    defaults   0       0
```

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122334994.png)

然后Ctrl+X 输入Y

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122336954.png)

然后按回车退出就配置好了

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122539694.png)

# 更改国内源删除订阅弹窗

```bash
# 将此文件的中的所有内容注释掉
nano /etc/apt/sources.list.d/pve-enterprise.list

# 下载中科大的GPG KEY
wget https://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg

# 使用Proxmox非企业版源
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list

# 将Debian官方源替换为中科大源
sed -i 's|^deb http://ftp.debian.org|deb https://mirrors.ustc.edu.cn|g' /etc/apt/sources.list
sed -i 's|^deb http://security.debian.org|deb https://mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list

# 替换Ceph源
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/ceph-quincy bookworm no-subscription" > /etc/apt/sources.list.d/ceph.list

# 替换CT镜像下载源
sed -i 's|http://download.proxmox.com|https://mirrors.ustc.edu.cn/proxmox|g' /usr/share/perl5/PVE/APLInfo.pm

#删除订阅弹窗
sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{ \/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy.service
# 执行完成后，浏览器Ctrl+F5强制刷新缓存
```

# 硬件直通

 什么是硬件直通(Passthrough) VT-d 、DirectPath I/O，通过 DirectPath I/O，虚拟机可以使用 I/O 内存管理单元访问平台上的物理 PCI 功能，就是俗称的虚拟化直通，简单理解就是允许宿主机将某些硬件资源的管辖权直接移交给虚拟机，虚拟机会以直通独占的方式使用硬件，宿主机将不能再使用此硬件，利用效率几乎等同于将硬件插到了虚拟机的主板扩展槽里一样，最实用的目的是避免了虚拟化平台自身软件层转换带来的效能下降。

  典型应用场景，例如在服务器上将某个物理网卡直接划给某台虚拟机使用，以达到几乎和物理机搭配物理网卡类似的网络性能。更可观的场景是，将磁盘控制器直通给虚拟机独占使用，那么虚拟机往往最瓶颈的磁盘性能，将得到非常可观的提升。

  我们在Proxmox VE(Proxmox Virtual Environment)PVE系统操作添加: PCI设备 硬件直通提示：No IOMMU detected, please activate it.See Documentation for further information.【翻译：未开启IOMMU，请设置开启激活，更多有关更多信息，请参阅文档。】

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122339880.png)

PVE系统添加PCI设备直通时提示：No IOMMU detected界面

是因为默认ProxmoxVE PVE系统只能支持硬盘、CPU型号直通。其他PCI硬件，例如：网卡 或者 核心显卡的直通，还需要开启IOMMU分组功能。

在Proxmox VE(PVE)系统开启IOMMU功能实现硬件直通之前，我们要确认CPU是否支持VT-D技术；

开启直通的必要条件 CPU支持VT-D，同时主板要开启VT-D支持。

## 查询CPU是否支持VT-D

1.[点击进入Intel官方网站](https://www.intel.cn/)或 AMD 官方网站【[AMD ׀ 同超越，共成就 \_ 人工智能](https://www.amd.com/zh-hans)】，搜索对应处理器型号(例如：i7-7700【[传送门](https://ark.intel.com/content/www/cn/zh/ark/products/97122/intel-core-i7-7700t-processor-8m-cache-up-to-3-80-ghz.html#tab-blade-1-0-7)】)

如果看到下图内容，则说明CPU支持VT-D技术

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122343448.png)

## 启用IOMMU功能

Intel CPU

对于Intel CPU，添加 intel\_iommu=on，操作如下：

```bash
1、Shell 里面输入命令：
nano /etc/default/grub
root@pve:~# nano /etc/default/grub
2、在里面找到：GRUB_CMDLINE_LINUX_DEFAULT="quiet"然后修改为
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
编辑完成后，使用快捷键 Ctrl + O 回车保存文件，Ctrl + X 退出编辑器。
3、使用命令 update-grub 保存更改并更新grub
root@pve:~# update-grub
4、更新完成后，使用命令 reboot 重启PVE系统
root@pve:~# reboot
从命令行运行 dmesg | grep -e DMAR -e IOMMU 如果没有输出，则说明有问题。
如果有,可基本确认这个过程顺利完成! 接下来就可以为虚拟机正常的添加硬件直通了。
```

AMD CPU

对于AMD CPU 添加 amd\_iommu=on, 操作如下：

```bash
1、Shell 里面输入命令：nano /etc/default/grub
root@pve:~# nano /etc/default/grub
2、在里面找到：GRUB_CMDLINE_LINUX_DEFAULT="quiet"
然后修改为
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
编辑完成后，使用快捷键 Ctrl + O 回车保存文件，Ctrl + X 退出编辑器。
3、使用命令 update-grub 保存更改并更新grub
root@pve:~# update-grub
4、更新完成后，使用命令 reboot 重启PVE系统
root@pve:~# reboot
从命令行运行 dmesg | grep -e DMAR -e IOMMU 如果没有输出，则说明有问题。
如果有,可基本确认这个过程顺利完成! 接下来就可以为虚拟机正常的添加硬件直通了。
```

增加虚拟化驱动，加载vifo系统模块

这仅在必要时启用IOMMU转换，将iommu分组相关的内核模块启用，从而可以提高VM中未使用的PCIe设备的性能。

然后是修改 /etc/modules 文件

```bash
root@pve:~# nano /etc/modules
```

添加如下内容

```bash
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

![请在此添加图片描述](https://obj.muyoung.com/blogimg/20250517122348735.png)

PVE系统添加PCI设备开启硬件直通界面 

如果按照此方法：ProxmoxVE 开启硬件直通 还设置无效，请再次检查自己的CPU支持VT-D技术。

**注意：虚拟机进行直通操作时，取消勾选开机自启动的选项，这样哪怕直通错误，只需重启一下物理机就可以了，因为虚拟机没有自启的原因就不会直通，不会导致冲突无法开机使用。**