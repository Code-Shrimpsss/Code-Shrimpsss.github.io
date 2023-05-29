---
title: win端操控ubuntu虚拟机
date: 2021-11-15 00:00:00
type: Mysql
description: '在win端的终端操控ubuntu虚拟机终端'
---

#### 第一步：确保VM虚拟机已连接网络 ####

**在VM虚拟机终端输入**

1. 进入环境

   ```
   cat /var/lib/NetworkManager/NetworkManager.state
   ```

2. 进行验证 （过程有三次输入密码）

   **全程使用VM账号密码 第二次为隐藏输入**

   ```
   service NetworkManager stop
   
   sudo rm /var/lib/NetworkManager/NetworkManager.state
   
   service NetworkManager start
   ```

   `三次输入完成后 右上角弹出网络logo或者用浏览器测试是否接通网络`

- 第一步查看两端IP地址前三位一不一致

  本机服务端查看 `ipconfig`
  虚拟机服务端查看 `ifconfig`

  > 如： VM ： 192.168.44.111   与 win： 192.168.44.222 两者是可以互通的
  >
  > 可以使用 `ping ` 测试

#### 第二步：控制虚拟机终端 ####

##### **在win端输入** #####

```markdown
ssh 你的VM名字@你的VM端IP地址 
如:ssh Shrimps@192.168.44.352
```

1. 输入后会提示 YES/NO 输入**YES**
2. 再次输入你的密码

*以上操作执行完成之后如果出现：*

```markdown
----------
Welcome to Ubuntu 18.10 (GNU/Linux 4.18.0-25-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


167 packages can be updated.
0 updates are security updates.
----------
```

**就成功啦~~**