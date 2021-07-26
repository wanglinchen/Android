adb root 
adb shell 
cd sys/class/power_supply/battery 
cat shipmode 如果是0 就表示非shipmode模式
echo 1 >> shipmode . 打开shipmode模式
等30s按关机键软件关机， 再测量一下Vph_pwr的电压。

量不到电压 就表示shipmode功能 OK
