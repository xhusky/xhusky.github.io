---
title: ubuntu 18.04 LTS 更新数据源
date: 2018-05-02 13:19:47
tags: ubuntu
---

目前测试了两个数据源，`中科大` 和 `阿里云`

**中科大**

```
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse

```

**阿里云**
```
deb https://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb https://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

```

修改/etc/apt/sources.list即可，建议先备份

```bash
# 备份
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

# 修改数据源
sudo vi /etc/apt/sources.list
```

若为新安装系统，直接覆盖即可，否则建议追加到文件最后，注释掉ubuntu的原始源

我采用的是中科大的数据源

```
cat /etc/apt/sources.list    
```

```
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse

```
[更多18.04源，戳这里](http://wiki.ubuntu.org.cn/%E6%A8%A1%E6%9D%BF:18.04source)
