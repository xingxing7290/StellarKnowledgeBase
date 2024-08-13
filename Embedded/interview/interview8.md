# linux 

1、linux启动流程：
>(1)加载BIOS
>(2)读取MBR
>(3)运行BootLoader
>(4)加载内核
>(5)用户层Init依据inittab文件来设定运行等级
>(6)init进程执行rc.sysinit
>(7)启动内核模块
>(8)执行不同运行级别的脚本程序
>(9)执行/etc/rc.d/rc.local
>(10)执行/bin/login程序，进入登入状态

文章推荐：

<https://blog.csdn.net/jmppok/article/details/53334488>

<http://www.ruanyifeng.com/blog/2013/08/linux_boot_process.html>

2、new/delete 和 malloc/free 的底层实现(malloc与new的区别)？

>(1)、new关键字是C++的一部分，new是操作符，可以被重载。
>(2)、malloc是由c库函数提供的
>(3)、new关键字以具体类型为单位进行内存分配。
>(4)、malloc函数是以字节为单位进行内存分配。
>(5)、new关键在申请单个类型变量时可以进行初始化。
>(6)、malloc 不可以进行内存初始化。
>(7)、malloc返回空指针，new返回对象类型指针
>(8)、malloc分配失败返回null,new分配失败返回异常
>(9)、new-delete调用构造函数和析构函数，malloc没有。
>
>new/delete、malloc/free 底层实现原理：
>概述：new/delete 的底层实现是调用 malloc/free 函数实现的，而 malloc/free 的底
>层实现也不是直接操作内存而是调用系统 API 实现的。

3、请描述 IO 多路复用机制?

IO 模型有 4 中：同步阻塞 IO、同步非阻塞 IO、异步阻塞 IO、异步非阻塞 IO；IO 多路
复用属于 IO 模型中的异步阻塞 IO 模型，在服务器高性能 IO 构建中常常用到。

同步异步是表示服务端的，阻塞非阻塞是表示用户端，所以可解释为什么 IO 多路复用
（异步阻塞）常用于服务器端的原因；
文件描述符（FD，又叫文件句柄）：描述符就是一个数字，它指向内核中的一个结构
体(文件路径，数据区等属性)。具体来源：Linux 内核将所有外部设备都看作一个文
件来操作，对文件的操作都会调用内核提供的系统命令，返回一个 fd(文件描述符)。
下面开始介绍 IO 多路复用：
（1）I/O 多路复用技术通过把多个 I/O 的阻塞复用到同一个 select、poll 或 epoll
的阻塞上，从而使得系统在单线程的情况下可以同时处理多个客户端请求。与传统的
多线程/多进程模型比，I/O 多路复用的最大优势是系统开销小，系统不需要创建新的
额外进程或者线程。
（2）select，poll，epoll 本质上都是同步 I/O，因为他们都需要在读写事件就绪后
自己负责进行读写，也就是说这个读写过程是阻塞的，而异步 I/O 则无需自己负责进
行读写，异步 I/O 的实现会负责把数据从内核拷贝到用户空间。
（3）I/O 多路复用的主要应用场景如下：
服务器需要同时处理多个处于监听状态或者多个连接状态的套接字；
服务器需要同时处理多种网络协议的套接字；

（4）目前支持 I/O 多路复用的系统调用有 select，poll，epoll，epoll 与 select
的原理比较类似，但 epoll 作了很多重大改进，现总结如下：
①支持一个进程打开的文件句柄 FD 个数不受限制（为什么 select 的句柄数量受限
制：select 使用位域的方式来传递关心的文件描述符，因为位域就有最大长度，在
Linux 下是 1024，所以有数量限制）； ②I/O 效率不会随着 FD 数目的增加而线性下降；
③epoll 的 API 更加简单；
（5）三种接口调用介绍：
①select 函数调用格式：

```c
#include <sys/select.h>
#include <sys/time.h>
int select(int maxfdp1,fd_set *readset,fd_set *writeset,fd_set 
*exceptset,const struct timeval *timeout)
//返回值：就绪描述符的数目，超时返回 0，出错返回-1 ②poll 函数调用格式：
# include <poll.h>
int poll ( struct pollfd * fds, unsigned int nfds, int timeout);
```

③epoll 函数格式（操作过程包括三个函数）：

```c
#include <sys/epoll.h>
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

（6）作用：一定程度上替代多线程/多进程，减少资源占用，保证系统运行的高效
率

4、快速排序的思想、时间复杂度、实现以及优化方法?

快速排序的三个步骤
(1)选择基准：在待排序列中，按照某种方式挑出一个元素，作为 "基准"（pivot）；
(2)分割操作：以该基准在序列中的实际位置，把序列分成两个子序列。此时，在基准
左边的元素都比该基准小，在基准右边的元素都比基准大；
(3)递归地对两个序列进行快速排序，直到序列为空或者只有一个元素。
基准的选择：
对于分治算法，当每次划分时，算法若都能分成两个等长的子序列时，那么分治算法
效率会达到最大。
即：同一数组，时间复杂度最小的是每次选取的基准都可以将序列分为两个等长的；
时间复杂度最大的是每次选择的基准都是当前序列的最大或最小元素；
快排代码实现：
我们一般选择序列的第一个作为基数，那么快排代码如下：
```c
void quicksort(vector<int> &v,int left, int right)
{ 
if(left < right)//false 则递归结束
{ 
int key=v[left];//基数赋值
int low = left; 
int high = right; 
while(low < high) //当 low=high 时，表示一轮分割结束
{ 
    while(low < high && v[high] >= key)//v[low]为基数，从后向前与基数比 较
    { 
        high--; 
    }
    swap(v[low],v[high]);
    while(low < high && v[low] <= key)//v[high]为基数，从前向后与基数比 较
    { 
    low++; 
    } 
    swap(v[low],v[high]);
} 
//分割后，对每一分段重复上述操作
quicksort(v,left,low-1); 
quicksort(v,low+1,right);
} }
```

注：上述数组或序列 v 必须是引用类型的形参，因为后续快排结果需要直接反映在原
序列中；
优化：
上述快排的基数是序列的第一个元素，这样的对于有序序列，快排时间复杂度会达到
最差的 o(n^2)。所以，优化方向就是合理的选择基数。
常见的做法“三数取中”法（序列太短还要结合其他排序法，如插入排序、选择排序
等），如下：
①当序列区间长度小于 7 时，采用插入排序；
②当序列区间长度小于 40 时，将区间分成 2 段，得到左端点、右端点和中点，我们
对这三个点取中数作为基数；
③当序列区间大于等于 40 时，将区间分成 8 段，得到左三点、中三点和右三点，分
别再得到左三点中的中数、中三点中的中数和右三点中的中数，再将得到的三个中数
取中数，然后将该值作为基数。
具体代码只是在上一份的代码中将“基数赋值”改为①②③对应的代码即可：
int key=v[left];//基数赋值
if (right-left+1<=7) {
insertion_sort(v,left,right);//插入排序
return;
}else if(right-left+1<=8){
key=SelectPivotOfThree(v,left,right);//三个取中
}else{
//三组三个取中，再三个取中（使用 4 次 SelectPivotOfThree，此处不具体展示）
}
需要调用的函数：
void insertion_sort(vector<int> &unsorted,int left, int right) //插入排序算法
{ 
for (int i = left+1; i <= right; i++) 
{ 
if (unsorted[i - 1] > unsorted[i]) 
{ 
int temp = unsorted[i]; 
int j = i; 
while (j > left && unsorted[j - 1] > temp)
{ 
unsorted[j] = unsorted[j - 1]; 
j--; 
} 
unsorted[j] = temp; 
} 
} 
}
int SelectPivotOfThree(vector<int> &arr,int low,int high) 
//三数取中，同时将中值移到序列第一位
{ 
int mid = low + (high - low)/2;//计算数组中间的元素的下标
//使用三数取中法选择枢轴
if (arr[mid] > arr[high])//目标: arr[mid] <= arr[high] 
{ 
swap(arr[mid],arr[high]);
} 
if (arr[low] > arr[high])//目标: arr[low] <= arr[high] 
{ 
swap(arr[low],arr[high]);
} 
if (arr[mid] > arr[low]) //目标: arr[low] >= arr[mid] 
{ 
swap(arr[mid],arr[low]);
} 
//此时，arr[mid] <= arr[low] <= arr[high] 
return arr[low]; 
//low 的位置上保存这三个位置中间的值
//分割时可以直接使用 low 位置的元素作为枢轴，而不用改变分割函数了
} 
这里需要注意的有两点：
①插入排序算法实现代码；
②三数取中函数不仅仅要实现取中，还要将中值移到最低位，从而保证原分割函数依
然可用。

5、冒泡实现代码：

```c
void bubble_sort(int a[], int n)
{
      for(int i =0; i<n-1; i++)
      {
          for(int j =0; j<n-1-i;j++)
          {
              if(a[j]>a[j+1]
              {
                  int temp = a[j];
                  a[j]=a[j+1];
                  a[j+1]=temp;
            }
          }
      }
}
```

好文推荐：
<https://blog.csdn.net/guoweimelon/article/details/50902597>
<http://c.biancheng.net/cpp/html/2443.html>

1、线程中有哪些同步机制？
(1)互斥锁
(2)条件变量
(3)读写锁
(4)信号量
(5)自旋锁
(6)屏障

注具体用法：可以查找几篇博客来看

<https://www.jianshu.com/p/eca71b7fda2c>

2、uboot的启动流程：

>第一阶段:
>         硬件初始化
>       为加载的bootloader准备RAM空间
>       复制代码到第二阶段RAM空间
>       设置栈
>      跳转第二阶段C入口点
>第二阶段:
>      初始本阶段使用的硬件设备
>       检测系统内存映射
>      将内核，根文件系统从FLASH读取到RAM
>       为内核设置启动参数
>       调用内核

好文推荐：

<https://www.cnblogs.com/heaad/archive/2010/07/17/1779829.html>

<https://juejin.im/post/6844904160228311047>

3、tcp与udp的区别：

(1)、基于连接与无连接；

(2)、对系统资源的要求（TCP较多，UDP少）；

(3)、UDP程序结构较简单；

(4)、流模式与数据报模式 ；

(5)、TCP保证数据正确性，UDP可能丢包；

(6)、TCP保证数据顺序，UDP不保证。

好文推荐：

<https://zhuanlan.zhihu.com/p/24860273>

<https://zhuanlan.zhihu.com/p/108822858>

<https://zhuanlan.zhihu.com/p/76023663>
