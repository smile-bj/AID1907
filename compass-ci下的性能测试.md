如果您已经拥有一个可以提交测试任务的环境，
目前已在compass-ci平台完成lmbench,iozone,netperf,netperf,mutex等性能工具在不同OS的调测。

# 1. 目前compass-ci提供的OS镜像

OS                      version
openEuler               20.03
oepnEuler		20.09
CentOS			7.6
CentOS			8.1

## 1.1 制作作自定义的rootfs:

# 2. compass-ci提供丰富的依赖包


# 2.1 如何添加测试所需的依赖包
w
## 以netperf举例，用户可通过在lkp-tests/distro/depends/下不同OS添加netperf及netperf-dev文件
netperf文件为本次测试过程所需的依赖：
```
libsctpl
lksctp-tools
ethtool
```
netperf-dev文件为构建netperf包提供依赖
```
libsctp-dev
```
## 提交cci-depends.yaml

## 2.1 pkgbuild脚本如下(注：pkgbuild可来源于代码库上直接下载，此处pkgbuild为个人编写)：

'''
pkgname=mysql
pkgver=8.0.17
pkgrel=3
arch=('aarch64' 'x86_64')
url="http://www.mysql.com"
license=('GPLv2')
source=("https://cdn.mysql.com/archives/mysql-8.0/mysql-boost-$pkgver.tar.gz"
	"0000-mysql-add-fstack-protector-strong.patch"
	"https://mirrors.huaweicloud.com/kunpeng/archive/openEuler/patch/database/mysql/getcpu.tgz"
	"my.cnf")
md5sums=('7472a25d91973cbba5d8a8f176a3080b' '6e853850efbd6f887ac3e13b1bf99cc4' 'f13eb11de53616017f7428cc06d4e242' '7bb0372ec4e9281813a9afc67c6cfc20')

prepare()
{
	cd $srcdir/$pkgname-$pkgver
	patch -p1 < ../0000-mysql-add-fstack-protector-strong.patch
}

build()
{
	build_getcpu
	build_mysql
}

build_mysql()
{
	cd $srcdir/$pkgname-$pkgver

	CMAKE=cmake
	grep -sqF "CentOS Linux release 7" /etc/centos-release && CMAKE=cmake3

	$CMAKE . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/data/mysql/data -DSYSCONFDIR=/etc \
                -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_FEDERATED_STORAGE_ENGINE=1 \
                -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_MYISAM_STORAGE_ENGINE=1 \
                -DENABLED_LOCAL_INFILE=1 -DENABLE_DTRACE=0 -DDEFAULT_CHARSET=utf8mb4 -DDEFAULT_COLLATION=utf8mb4_general_ci \
                -DWITH_EMBEDDED_SERVER=1 -DDOWNLOAD_BOOST=1 -DWITH_BOOST=./boost -DFORCE_INSOURCE_BUILD=1
	make -j8
}
'''

## 2.2 编译成功的的包文件如下：

/srv/initrd/pkg/initramfs/openeuler/aarch64/20.03/mysql/8.0.17-3.cgz -> mysql.cgz

# 3. MySQL测试

## 3.1 通过submit方式提交job.yaml，提交完成后等待测试完成

## 3.2  登录到 https://compass-ci.openeuler.org/jobs/页面

## 3.3 根据submit返回的job_id找到任务，在job_state列点击finished/failed进入到结果页面

## 3.4 找到任务同名的文件打开如下：

 SQL statistics:
    queries performed:
        read:                    210212086
        write:                   60060591
        other:                   30030296
        total:                   300302973
    transactions:                15015147 (25024.26 per sec.)
    queries:                     300302973 (500485.16 per sec.)

## 3.5 MySQL测试指标：

Transactions: --总事务数/秒
一台数据库服务器在单位时间内处理梳理事务的个数。
