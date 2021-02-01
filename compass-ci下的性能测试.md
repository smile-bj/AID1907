如果您已经拥有一个可以提交测试任务的环境，

目前已在compass-ci平台完成lmbench,iozone,netperf,netperf,mutex等性能工具在不同OS的调测。

# 1. 目前compass-ci提供的OS镜像

OS                      version
openEuler               20.03
oepnEuler		20.09
CentOS			7.6
CentOS			8.1

## 1.1 制作作自定义的rootfs:

# 2. compass-ci提供丰富的依赖包，详情见


## 2.1 如何添加测试所需的依赖包

### 以netperf举例，用户可通过在lkp-tests/distro/depends/下不同OS添加netperf及netperf-dev文件
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
### 提交cci-depends.yaml, [如何提交测试任务详见][https://gitee.com/wu_fengguang/compass-ci/blob/master/doc/manual/submit-job.en.md]

## 2.2 如何添加测试软件包 

### 编写pkgbuild(注：pkgbuild可来源于代码库上直接下载，也可个人编写)：
[如何编写PKGBUILD][https://gitee.com/wu_fengguang/compass-ci/blob/master/doc/manual/write-PKGBUILD.zh.md]

### 提交cci-makepkg.yaml, [如何提交测试任务详见][https://gitee.com/wu_fengguang/compass-ci/blob/master/doc/manual/submit-job.en.md]

# 3. MySQL测试

## 3.1 通过submit方式提交job.yaml，提交完成后等待测试完成

## 3.2  登录到 https://compass-ci.openeuler.org/jobs/页面

## 3.3 根据submit返回的job_id找到任务，在job_state列点击finished/failed进入到结果页面

## 3.4 找到任务同名的文件打开如下：

## 3.5 MySQL测试指标：

Transactions: --总事务数/秒
一台数据库服务器在单位时间内处理梳理事务的个数。
