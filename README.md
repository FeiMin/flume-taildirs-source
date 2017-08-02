# flume-taildirs-source
基于 flume 1.7 taildir source 更新优化，同时保持 1.7 版本兼容性。

## 应用场景
某些场景下，例如采集 java 输入日志，需要将异常栈（如下所示）读取为一行。

```
[DEBUG] [2017-01-19 11:33:41][org.apache.hadoop.util.NativeCodeLoader]Trying to load the custom-built native-hadoop library...
[DEBUG] [2017-01-19 11:33:41][org.apache.hadoop.util.NativeCodeLoader]Failed to load native-hadoop with error: java.lang.UnsatisfiedLinkError: no hadoop in java.library.path
[DEBUG] [2017-01-19 11:33:41][org.apache.hadoop.security.JniBasedUnixGroupsMappingWithFallback]Group mapping impl=org.apache.hadoop.security.ShellBasedUnixGroupsMapping
[DEBUG] [2017-01-19 11:33:42][org.apache.hadoop.util.Shell]Failed to detect a valid hadoop home directory
java.io.IOException: HADOOP_HOME or hadoop.home.dir are not set.
	at org.apache.hadoop.util.Shell.checkHadoopHome(Shell.java:265)
	at org.apache.hadoop.util.Shell.<clinit>(Shell.java:290)
	at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:76)
	at org.apache.hadoop.security.Groups.parseStaticMapping(Groups.java:93)
	at org.apache.hadoop.security.Groups.<init>(Groups.java:77)
	at org.apache.hadoop.security.Groups.getUserToGroupsMappingService(Groups.java:240)
	at org.apache.hadoop.security.UserGroupInformation.initialize(UserGroupInformation.java:255)
	at org.apache.hadoop.security.UserGroupInformation.ensureInitialized(UserGroupInformation.java:232)
	at org.apache.hadoop.security.UserGroupInformation.loginUserFromSubject(UserGroupInformation.java:718)
	at org.apache.hadoop.security.UserGroupInformation.getLoginUser(UserGroupInformation.java:703)
[DEBUG] [2017-01-19 11:35:46][org.apache.hadoop.security.UserGroupInformation]hadoop login
[DEBUG] [2017-01-19 11:35:46][org.apache.hadoop.security.UserGroupInformation]hadoop login commit
```

## new feature
- 将 java 异常栈类型的日志读为一行
- 记录行号到 position file

## how to use
### 部署
`maven clean package`  将打出的 jar 包拷贝至 flume lib 目录下
### 配置
在配置文件中新增配置`linePrefix`，配置行首开始的正则表达式，例如
a1.sources.r1.linePrefix = \\[.*
如果不需要将栈读取为一行 置为null或者空即可


