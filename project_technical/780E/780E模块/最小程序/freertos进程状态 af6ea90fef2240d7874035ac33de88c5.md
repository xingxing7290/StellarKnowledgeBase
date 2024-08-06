# freertos进程状态

原来知道的进程的状态

运行，就绪，阻塞，创建，终止

freertos的任务状态

就绪（Ready），运行（Running），阻塞（Blocked），挂起（Suspended）

挂起态（Suspended）：任务调用vTaskSuspend函数的时候。这是让任务挂起的唯一方法。而把一个挂起状态的任务唤醒的唯一途径就是调用vTaskResume或vTaskResumeFromISR函数。

![Untitled](freertos%E8%BF%9B%E7%A8%8B%E7%8A%B6%E6%80%81%20af6ea90fef2240d7874035ac33de88c5/Untitled.png)

6、就绪-->挂起：在其他的正在运行的任务当中调用vTaskSuspend来将任务挂起即可。

7、还在阻塞状态的任务被正在运行的其他任务调用vTaskSuspend来挂起。

8、运行-->挂起：任务本身在运行时调用vTaskSuspend来将自身挂起。

9、挂起-->就绪：挂起的任务被正在运行的任务或者中断调用对应API函数恢复后，就会加入到就绪列表当中进入就绪态。

delayed_1状态并不存在，可能造成的原因是。freertos_gdb 和freertos版本不兼容所造成的

查看freertos版本号：FreeRTOS.h  skKERNEL_VERSION_NUMBER 宏定义了版本号

访问 freertos_gdb 插件的 GitHub 仓库（[https://github.com/coldnew/freertos.gdb），查看该插件支持的](https://github.com/coldnew/freertos.gdb%EF%BC%89%EF%BC%8C%E6%9F%A5%E7%9C%8B%E8%AF%A5%E6%8F%92%E4%BB%B6%E6%94%AF%E6%8C%81%E7%9A%84) FreeRTOS 版本。