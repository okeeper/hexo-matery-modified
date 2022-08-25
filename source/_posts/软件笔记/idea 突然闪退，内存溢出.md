解决办法：到安装路径C:\Program Files (x86)\JetBrains\IntelliJ IDEA 15.0.4\bin
找到idea.exe.vmoptions配置修改Xmx 为合适大小1024/2048，然后启动此路径下的idea.exe
```
-Xms128m
-Xmx1024m
-XX:MaxPermSize=350m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow

```

** 如果你的系统是64位的，则就需要修改idea64.exe.vmoptions这个配置，然后启动idea64.exe**