# 替换ec_main没生效

在替换libdriver_priver.a中的ec_main.o时，出现的几个方面

1. libdriver_private.a的地址是C:\Users\Administrator\Desktop\changeOS\onechange\luatos-soc-2022\PLAT\prebuild\PLAT\lib\gcc\lite 而不是gcc下面的libdriver_private.a
2. 在替换之后，在map中查看，始终使用的是原来的ec_main 而不是更改之后的。
    
    所以使用xmake clean指令清除之前的链接指令。然后再build就可以使更改后的ec_main生效了
    

烧录之后直接端口就没了：驱动不起作用了

生效之后使用JLink调试

ec_main.o中用到了那些

.bss.bcWaitStart  .bss.bcWaitEnd   .bss.bcldCfg   .bss.bcldErr   .bss.branchTrace   .bss.rawFlashPlatConfigPtr     .bss.wkupCfg   

.text.ecPrintFullImageReason.constprop.0    .text.BSP_Init_SmallImg    .text.BSP_DeInit_SmallImg 

.text.ec_main      .ARM.attributes       .comment

Cross Reference Table：：

ApmuFeedWtdg   ApmuPrintSleepLength    ApmuSniffProc    ApmuWaitBcLdComplete    ApmuWakeupProc  

 BSP_DeInit_SmallImg   BSP_GetPlatConfigItemValue BSP_GetRawFlashPlatConfig      BSP_Init_SmallImg    BSP_LoadPlatConfigFromRawFlash

EFuseGetChipID  EFuseGetDevVer  EFuseGetFtPrmVer  GPR_apAccessEnter   GPR_apDFCVote4CP  GPR_apDFCVoteIdle  GPR_clockDisable  GPR_clockEnable  GPR_initialize  GPR_rmiHrstRel  GPR_setClockDiv  GPR_setClockSrc  GPR_swReset  IC_Powoff   IC_PowupInit   PAD_driverInit  PsAonVarInit   PsL1BootVarAddrInit  RfAntTunerFemGpioInitConfig   ShareInfoInit  ShareInfoInitPlatConfig    SystemCoreClockUpdate   SystemFullImageInit   UsartPrintHandle  WDT_deInit  WDT_kick   WDT_start   WDT_stop

__PmCoreRegRestore  __wrap_SetUnilogUart  alarmFuncInit  apmuCPStartCheckInEcMain  apmuCPSwPowerOn  apmuCheckPSFullImageWakeup  apmuEnterPagingDeepSlp  apmuEnterPagingSlp1  apmuGetAPBootFlag  apmuGetAPLLBootFlag  apmuGetAPWakeupSrc  apmuGetBTMsCnt  apmuGetCPSleepState  apmuGetLatchExternalInt  apmuGetWakeupSrcBm  apmuHasHibTimertoWakeup     apmuIPCIntEnableSmallImg  apmuInitSmallImg  apmuIntInit  apmuIsCpSleepInSmallImg  apmuIsCpSleeped  apmuIsUnschdWakeup  apmuPeriDeleteWFITimer apmuPeriStartWFITimer  apmuPrintBootTimeStamp  apmuRecoverSystemRegisters  apmuRestoreBootFlag  apmuSetBootTimeStamp  apmuSetCPAonInvalid  apmuSetFullImgReason   ****apmuSetImageType   apmuSetLatchedPending  apmuSetSleepedFlag    apmuSetWakeupBT  apmuSlpMemInit  apmuTimeoffsetNeedUpdate  bcWaitEnd  bcWaitStart  bcldCfg  bcldErr cpADCInit  delay_us  ecRecordNodeInit

ec_main

excepCleanInExcephandler  gApmuConfigCtrl  gXPShareInfo  hibTimerGetNearestMs  hibTimerRecoverEnvPagingImg

main_entry

psDialPagingImgInfoInit  setOSState  swLogPrintf  trimAdcGetCalCode  trimAdcGetT0Code  trimAdcSetGolbalVar  trimEfuseNotAon  uniLogFlushOut  uniLogInitStart  uniLogStop  wkupCfg