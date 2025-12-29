# Kobato读卡器使用教程（Sega)

# 读卡器基本介绍

本读卡器软硬件全部开源，开源地址[https://github.com/QHPaeek/Arduino-Aime-Reade](https://github.com/QHPaeek/Arduino-Aime-Reader)[r](https://github.com/QHPaeek/Arduino-Aime-QReader)

### 硬件介绍：

主控：本读卡器基于LQFP48-STM32主控进行复用设计，根据主控可分为F1版（旧）与F0版（新）

灯光：某些旧版使用共阳RGB灯珠，现行新版采用WS2812作为灯光。

因此，总共分为三个硬件小版本：F1版，F0版（共阳RGB），F0版（WS2812）

NFC读卡模块为PN532（NFC MODULE V3模块），连接方式为UART。

主控与PC之间通讯使用USB CDC端口，**无视实际波特率。**

### 支持卡片：

1. Amusement IC卡
2. 经典Aime卡
3. Banapass卡

有关不同卡片的介绍，请看卡片介绍（暂未编写）

### 功能介绍：

- 可以通过上位机Baudratetool修改内部设置
- 可以自定义灯光亮度（256级亮度）
- 卡号映射功能，支持将一张AIC卡映射为自定义卡号

### 备注：

本读卡器在Sega下通过模拟官方837-15693读卡器或TN32读卡器，使用原生端口连接游戏，因此无需aimeio。高波特率模式下模拟的是837-15693读卡器，低波特率模式下模拟TN32读卡器。

<aside>
💡 高波特率模式与低波特率模式影响的不只是通讯波特率，还会影响读卡器传给游戏的硬件版本信息。由于Kobato读卡器使用CDC，CDC具有不受通讯波特率影响也能正常通讯的特性，所以模式影响的只是读卡器传给游戏的版本信息。

</aside>

读卡器刚刚启动时的LED颜色表示了目前状态。蓝色表示处于高波特率模式，绿色表示处于低波特率模式，红色表示找不到PN532模块，启动失败。启动失败的状态下将无法连接到PC。

当接收到游戏发来的灯光指令时，读卡器的灯光将会交由游戏控制。因此进入游戏后，灯光将不再能表征读卡器的状态。

<aside>
💡

目前c3系列读卡器存在bug，在使用baudratetool.exe会出现读卡器连接错误，重新启动baudratetool即可。

![image.png](Kobato%E8%AF%BB%E5%8D%A1%E5%99%A8%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B%EF%BC%88spice)/image.png)

</aside>

# 如何连接游戏

## 支持的游戏

| 游戏 | 读卡器端口号 | 默认波特率 | 备注 |
| --- | --- | --- | --- |
| ONGEKI, maimaiDX | COM1 | 高 |  |
| maimai FiNALE（旧框）
 | COM2 | 低 | 不支持AIC卡 |
| CHUNITHM | COM4 | 低（cvt），高（sp） |  |
| Project DIVA, Initial D | COM10 | 低 |  |

<aside>
💡 高波特率=115200，低波特率=38400

</aside>

<aside>
💡 Chunithm的CVT即60HZ模式，对应旧框体。SP模式对应120HZ，新框体。

</aside>

## 操作步骤

- 首先将读卡器的端口改为对应游戏的读卡器端口号。右键windows图标，打开设备管理器，点击端口

<aside>
💡 kobato读卡器无须安装驱动

</aside>

![image.png](Kobato%E8%AF%BB%E5%8D%A1%E5%99%A8%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B%EF%BC%88Sega)/image.png)

![Untitled](Kobato%20Card%20Reader%20Tutorial%20(Sega)/Untitled.png)

![Untitled](Kobato%20Card%20Reader%20Tutorial%20(Sega)/Untitled%201.png)

点击属性-高级-COM端口号，修改。修改完成后，请重启电脑。

<aside>
💡 如果提示“这个COM名称正在被另一个设备使用”，请观察一下现有的COM口列表中是否有COM序号一样的设备。如果有，大概率是主板板载的打印机通讯端口等。为了确保COM口序号唯一，保证正常通讯，请按上述操作将这个占用了序号的设备挪到一个未在使用的空端口上，再修改读卡器端口号。如果没有，直接修改即可。

</aside>

<aside>
💡 F1版的端口在设备管理器中应该显示为Maple Serial，F0版的端口应该显示为USB串行设备（未安装驱动时）或STM Serial（安装驱动后）

</aside>

<aside>
💡 重启电脑是因为windows的BUG，修改了端口号后如果不重启可能导致游戏打不开端口

</aside>

<aside>
💡 修改一次端口号后电脑就会记忆存储这个端口号，不会发生变化。因此不需要反复修改设置。

</aside>

- 打开baudratetool，跟随工具提示，输入端口号后按1进入Sega模式下设置修改，更换为你运行游戏对应的波特率模式。

<aside>
💡 如果进错了模式，灯光会发生变化。黄色：读卡测试模式，水蓝色：PN532直通模式，闪一下粉色后熄灭：SPICE模式，闪一下红灯后熄灭：Namco模式。请确保你的读卡器处于Sega模式下。否则无法连接游戏

</aside>

<aside>
💡 如果你游玩的版本使用了sinmod或其他魔改工具，可能会影响读卡器的连接。具体表现为：连接读卡器后即使网络全部GOOD也会断网（MaiMai），卡在Aime检测（音击）。这种情况下请将读卡器切换为低波特率模式并重试。不推荐使用任何魔改。

</aside>

- 请打开`segatool.ini`，找到如下内容

```jsx
[aime]
; If you wanna use native Aime reader, set this to 0
enable=0
```

在`[aime]`一栏中找到`enable`，如果是=1，请修改为=0。如果没有这个选项，可以把以上内容复制粘贴到`segatool.ini`末尾。

<aside>
💡 提醒一下，英文分号“;”在这里是注释的意思。注释是代码文件中常用的手段，注释符号后面的内容不会被程序读取。因此如果你想让某一行不发挥作用，在前面打一个分号即可。

</aside>

如果你使用了aimeio，请将其注释掉，如：

```jsx
[aimeio]
; If you wish to sideload a different aimeio, specify the DLL path here
;path=aimeio.dll
; If you are YubiDeck use this
;path=aimeio_yubideck.dll
```

如上，保证`[aimeio]`下面没有任何有效的path。

如果你游玩的是Chunithm：

```jsx
[gpio]
; NetInstall type: 0 = client, 1 = server
dipsw1=1
; Monitor type: 0 = 120fps, 1 = 60fps
dipsw2=0
; Aime reader hardware type: 0 = sp, 1 = cvt
dipsw3=0
```

请注意这里的dipsw3:Aime Reader Hardware Type。如果为0的话，读卡器应该为高波特率模式。如果为1的话，应该为低波特率模式。

<aside>
💡 另外，读卡器的配置与config_cvt.json/config_sp.json（chunithm）、config_common.json（maimai）等文件内配置有关，如果你不了解请不要轻易修改这些文件内容，保持默认状态即可。

</aside>

- 请把amdaemon.exe sinmai.exe(或chuniapp.exe) inject.exe(或inject64.exe.与inject86.exe两个文件)全部右键设置中设置为管理员启动，再以管理员身份运行start.bat。

到这一步，运行游戏后应该就能正确检测到读卡器并读卡了。如果仍然出现问题，请查阅常见问题手册。

## 其他注意事项：

- 由于读卡器天线功率有限，所以请勿将读卡器靠近金属物体，否则天线信号会被吸引走，导致刷不到卡。例如贴在ADX的外壳上，一些中二手台的金属外壳，电脑机箱等。如果一定要贴，请在读卡器与物体表面之间隔一层隔磁片。如果没有隔磁片，请在某宝自行购买。
- 如果你使用的是AIC卡（例如蓝白AIME卡），并且连接了一个没有接入AIMEDB的服务器，那么你刷卡所登陆的卡号应该会与卡背后印刷的卡号不一致。这种情况不影响正常游玩，但如果你已经用虚拟读卡器写这个卡号登陆有了一个存档，并且想继续玩，那么可以使用本读卡器的卡号映射功能。请先使用baudratetool将读卡器进入读卡测试功能，刷这张AIC卡，记录窗口上显示的IDm序号，然后拔掉读卡器重新连接，重新打开baudratetool，在设置中开启卡号映射功能并输入IDm和目标卡号。