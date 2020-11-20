## 概述

作为一名开发者，相信对adb指令一定不会陌生。那么在手机连接adb后，可通过am命令做很多操作：

#### 拨打电话

通过adb，可以直接拨打电话10086

    adb shell am start -a android.intent.action.CALL -d tel:10086


#### 打开网站

比如，打开网站www.gityuan.com

    adb shell am start -a android.intent.action.VIEW -d  http://gityuan.com


#### 启动应用

比如，启动包名为com.gityuan.app，主Activity为`.MainActivity`，且extra数据以”website”为key, “gityuan.com”为value。通过java代码要完成该功能虽然不复杂，但至少需要一个android环境，而通过adb的方式，只需要在adb窗口，输入如下命令便可完成:

    am start -n com.gityuan.app/.MainActivity -es website gityuan.com


am命令还可以启动Service、Broadcast，杀进程，监控等功能，这些功能都非常便捷调试程序，接下来讲述关于am更多更详细的功能。

## Am命令

### 2.1 命令列表

命令格式如下：

    am [subcommand] [options]


命令列表如下：
命令功能实现方法am start  `[options`]  `>`启动ActivitystartActivityAsUseram startservice  `>`启动ServicestartServiceam stopservice  `>`停止ServicestopServiceam broadcast  `>`发送广播broadcastIntentam kill  `>`杀指定后台进程killBackgroundProcessesam kill-all杀所有后台进程killAllBackgroundProcessesam force-stop  `>`强杀进程forceStopPackageam hang系统卡住hangam restart重启restartam bug-report创建bugreportrequestBugReportam dumpheap `> `>``进程pid的堆信息输出到filedumpheapam send-trim-memory  `> `>``收紧进程的内存setProcessMemoryTrimLevelam monitor监控MyActivityController.run
am命令实的实现方式在Am.java，最终几乎都是调用`ActivityManagerService`相应的方法来完成。

- 比如命令`am start -a android.intent.action.VIEW -d  http://gityuan.com`， 启动Acitivty最终调用的是ActivityManagerService类的startActivityAsUser()方法来完成的。
- 再比如 `am kill-all`命令，最终的实现工作是由ActivityManagerService的killBackgroundProcesses()方法完成的。

接下来，说说`[options`]和 `>参数含义和使用。`

### 2.2 Activity启动命令

先来介绍一下启动Activity命令`am start [options] `使用options参数，接下来列举Activity命令的[options]参数：

- -D: 允许调试功能
- -W: 等待app启动完成
- -R `>: 重复启动Activity COUNT次`
- -S: 启动activity之前，先调用forceStopPackage()方法强制停止app.
- –opengl-trace: 运行获取OpenGL函数的trace
- –user `> `|` current: 指定用户来运行App,默认为当前用户。`
- –start-profiler `>: 启动profiler，并将结果发送到 `>;``
- -P `>: 类似 –start-profiler，不同的是当app进入idle状态，则停止profiling`
- –sampling INTERVAL: 设置profiler 取样时间间隔，单位ms;

启动Activity的实现原理： 存在-W参数则调用startActivityAndWait()方法来运行，否则startActivityAsUser()。

### 2.3 trim-memory命令

收紧内存命令

    am send-trim-memory  

例如： 向pid=12345的进程，发出level=RUNNING_LOW的收紧内存命令

    am send-trim-memory 12345 RUNNING_LOW。


那么level取值范围为： HIDDEN、RUNNING_MODERATE、BACKGROUND、RUNNING_LOW、MODERATE、RUNNING_CRITICAL、COMPLETE。

### 2.4 Intent参数

Intent的参数和flags较多，本文为方便起见，分为3种类型参数，常用参数，Extra参数，Flags参数。

#### 2.4.1 常用参数

- `-a `: 指定Intent action， 实现原理Intent.setAction()；
- `-n `: 指定组件名，格式为{包名}/.{主Activity名}，实现原理Intent.setComponent(）；
- `-d `: 指定Intent data URI
- `-t `: 指定Intent MIME Type
- `-c  [-c ] ...]`:指定Intent category，实现原理Intent.addCategory()
- `-p `: 指定包名，实现原理Intent.setPackage();
- `-f `: 添加flags，实现原理Intent.setFlags(int )，紧接着的参数必须是int型；

实例

    am start -a android.intent.action.VIEW
    am start -n com.gityuan.app/.MainActivity
    am start -d content://contacts/people/1
    am start -t image/png
    am start -c android.intent.category.APP_CONTACTS


### 2.4.2 Extra参数

**(1). 基本类型**
参数-e/-es-esn-ez-ei-el-ef-eu-ecn类型String(String)nullbooleanintlongfloaturicomponent
比如参数es是Extra String首字母简称，实例：

    am start -n com.gityuan.app/.MainActivity -es website gityuan.com


此处`-es website gityuan.com`，等价于Intent.putExtra(“website”, “gityuan.com”);

**(2). 数组类型**
参数-esa-eia-ela-efa数组类型String[]int[]long[]float[]
比如参数eia，是Extra int array首字母简称，多个value值之间以逗号隔开，实例：

    am start -n com.gityuan.app/.MainActivity -ela weekday 1,2,3,4,5


此处`-ela weekday 1,2,3,4,5`，等价于Intent.putExtra(“weekday”, new int[]{1,2,3,4,5});

**(3). ArrayList类型**
参数-esal-eial-elal-efalList类型Stringintlongfloat
比如参数efal，是Extra float Array List首字母简称，多个value值之间以逗号隔开，实例：

    am start -n com.gityuan.app/.MainActivity -efal nums 1.2,2.2


此处`-efal nums 1.2,2.2`，等价于先构造ArrayList变量，再通过putExtra放入第二个参数。

### 2.4.3 Flags参数

在参数类型1中，提到有`-f `，是通过`Intent.setFlags(int )`方法，来设置Intent的flags.本小节也是关于flags，是通过`Intent.addFlags(int )`方法。如下所示，所有的flags参数。

    [--grant-read-uri-permission] [--grant-write-uri-permission]
    [--grant-persistable-uri-permission] [--grant-prefix-uri-permission]
    [--debug-log-resolution]
    [--exclude-stopped-packages] [--include-stopped-packages]
    [--activity-brought-to-front] [--activity-clear-top]
    [--activity-clear-when-task-reset] [--activity-exclude-from-recents]
    [--activity-launched-from-history] [--activity-multiple-task]
    [--activity-no-animation] [--activity-no-history]
    [--activity-no-user-action] [--activity-previous-is-top]
    [--activity-reorder-to-front] [--activity-reset-task-if-needed]
    [--activity-single-top] [--activity-clear-task]
    [--activity-task-on-home]
    [--receiver-registered-only] [--receiver-replace-pending]


例如，发送action=”broadcast.demo”的广播，并且对于forceStopPackage()的应用不允许接收该广播，命令如下：

    am broadcast -a broadcast.demo --exclude-stopped-packages

## ADB命令

查看启动包名和activity命令：
	adb shell dumpsys window w |findstr \/ |findstr name=
	
	logcat -v time -f /xxx.txt
-- "-s"选项 : 设置输出日志的标签, 只显示该标签的日志;

--"-f"选项 : 将日志输出到文件, 默认输出到标准输出流中, -f 参数执行不成功;

--"-r"选项 : 按照每千字节输出日志, 需要 -f 参数, 不过这个命令没有执行成功;

--"-n"选项 : 设置日志输出的最大数目, 需要 -r 参数, 这个执行 感觉 跟 adb logcat 效果一样;

--"-v"选项 : 设置日志的输出格式, 注意只能设置一项;

--"-c"选项 : 清空所有的日志缓存信息;

--"-d"选项 : 将缓存的日志输出到屏幕上, 并且不会阻塞;

--"-t"选项 : 输出最近的几行日志, 输出完退出, 不阻塞;

--"-g"选项 : 查看日志缓冲区信息;

--"-b"选项 : 加载一个日志缓冲区, 默认是 main, 下面详解;

--"-B"选项 : 以二进制形式输出日志;