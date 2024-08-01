# ec_main中都有啥

ec_main.o中用到了那些

.bss.bcWaitStart  .bss.bcWaitEnd   .bss.bcldCfg   .bss.bcldErr   .bss.branchTrace   .bss.rawFlashPlatConfigPtr     .bss.wkupCfg   

.text.ecPrintFullImageReason  .constprop.0    .text.BSP_Init_SmallImg    .text.BSP_DeInit_SmallImg 

.text.ec_main      .ARM.attributes       .comment

Cross Reference Table：：

（**apmu_external.h:**EC618平台电源管理头文件）

ApmuFeedWtdg  ：恢复启动标志到RAM var  在平台启动时调用，以记录全局启动标志     ApmuWakeupProc      **apmuInit**

ApmuPrintSleepLength  ApmuSniffProc    ApmuWaitBcLdComplete 

**(plat_config.h)**平台配置头文件  将plat配置分成两个部分，FS和raw flash

 BSP_DeInit_SmallImg   BSP_Init_SmallImg   (这两个应该是再ec_main中定义的)

 BSP_GetPlatConfigItemValue获取平台配置项的值   

BSP_GetRawFlashPlatConfig 获取原始flash平台配置变量指针  

BSP_LoadPlatConfigFromRawFlash从raw flash加载平台配置

EFuseGetChipID  EFuseGetDevVer  EFuseGetFtPrmVer

**(clock.h)**EC618时钟驱动头文件

GPR_apAccessEnter   GPR_apDFCVote4CP  GPR_apDFCVoteIdle  

(能找到的)GPR_clockDisable  GPR_clockEnable  GPR_initialize GPR_setClockDiv    GPR_swReset  

 GPR_rmiHrstRel  GPR_setClockDiv  GPR_setClockSrc  GPR_swReset  

**(ic.h)** EC618中断控制器头文件

IC_Powoff  取消中断控制器的初始化，包括硬件配置和ISR初始化。

 IC_PowupInit 初始化中断控制器，包括硬件配置和ISR初始化。当POR或从Hibernate唤醒时调用，其中SRAM内容已经丢失。

**(pad.h)**

 PAD_driverInit   初始化PAD驱动内部数据，必须在调用任何其他api之前调用

PsAonVarInit   PsL1BootVarAddrInit  RfAntTunerFemGpioInitConfig   ShareInfoInit  ShareInfoInitPlatConfig    

SystemCoreClockUpdate**（system_ec618.h）**   用从cpu寄存器检索到的当前核心时钟更新SystemCoreClock。

SystemFullImageInit   UsartPrintHandle**(bsp.h)**  

**(wdt.h)** Watchdog TimerEC618 看门狗定时器驱动头文件

WDT_deInit  将 WDT 设置回初始状态，停止计时和触发功能，以便后续的重新初始化或其他操作。

  WDT_kick（刷新看门狗计时器）   WDT_start  开始 WDT_stop 停止  

__PmCoreRegRestore  __wrap_SetUnilogUart  

(hal_alarm.h）

alarmFuncInit  

apmuCPStartCheckInEcMain  apmuCPSwPowerOn  apmuCheckPSFullImageWakeup  apmuEnterPagingDeepSlp  apmuEnterPagingSlp1  apmuGetAPBootFlag  apmuGetAPLLBootFlag  apmuGetAPWakeupSrc  apmuGetBTMsCnt  apmuGetCPSleepState  apmuGetLatchExternalInt  apmuGetWakeupSrcBm  apmuHasHibTimertoWakeup     apmuIPCIntEnableSmallImg  apmuInitSmallImg  apmuIntInit  apmuIsCpSleepInSmallImg  apmuIsCpSleeped  apmuIsUnschdWakeup  apmuPeriDeleteWFITimer apmuPeriStartWFITimer  apmuPrintBootTimeStamp  apmuRecoverSystemRegisters  apmuRestoreBootFlag  apmuSetBootTimeStamp  apmuSetCPAonInvalid  apmuSetFullImgReason   ****apmuSetImageType   apmuSetLatchedPending  apmuSetSleepedFlag    apmuSetWakeupBT  apmuSlpMemInit  apmuTimeoffsetNeedUpdate  

bcWaitEnd  bcWaitStart  bcldCfg  bcldErr cpADCInit  delay_us  ecRecordNodeInit

ec_main

excepCleanInExcephandler  gApmuConfigCtrl  gXPShareInfo  hibTimerGetNearestMs  hibTimerRecoverEnvPagingImg

main_entry  （libcore_airm2m.a(bsp_custom.o)）不用管应该是

**psDialPagingImgInfoInit**  初始化"pagingImgInfo"上下文

\注:a)在ec_main()调用之前，AP开始分页图像，
从AP分页图像启动到AP全图像时，不能重新调用\ b)

setOSState(bsp.h)设置操作系统状态： state  1：os启动  0：没有OS 或os没有启动

**(hal_trim.h)**

 trimAdcGetCalCode 用于ADC获取Cali代码

trimAdcGetT0Code   用于ADC获取 T0 代码

trimAdcSetGolbalVar :在efuse中读取adc cali值，并设置为一个全局var以供adc使用

这个全局变量将在分页和应用img中使用，并且应该在POR/SLEEP2/HIB情况下设置:不需要睡觉。

 trimEfuseNotAon 用于从efuse读取修剪值，然后写入到无Aon reg 

 当POR/SLEEP2/HIB时在bootloader中调用，当SLEEP1在ramboot中调用

(unilog.h) 

 swLogPrintf 打印日志

 uniLogFlushOut  uniLogInitStart  uniLogStop  wkupCfg

bsp.c 中的BSP_CommonInit 中板间通用初始化