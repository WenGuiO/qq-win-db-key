# PCQQ (非 NT 架构)

## 预先准备

备份数据库！备份数据库！备份数据库！默认数据库路径为：`C:\Users\<用户名>\Documents\Tencent Files\<QQ号>\Msg3.0.db`

测试可用的 QQ 版本：`QQ9.7.3.28.94`、`QQ9.7.6 (28997)`、`QQ9.7.9 (29059)`

如果出现异常，可以尝试消灭`QQProtect`后重试：<https://www.zhihu.com/question/265963430/answer/2492603110>

## 跑（自动，建议）

需要 Python 以及 Frida：`pip install frida`

备份`Msg3.0.db` -> 打开 QQ -> `python pcqq_dump.py` -> 登录 -> 得到 key，同时解密并修复后的数据库文件将自动生成在运行目录下

## 跑（手动）

### hook

需要 Python 以及 Frida：`pip install frida`

备份`Msg3.0.db` -> 打开 QQ -> `python pcqq_get_key.py` -> 登录 -> 得到 key

### pcqq_rekey_to_none.cpp

将`BYTE pwdKey[16]`的下一行（也就是第 313 行）替换为你得到的 key

使用 32 位 MinGW-W64（[我用的版本](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/8.1.0/threads-win32/dwarf/i686-8.1.0-release-win32-dwarf-rt_v6-rev0.7z)）编译：`g++ pcqq_rekey_to_none.cpp` （记得把`mingw32\bin`加到`PATH`环境变量）

把`a.exe`与`Msg3.0.db`一起放在 QQ 安装目录的`Bin`文件夹（比如`C:\Program Files (x86)\Tencent\QQ\Bin\`下，运行`a.exe`，运行完成后`Msg3.0.db`即为解密状态。

### 修复

得到的`Msg3.0.db`开头有 1024 字节的扩展头，删掉。

## 毁灭（必定损坏原始数据）

备份`Msg3.0.db` -> 打开 QQ -> `python pcqq_DANGER_rekey.py` -> 登录 -> 原始数据库被破坏 -> 解密并修复后的数据库文件将自动生成在运行目录下

## 读取信息

<https://github.com/Akegarasu/qmsg-unpacker>

## 致谢（询问一切有关编解码、数据格式的问题前必看！！）

<https://bbs.kanxue.com/thread-250509.htm>

<https://www.52pojie.cn/thread-1370802-1-1.html>

<https://bbs.kanxue.com/thread-266370.htm> ( <https://www.52pojie.cn/thread-1386731-1-1.html> )

<https://github.com/Mrs4s/qq-db-key-injector>

<https://github.com/Akegarasu/qmsg-unpacker>

## 另一种方式

x64dbg hook sqlite3_key
