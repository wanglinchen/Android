adb devices  查看adb是否连接上

adb root  获取root权限

adb shell 进入adb设备

cd /sys/kernel/debug/usb20_phy/  进入目录下

ls  看是否有RG_USB20_VRT_VREF_SEL，RG_USB20_TERM_VREF_SEL，RG_USB20_PHY_REV6这些节点。


（1）如果眼图测试压线FAIL（如图），先写如下节点调节Driving capability，重复步骤3、4复测：
echo 111> RG_USB20_VRT_VREF_SEL 
echo 111> RG_USB20_TERM_VREF_SEL
(VRT/TERM最大可以调到7，档位需调整一致，详见Page5 )

（2）如果VRT/TERM调到最大仍压线FAIL，则修改ENHANCEMENT，重复步骤3、4复测：
echo 10 > RG_USB20_PHY_REV6
（ENHANCEMENT最大取值为3，详见Page6）

（3）VRT/TERM/ENHANCEMENT调到最大仍压线，则联系MTK提供帮助；

（4）如若测试PASS，则请SW同仁根据最终参数修改code；修改方法参考FAQ21068

注意：
 重新拔插后、adb修改的参数会被代码默认值覆盖，需重新设定；
