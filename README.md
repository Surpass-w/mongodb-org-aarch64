### Mongodb 3.4.21 aarch64

编译环境

| Os   | Kylin v10 sp2 |
| ---- | ------------- |
| Cpu  | Kunpeng 920   |

官方下载的源码编译mongo&mongo tools

编译依赖安装

gcc > 7

```shell
yum -y install rpm-build python2-scons libpcap-devel
```

Mongo tools 编译

下载 mongo tools 源码,使用go 编译

```shell
tar -C /usr/local -zxvf go1.17.5.linux-arm64.tar.gz
tar zxvf mongo-tools-r3.4.21.tar.gz && cd mongo-tools-r3.4.21
chmod +x set_goenv.sh && source set_goenv.sh
cd /root && mkdir -p mongodb-tools-3.4.21/src/github.com/mongo
mv mongo-tools-r3.4.21 mongodb-tools-3.4.21/src/github.com/mongodb/mongo-tools
cd /root/mongodb-tools-3.4.21/src/github.com/mongodb/mongo-tools/vendor/github.com/google/ && rm -rf gopacket
git clone https://github.com/google/gopacket.git
```

建立gcc 软链接 && 编译

```shell
mkdir -p /opt/mongodbtoolchain/v2/bin
ln -s /usr/bin/gcc /opt/mongodbtoolchain/v2/bin/aarch64-mongodb-linux-gcc

cd /root/mongodb-tools-3.4.21/src/github.com/mongodb/mongo-tools
export PATH=$PATH:/usr/local/go/bin
export GOROOT="/usr/local/go"
go env -w GO111MODULE=off

./build.sh
```

编译会生存tools 的二进制文件，这里已经把tools的二进制collect到mongodb-org-r3.4.21中的tools中，无需编译

编译rpm包，将mongodb-org.spec放到rpmbuild/SPECS下,mongodb-org-r3.4.21.tar.gz放到rpmbuild/SOURCES下

```shell
rpmbuild  -ba --target=aarch64  -D"dynamic_version r3.4.21" -D "dynamic_release r3.4.21"  -D "_arch aarch64" SPECS/mongodb-org.spec
```
