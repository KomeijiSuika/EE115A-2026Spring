# 混淆 PMOS/NMOS 与 S/G/D 端子

错误总结: MOS 电路先分清 PMOS / NMOS，再标 source、gate、drain；否则 VGS/VSG、方向和饱和条件都会跟着错。

**❌ 错在哪**：分析 MOS 电路时容易没先确认 PMOS / NMOS 类型，以及 source / gate / drain 的实际位置，导致 $V_{GS}$、$V_{SG}$、电流方向或饱和条件写错。

**✅ 正确做法**：先标器件类型：NMOS 看 $V_{GS}$、电流从 drain 到 source；PMOS 看 $V_{SG}$、电流从 source 到 drain。再在图上圈出 S / G / D，最后列工作区条件。

**🐈 防错提醒**：下笔前固定三步：①判 P/N MOS；②标 S/G/D；③再写 $V_{OV}$、饱和条件和小信号模型。