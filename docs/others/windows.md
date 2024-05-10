## 通过 u 盘创建一个引导盘

> 不用工具制作 U 盘启动盘

```js
// 1.以管理员身份打开“命令提示符”；

win+r

// 2.在弹出的DOS窗口输入“DISKPART”，回车；

win+r+DISKPART

// 3.输入“LIST DISK”，回车；

LIST DISK

// 4.输入“SELECT DISK 1”，回车，选中要制作启动盘的U盘，这里不一定是“1”，可以通过磁盘空间的大小来区别U盘，千万别选错；
// 一般从0开始：0，1，2，3...，选择完后可以再次输入LIST DISK查看，带*的即为选中盘

SELECT DISK 1

// 5.输入“CLEAN”命令，回车，清空U盘上的所有数据；

CLEAN

// 6.输入“CREATE PARTITION PRIMARY”，回车，在U盘上创建一个分区；

CREATE PARTITION PRIMARY

// 7.输入“SELECT PARTITION 1”，回车，选中创建的分区；

SELECT PARTITION 1

// 8.输入“ACTIVE”命令，回车，将选中的分区标记为活动分区；

ACTIVE

// 9.输入“FORMAT FS = NTFS QUICK”命令，回车，将U盘格式化为NTFS格式；

FORMAT FS = NTFS QUICK

// 10.格式化完成，输入“EXIT”，回车，退出DISKPART。

EXIT

// 11.将windows iso镜像文件解压到U盘引导盘制作完成

```

#### ps:

1. U 盘原有数据请备份；

2. 选择磁盘时不要选错，否则后果很严重。

## 恢复 u 盘引导盘为普通 u 盘

> 右键直接格式化就行，选择还原设备默认值，把 NTFS 的格式转换成 FAT 即可

## 激活 Windows10

[180 天激活工具](https://www.cnblogs.com/bigben0123/p/10147651.html)

> 使用方法：直接运行 KMS-VL-ALL.cmd 即可激活，激活后不要删除或者移动本程序，180 天激活到期后会自动续期。安装包在硬盘 windows10-iso 里面

[百度网盘下载地址](https://pan.baidu.com/s/1pLUQdrP)

Win10 系统本身集成杀毒软件 Windows Defender 更新后会将此激活工具报为病毒，遭报毒后会删除激活程序文件导致激活失败。
解决方法可以安装其他杀毒软件，比如火绒 https://www.huorong.cn/ 或者添加到微软杀毒的排除里：
双击任务栏右下角盾牌图标(需先点“∧”箭头，显示隐藏的图标)，进入 Windows Defender 后，点击“设置”，“实时保护”点击“开”按钮变成“关”，
再往下拉找到“排除”，点击“添加排除项”，“排除文件夹”选择激活程序所在的文件夹，比如遐想系统激活程序放在了 C:\Windows\jihuo ；“排除文件”、
选择 C:\Windows\system32\SppExtComObjPatcher.exe （此文件正在激活时才会出现，激活成功后会自动删除，如果不添加此文件会影响激活 180 天后的激活续期，
请在激活程序子文件夹 32-bit 或 64-bit 里找到 SppExtComObjPatcher.exe 文件，复制到 C:\Windows\system32 文件夹里就可以添加此文件的排除了）。
最后把“实时保护”的“关”按钮点击变“开”即可。

```js
通过slmgr.vbs - xpr命令查看过期时间;
```
