---
title: 升级系统到Ubuntu 18.04 LTS
date: 2018-04-27 14:08:01
tags: [ubuntu]
---

升级前请备份重要数据，防止数据意外丢失

更新当前系统

```bash
sudo apt update && sudo apt dist-upgrade
```

完成后重启系统

```bash
sudo reboot
```

打开 Software & updates

![](/images/upgrade-to-ubuntu-18.04-LTS-1.png)

For Ubuntu 16.04 LTS desktop:

![](/images/upgrade-to-ubuntu-18.04-LTS-2.png)

For Ubuntu 17.10 desktop:

![](/images/upgrade-to-ubuntu-18.04-LTS-3.png)

开始更新

```bash
sudo update-manager -d
```

![](/images/upgrade-to-ubuntu-18.04-LTS-4.png)

![](/images/upgrade-to-ubuntu-18.04-LTS-5.png)

![](/images/upgrade-to-ubuntu-18.04-LTS-6.png)

![](/images/upgrade-to-ubuntu-18.04-LTS-7.png)

![](/images/upgrade-to-ubuntu-18.04-LTS-8.png)

![](/images/upgrade-to-ubuntu-18.04-LTS-9.jpg)

![](/images/upgrade-to-ubuntu-18.04-LTS-10.png)

![](/images/upgrade-to-ubuntu-18.04-LTS-11.jpg)


更多信息参考： https://www.ostechnix.com/how-to-upgrade-to-ubuntu-18-04-lts-desktop-and-server/