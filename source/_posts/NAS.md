# 安装CentOS

SMJB

## Q&A：

### 忘记开网卡了

1. 使用root账户查看网关

   ````shell
   su root
   #输入密码
   cat /etc/resolv.conf
   ````

   或者可以在同级路由的windows查看网关

   ````powershell
   ipconfig
   ````

2. 修改网卡配置文件，保存并退出

   ````shell
   cd /etc/sysconfig/network-scripts/
   dir
   vi ifcfg-eth0
   #ONBOOT=yes
   #BOOTPROTO=static
   IPADDR=192.168.189.233
   NETMASK=255.255.255.0
   GATEWAY=192.168.176.1
   #esc : w q enter
   ````

3. 重启网卡，并查看ip
   
   ````shell
   systemctl restart network && ip addr
   ````

   

## 没把用户设置成管理员

1. 编辑/etc/sudoers
   
    ````shell
    vi /etc/sudoers
    #在root    ALL=(ALL)     ALL
    #添加
    #用户名     ALL=(ALL)     ALL
    #esc : w q ! enter
    ````



# 安装Xfce

1. 安装额外包yum源和更新系统

    ````shell
    sudo yum install epel-release
    sudo yum update
    ````

2. 安装Xfce，检验并设为默认桌面

    ````shell
    yum groupinstall xfce
    sudo systemctl isolate graphical.target
    sudo systemctl set-default graphical.target
    ````

4. 安装中文字体

    ````shell
    yum install cjkuni-ukai-fonts
    ````

5. 安装Firefox

    ````shell
    yum install firefox
    ````

# 安装xrdp

1. 依次安装xrdp所需要的环境

    ````shell
    yum install xrdp tigervnc tigervnc-server
    ````
    
    ````shell
    sudo apt-get install tightvncserver
    ````
    
2. 设置vnc密码

    ````shell
    vncpasswd
    # 设置当前用户的vnc密码 如果要设置用户 k 的 vnc 登陆密码，则先切换用户
    su k
    vncpasswd
    # 当然，你也可以再设置一个view-only passwd
    ````

3. 打开3389端口，并重启防火墙

    ````shell
    sudo firewall-cmd --permanent --zone=public --add-port=3389/tcp
    sudo firewall-cmd --reload
    ````

4. 启动xrdp服务，并设置开机启动

   ````shell
   sudo systemctl start xrdp
   sudo systemctl enable xrdp
   # 此外，你可以通过sudo systemctl stop|status|restart xrdp 来停止|查看状态|重启 xrdp服务
   ````

5. 修改xrdp配置

   ````shell
   vi /etc/xrdp/xrdp.ini
   
   ; set SSL protocols
   ; can be comma separated list of 'SSLv3', 'TLSv1', 'TLSv1.1', 'TLSv1.2', 'TLSv1.3'
   ssl_protocols=TLSv1, TLSv1.1, TLSv1.2
   ````

6. 重启xrdp服务

   ````shell
   sudo systemctl restart xrdp
   ````

7. 现在可以通过微软的mstsc连接了

   ````shell
   su k
   touch ~/.Xclients
   echo "xfce4-session" > ~/.Xclients
   chmod +x ~/.Xclients
   ````

   

如果不行的话，放行selinux

       ````shell
       chcon -t bin_t /usr/sbin/xrdp
       chcon -t bin_t /usr/sbin/xrdp-sesman
       ````


8. 如果还是不行

   ````shell
   vi /etc/selinux/config
   SELINUX=disabled
   ````

   

# 安装宝塔

````shell

````



# 安装SMB

````shell
yum install samba
systemctl start|stop|status|restart smb
firewall-cmd --add-service samba --permanent
firewall-cmd --reload

````



# KMS服务器

````shell
cd /opt
在宝塔上传kmspro.sh到/opt
chmod +x kmspro.sh && bash kmspro.sh centos
bash kmspro.sh start|restart|status|uninstall
bash kmspro.sh auto
````



# 安装百度网盘

安装依赖文件

````
yum install libXScrnSaver xdg-utils
````

安装百度网盘

````shell
rpm -i http://wppkg.baidupcs.com/issue/netdisk/Linuxguanjia/3.3.2/baidunetdisk-3.3.2.x86_64.rpm
````

在宝塔中找到文件夹/usr/lib64并删除libstdc++.so.6

````shell
yum provides libstdc++.so.6
cd /usr/lib64
ln libstdc++.so.6.0.20 libstdc++.so.6
````

# 安装新硬盘

````shell
fdisk -l
parted /dev/sdc
print
mklabel gpt
mkpart extended 0% 100%
print
quit
mount /dev/sdc1 /文件夹位置
umount /dev/sdc1
pvcreate /dev/sdc1
vgextend centos /dev/sdc1
pvscan
mkfs.xfs -f /dev/sdc1

````

# 双网卡链路聚合

````bash
su root
nmcli connection add type team con-name team0 ifname team0 config '{"runner":{"name":"activebackup"}}'
nmcli connection modify team0 team.config '{"runner":{"name":"loadbalance"}}'
nmcli connection modify team0 team.config '{"runner":{"name":"activebackup"}}'
nmcli connection add type team-slave con-name team0-1 ifname enp2s0 master team0
nmcli connection add type team-slave con-name team0-2 ifname enp3s0 master team0
nmcli connection show
nmcli connection up team0-1
nmcli connection up team0-2
nmcli connection modify team0 ipv4.addresses '192.168.2.233/24'
````

更换内核

````shell
egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
````

