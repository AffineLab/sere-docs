# Monica-Lite读卡器使用教程

<div style="text-align:center;margin:8px;">
    <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/b4f8d2420e21f4622744bb75d4ffd4b9.png">
</div>

## 读卡器基本介绍与使用说明

??? info "硬件概述"

    **主控芯片：** 本读卡器采用 **STM32F072** 作为主控芯片。

    与市面上常见的 **PN532** 不同，本产品使用了意法半导体公司生产的 **ST25R3911** NFC 芯片，拥有更强的读卡距离与抗干扰性能。

    **版本说明：**

    Monica 家族目前包括以下三个版本：

    - **Monica Lite**：基础版本，不含灯光。
    - **Monica 标准版：**带LED，标准版外壳。(研发中)
    - **Monica Pro：**带LED，铝合金外壳。(研发中)

    **通信接口：**

    主控与 PC 之间通过 **USB CDC** 端口通信，**不受实际波特率影响**。

    **外壳材质：**

    采用哑光 PETG 材料 3D 打印

### 基础连接步骤

!!! danger "必看"
    无论游玩哪款游戏都先需要按照这个步骤进行

!!! info ""
    1. 使用 **Type-C 数据线** 连接读卡器与电脑。
    2. 打开 **设备管理器**，在 **“端口 (COM 和 LPT)”** 分类下能看到 **“USB 串行设备”**。
    3. 若未显示此设备，请尝试：
        - 更换 Type-C 数据线；
        - 或更换 USB 接口。

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image.png">
    </div>

### SEGA 系游戏设置指南

!!! danger "注意"
    本产品仅保证在纯净包中正常运行，如果你使用了懒人包，请往下翻阅常见问题

#### 设置端口号

!!! warning
    如果端口中有其它设备占用了读卡器的端口号，请将该设备的端口号设置成其他数字。
    
!!! note "注意"
    Chunithm的CVT即60HZ模式，对应旧框体。SP模式对应120HZ，新框体。

!!! info ""
    属性 → 端口设置 → 高级，即可修改端口号。
        
    请根据下表选择对应端口号：

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image%201.png">
    </div>

    | 游戏 | 读卡器端口号 | 默认波特率 | 备注 |
    | --- | --- | --- | --- |
    | ONGEKI, maimaiDX | COM1 | 高 |  |
    | maimai FiNALE（旧框）| COM2 | 低 | 不支持AIC卡 |
    | CHUNITHM | COM4 | 低（cvt），高（sp） |  |
    | Project DIVA, Initial D | COM10 | 低 |  |

#### 修改 segatools.ini
!!! info ""
    在游戏文件夹中找到 **`segatools.ini`** 文件并编辑：

    1.在 `[aimeio]` 部分：

    - 本读卡器采用串口直连方式，不需 AIMEIO 模块。
    - 可将该段内容删除，或在 `path` 前加分号注释。

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image%202.png">
    </div>

    2.在 `[aime]` 部分：

    - 将 `enable=1` 修改为 `enable=0`。

    3.**保存修改**（Ctrl + S），退出文件。

    4.启动游戏后即可正常使用读卡器。

!!! warning
    如果无法刷卡请重启电脑再试

### KONAMI 系游戏设置指南

!!! note "注意"
    理论上支持通过spice启动的所有konami音游，包含SDVX、IIDX、Jubeat、Gitadora、Pop’n MUSIC等音游。

#### 设备管理器设置
!!! info ""
    参照前文 SEGA 游戏部分的设置步骤。
        
    唯一区别是：**只需确保端口号不与其他设备冲突**，无需特定编号。

#### Spice 设置
!!! info ""
    打开游戏目录下的 **`spicecfg.exe`**，在 “Advanced” 选项卡中：
    
    - 启用 **“CardIO HID Reader Support”**。
    - 保存设置后即可使用。

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image%203.png">
    </div>

### Namco 系游戏设置指南
!!! note "注意"
    - MonicaLite读卡器在更新"Monica_Lite_F072_2512270821.bin"固件后支持namco游戏刷卡。
    - 使用原始Namco bngrw.dll与游戏通讯。对于太鼓达人，可以使用这个：[GitHub - gyt4/tal_no_bngrw_hook](https://github.com/gyt4/tal_no_bngrw_hook)
#### namco使用教程
!!! info ""
    - 修改读卡器端口号为COM1
    - 打开游戏根目录，找到config.toml文件，修改其中的[card_reader]值为false并保存

```
[card_reader]
enabled= false
[qr]
enabled = true
image_path=""
[qr.data]
serial=""
type=0
song_no=[]
```

### 固件更新指南

!!! note "需要准备"
    - 需要更新的固件(.bin文件，例如"Monica_Lite_F072_2512270821.bin")
    - Monica跳转DFU.exe(群文件中下载)
    - stm32cubeprog  [下载链接](https://www.st.com/en/development-tools/stm32cubeprog.html)

!!! warning "注意"
    根据ST论坛上官方反馈的BUG，F0这款芯片的USB下载存在一定问题，具体表现为尝试下载时提示“芯片读保护”（大意），遇到这种情况，官方提供的替代性解决方案是将USB接到一个USB扩展坞上即可下载（有的电脑主板USB口本身就是通过内置扩展坞扩展出来的，所以直接就可以下载。同理，一些笔记本电脑也可以直接下载）。
    [参考链接](https://community.st.com/t5/stm32cubeprogrammer-mcus/dfu-mode-read-out-protection/td-p/682719)

#### 进入DFU模式
!!! info ""
    打开"Monica跳转DFU.exe"，输入你的读卡器端口号即可进入dfu模式。

#### 烧录固件
!!! info ""
    根据下图进行操作，然后点击"download"即可。
    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/83f1c086-bc12-4822-ae94-e6dabb23230c.png">
    </div>

### 常见问题

#### maimai无法识别读卡器
!!! info ""
    用懒人包导致的问题，将最底下的压缩包解压即可。

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image%204.png">
    </div>

#### maimai卡在aime check
!!! info ""
    munet的aimedb问题，在segatools.ini中注释掉aimedb一项即可，修改完之后必须重启电脑

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/c4b2fdd9818173cf47f07849692e4291.png">
    </div>

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image%205.png">
    </div>

#### chunithm无法使用读卡器
!!! info ""
    - 从luminous+版本开始，读卡器可能出现游戏自检aimebad的情况，需要在游戏内重新插拔读卡器才能正常使用，目前已经发现使用高波特率模式才能正常运行，具体操作如下：
    - 首先需要确保使用了正确的chusanApp.exe和amdaemon.exe
    - 修改游戏文件夹的三个json文件，分别是config_cvt.json，config_sp.json，config_hook.json
    - 将这三个文件中的”high_baudrate”后面的“false”都改为”true”并且保存

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image%206.png">
    </div>

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/Monicalite/image7.png">
    </div>

#### spice无法识别读卡器
!!! info ""
    - 使用了过旧版本的spice，请更新新版本。