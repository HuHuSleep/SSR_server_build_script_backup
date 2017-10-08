# Ubuntu包更新

apt-get 相关的升级更新命令有四个：

apt-get update

apt-get upgrade

apt-get full-upgrade

apt-get dist-upgrade

那么，这四个升级命令都有什么差别呢？

根据 apt-get 的官方手册：

update - 从服务器更新可用的软件包列表。

upgrade - 根据列表，更新已安装的软件包。upgrade 不会删除在列表中已经没有的软件包，也不会安装有依赖需求但尚未安装的软件包。

full-upgrade - 根据列表，更新已安装的软件包。full-upgrade 可能会为了解决软件包冲突而删除一些已安装的软件包。

dist-upgrade - 根据列表，更新已安装的软件包。dist-upgrade 可能会为了解决软件包冲突而删除一些已安装的软件包，也可能会为了解决软件包依赖问题安装新的软件包。

我们应该怎么做？

依次执行如下命令即可：

sudo apt-get update

sudo apt-get upgrade

sudo apt-get dist-upgrade

sudo reboot



# 单独给ShadowsocksR服务端指定 DNS

此方法适合给ShadowsocksR服务端单独设置一个DNS服务器，而不想去修改系统默认的DNS，如果无所谓了，那么也可以通过修改系统默认的DNS来修改 ShadowsocksR服务端使用的DNS。

假设你的ShadowsocksR服务端安装在 /root 目录中，那么：

ShadowsocksR根目录(多用户)，即目录为 /root/shadowsocksr/

ShadowsocksR子目录(单用户)，即目录为 /root/shadowsocksr/shadowsocks

根据当前在使用的单用户/多用户方式，来选择在对应的目录下 新建 dns.cof 文件，格式如下：

8.8.8.8 53

8.8.4.4 53

每行一个DNS服务器，端口为 53时，可忽略不写。

可以用 vi / nano 来新建/编辑修改，也可以用 echo 命令直接写入，比如：

根目录
echo -e "8.8.8.8 53

8.8.4.4 53" > /root/shadowsocksr/dns.conf
 
子目录
echo -e "8.8.8.8 53

8.8.4.4 53" > /root/shadowsocksr/shadowsocks/dns.conf

此方法具有最高优先级，当ShadowsocksR服务端启动时会检测目录下是否有 dns.conf 文件，如果有并格式正确，那就会直接读取这个 DNS配置，而不会读取 系统的DNS配置。


修改系统默认的 DNS

Linux的默认DNS配置文件为： /etc/resolv.conf

修改内容 格式如下：

nameserver 8.8.8.8

nameserver 8.8.4.4

可以用 vi / nano 来新建/编辑修改，也可以用 echo 命令直接写入，比如：

echo -e "nameserver 8.8.8.8

nameserver 8.8.4.4" > /etc/resolv.conf

此方法仅支持53端口，不需填写端口号。

此方法具有次级优先级，当ShadowsocksR服务端目录下没有 dns.conf 文件时，ShadowsocksR便会读取这个系统默认的DNS。


