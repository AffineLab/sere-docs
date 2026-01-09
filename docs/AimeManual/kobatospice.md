# Kobato读卡器使用教程（spice)


!!! note "功能介绍"

    - 可以通过上位机Baudratetool修改内部设置，实现IIDX的2P刷卡功能。
    - 可以自定义灯光亮度（256级亮度）

!!! note "说明"
    - 本读卡器在spice模式下通过spice API实现刷卡功能。

    - 读卡器刚刚启动时的LED颜色表示了目前状态。常亮蓝色和绿色表示处于sega模式，这时请使用本站附件中的baudratetool将模式修改为spice模式。spice模式应该是闪一下粉红色的灯光后熄灭。红色表示找不到PN532模块，启动失败。启动失败的状态下将无法连接到PC。

    - 读到卡片时，读卡器会闪烁灯光。读取到AIC卡片时，将闪烁一下蓝光。读取到旧版Aime卡时，将闪烁绿色。读到Banapass时，将闪烁绿色（旧版固件）或者黄色（新版固件）。

    - 由于konami的卡号与sega存在不同，目前无法实现读取原生卡号，因此实际传输的卡号是根据卡片的UID生成的。生成逻辑：

    - AIC卡：直接将IDM作为卡号。

    - aime和banapass：以E00401AF开头，后面填充卡片UID。

## 读卡器SPICE使用教程
!!! info ""
    - 首先使用Baudratetool，将读卡器调至SPICE模式,此时读卡器应该闪一下粉色led，然后熄灭
    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/s1.png">
    </div>

    > 读卡器的模式是会存储的，下次使用不需要重新切换模式。
    > 

    - 打开设备管理器，查看读卡器串口号：

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/s2.png">
    </div>

    - 如图所示，USB串行设备即为读卡器端口。如果你不确定具体哪个是读卡器，可以反复插拔，观察哪个是新出现的设备。这个图里面读卡器的端口是COM99。

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/s3.png">
    </div>

    - 打开spicecfg.exe，在API选项卡中，找到API(Serial)，像上图一样填写。API Serial Port填设备管理器里面的端口号（例如COM99），API Serial Baud填写115200。

    - 此时直接打开游戏即可刷卡。

## IIDX 2P刷卡教程

!!! info ""
    - 由于IIDX需要2个读卡器，而API Serial只能有一个，因此我们可以将Serial API通过小工具转发到TCP API端口，实现共用两个读卡器。

    - 打开设备管理器，找到读卡器，右键点击属性，点击端口设置：

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/s4.png">
    </div>

    - 点击高级，修改端口号为COM99：

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/s5.png">
    </div>

    - 点击确定，然后退出。

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/s6.png">
    </div>

    - 在spicecfg中找到API TCP Port，填写1337。

    - 下载com2tcptool.exe,并启动，随后打开游戏即可。