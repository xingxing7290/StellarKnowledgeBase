# HardFault

官方地址

```html
https://wiki.segger.com/Cortex-M_Fault
```

`[1] PRECISERR   - If 1, return address instruction caused the fault.`  返回地址指令导致故障。

**用Ozone分析Cortex-M故障**

```html
https://wiki.segger.com/Analyzing_Cortex-M_Faults_with_Ozone
```

Nested Exceptions on Multiple Stacks多个堆栈上的嵌套异常

### File not found

E:/luatos-sdk-ec618/PLAT/middleware/developed/ecapi/psapi/src/ps_sync_cnf.c

Please use the source files window to locate the file manually

出现这种情况可能是在这之间存在断点。

如果部分文件找不到是因为部分SDK文件没有开放出来，需要使用step over 跳过。或两个之间加断点，跳到下一个断点。