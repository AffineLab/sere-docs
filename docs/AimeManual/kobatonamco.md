# Kobato太鼓达人使用教程

!!! note "简介"
    使用原始Namco bngrw.dll与游戏通讯。对于太鼓达人，可以使用这个：[GitHub - gyt4/tal_no_bngrw_hook](https://github.com/gyt4/tal_no_bngrw_hook)

## **读卡器使用教程：**
!!! info ""
    1.首先使用Baudratetool，将读卡器调至NAMCO模式：

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/n1.png">
    </div>

    2.打开设备管理器，将读卡器端口切换到’COM1’

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/n2.png">
    </div>

    3.打开游戏根目录，找到config.toml文件，修改其中的[card_reader]值为false并保存。

    <div style="text-align:center;margin:8px;">
        <img alt="image.png" src="../../assets/images/aimemanual/kobato/n3.png">
    </div>

    4.启动游戏即可使用刷卡功能