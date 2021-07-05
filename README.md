平台：MT6833，MT6853，MT6893，MT6891
pmic: mt6360
charger ic: bq25601(TI)，bq25910(TI)，SGM41511(圣邦威)，SC8547(南芯)

充电硬件原理：
USB上的Vbus出来先经过一个充电保护模块，其作用：
1.检测Vbus的电压，根据Vbus电压控制，控制Vbus到Vbus_in_pmic的通断，Vbus_in_pmic连接到mt6360,作为普通充电的输入电压，连接到快充IC作为快充芯片的输入电压。
2.通过一个GPIO口做使能控制，软件控制Vbus到手机端的通断，做充电过高温保护。

USB上的D+,D-出来经过一个USB_SWITCH的模块，通过vbus_in_pmic和fastchg_en(gpio)做切换，当Vbus_in_pmic有电压时，控制将D+,D-切到与pmic连接。当fastchg_en使能时，将D+,D-切到快充IC。

USB上的CC1，CC2连接到mt6360，用作CC检测，判断typeC是正插还是反插，以及判断DFP的供电能力(BCM码通信)。（DFP，UFP，DRP(三种端口模式)）

快充使能GPIO，软件控制当快充通讯完成时，使能快充使能脚，打开快充通路。

配置检流电阻，配置电池NTC，USB小板NTC，配置shipmode使能，配置pmu的中断，配置typeC的中断，配置mt6360充电使能

充电协议：QC2.0，PD3.0，vooc，svooc
做过的充电功率：9V2A，11V3A，5V6A，10V6.5A

QC2.0充电流程：
typec插入检测（vbus和cc脚）
BC1.2协议
开启普充(1.5A充电)
把USB的Dp,Dm从pmic切到快充IC
启动充电线程(充电连接检测，获取电池信息，停充检测 。。。)
QC2.0协议
开启9V2A充电

PD3.0充电流程：

vooc/Svooc充电流程：
typec插入检测（vbus和cc脚）
BC1.2协议
开启普充(1.5A充电)
把USB的Dp,Dm从pmic切到快充IC
启动充电线程(充电连接检测，获取电池信息，停充检测 。。。)
快充协议(支持mcu的固件版，支持快充通讯的charger pump)
开启5V6A充电

11V3A充电：
vbus连接到mt6360和sc8547,根据充电阶段要求，控制充电路劲走pmic还是charger pump。
sc8547通过IIC挂载在mt6853芯片上，AP通过IIC接口读取charger pump的寄存器。
sc8547可以实时监控vbus,vbat,vsys,ibus等电信号，将这些值存在相应的寄存器上。
快充协议：
适配器端(快充询问 -> 识别适配器ID -> )
手机端(快充询问应答 -> )





typeC线缆：
gnd,tx1+,tx1-,vbus,cc1,d+,d-,sbu1,vbus,rx2-,rx2+,gnd
gnd,rx1+,rx1-,vbus,sbu2,d-,d+,cc2,vbus,tx2-,tx2+,gnd

充电相关的线缆：
USB上的Vbus出来先经过一个充电保护模块，其作用：
1.检测Vbus的电压，根据Vbus电压控制，控制Vbus到Vbus_in_pmic的通断，Vbus_in_pmic连接到mt6360,作为普通充电的输入电压，连接到快充IC作为快充芯片的输入电压。
2.通过一个GPIO口做使能控制，软件控制Vbus到手机端的通断，做充电过高温保护。

USB上的D+,D-出来经过一个USB_SWITCH的模块，通过vbus_in_pmic和可控的gpio口做切换，当Vbus_in_pmic有电压时，控制将D+,D-切到与pmic连接。当gpio口使能时，将D+,D-切到快充IC和AP芯片。

USB上的CC1，CC2连接到mt6360，用作CC检测，判断typeC是正插还是反插，以及判断DFP的供电能力(BCM码通信)。（DFP，UFP，DRP(三种端口模式)）

USB上的TX1+，TX1-，RX2-，RX2+。USB3.1的数据线。
USB上的SBU1，SBU2.辅助信号，连接耳机时用作音频信号线。



充电协议：QC2.0，PD3.0，vooc，svooc
做过的充电功率：9V2A，11V3A，5V6A，10V6.5A

 
模拟耳机和数字耳机的区分：





