如果您已经拥有一个可以提交测试任务的环境，

目前已在compass-ci平台完成lmbench,iozone,netperf,netperf,mutex等性能工具在不同OS的调测。

# 1. 目前compass-ci提供的OS镜像

OS version: openEuler 20.03, openEuler 20.09, CentOS 7.6, CentOS 8.1

## 1.1 制作作自定义的rootfs:

# 2. compass-ci提供丰富的依赖包

lkp依赖包[https://gitee.com/wu_fengguang/lkp-tests/blob/master/distro/depends/lkp]

## 2.1 如何添加测试所需的依赖包

### 以mysql为例，用户可通过在lkp-tests/distro/depends/下不同OS添加mysql-server文件
mysql-server文件为本次测试过程所需的依赖：
```
mysql-server
mysql-testsuite
```
### 执行如下命令提交构建依赖包任务，如何提交测试任务详见[https://gitee.com/wu_fengguang/compass-ci/blob/master/doc/manual/submit-job.en.md]
```
submit cci-depends.yaml benchmark=mysql-server os_mount=cifs testbox=vm-2p8g
```
### 可登陆到https://compass-ci.openeuler.org/jobs/页面查该任务是否成功

## 2.2 如何添加测试软件包 

### 以mysql为例，用户可通过在lkp-tests/distro/depends/下不同OS添加mysql-dev文件
mysql-dev文件为构建测试过程所需的依赖：
```
gcc
cmake
libssl-dev
libncurses5-dev
libtirpc-dev
```

### 编写pkgbuild(注：pkgbuild可来源于代码库上直接下载，也可个人编写)：
[如何编写PKGBUILD][https://gitee.com/wu_fengguang/compass-ci/blob/master/doc/manual/write-PKGBUILD.zh.md]

### 执行如下命令提交构建依赖包任务

```
submit cci-makepkg.yaml benchmark=mysql-server os_mount=cifs testbox=vm-2p8g
```

# 3. MySQL测试

## 3.1 通过submit方式提交job.yaml，提交完成后等待测试完成

## 3.2  登录到 https://compass-ci.openeuler.org/jobs/页面

## 3.3 根据submit返回的job_id找到任务，在job_state列点击finished/failed进入到结果页面

## 3.4 找到任务同名的文件打开如下：

## 3.5 MySQL测试指标：

Transactions: --总事务数/秒
一台数据库服务器在单位时间内处理梳理事务的个数。
