# UPSTREAM REPOS

## 这是一个用于存放所有上游仓库地址的仓库，compass-ci服务器会实时监控着这个仓库中注册的所有上游仓库，当您的仓库有commit提交时，测试任务会自动执行，并且可在我们的网站中查看结果。

## 添加文件
1.第一步：fork需要注册的上游仓库

2.第二步：创建目录及注册文件

目录创建规则：第一层目录的名称以上游仓库名称首个字符命名，第二层目录的名称以上游仓库名称命名，第二层目录下的文件以上游仓库名称命名

以mongodb/mongo举例：
```
mkdir -p m/mongo
echo 'url: https://github.com/mongodb/mongo.git' >  m/mongo/mongo
```
3.第三步：通过pull request的方式完成注册

## 自动触发测试任务

同样是采用Pull Request的方式将适配好的测试用例添加到compass-ci仓库下面的 sbin/auto_submit.yaml 文件中。
如何适配适配测试用例到compass-ci请参考：

	https://gitee.com/EmmaLee/lkp-tests/blob/master/doc/add-testcase.md

将所有的接口文件通过Pull Request的方式迁移到compass-ci中后，在 auto_submit.yaml 中补充测试参数：

	echo "$repo/$repo: os=openeuler" >> auto_submit.yaml
