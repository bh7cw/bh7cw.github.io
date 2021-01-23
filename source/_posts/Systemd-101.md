---
title: Systemd 101
date: 2020-08-10 18:33:49
author: bh7cw
#top: true
cover: false
coverImg: /images/6.jpg
toc: true
summary: This post introduces the Systemd.
categories: Linux
tags:
  - Linux
---
## intro
- start daemon
- become the first process(PID: 1) instead of `initd`
- check the version of systems
`$ systemctl --version`
## Overview

![](https://i.imgur.com/PW7AIfP.png)

## Commands
Systemd includes a series of commands.
### systemctl
Main command, manage system.
```
$ sudo systemctl reboot

$ sudo systemctl poweroff

$ sudo systemctl halt

$ sudo systemctl suspend

$ sudo systemctl hibernate

$ sudo systemctl hybrid-sleep

$ sudo systemctl rescue
```
### systemd-analyze
Analyze the startup.
```
$ systemd-analyze                                     

$ systemd-analyze blame

$ systemd-analyze critical-chain

$ systemd-analyze critical-chain atd.service
```
### hostnamectl
Show the machine information.
```
$ hostnamectl

$ sudo hostnamectl set-hostname rhel7
```
### localectl
Check the local configuration.
```
$ localectl

$ sudo localectl set-locale LANG=en_GB.utf8
$ sudo localectl set-keymap en_GB
```
### timedatectl
Show the time zone.
```
$ timedatectl

$ timedatectl list-timezones                                                                                   
$ sudo timedatectl set-timezone America/New_York
$ sudo timedatectl set-time YYYY-MM-DD
$ sudo timedatectl set-time HH:MM:SS
```
### loginctl
Show the user information.
```
$ loginctl list-sessions

$ loginctl list-users

$ loginctl show-user username
```
## Unit
Systemd can manage all the units/resources.
There are 12 units:
```
Service unit
Target unit
Device Unit
Mount Unit
Automount Unit
Path Unit
Scope Unit
Slice Unit
Snapshot Unit
Socket Unit
Swap Unit
Timer Unit
```
### Show the units
```
$ systemctl list-units

$ systemctl list-units --all

$ systemctl list-units --all --state=inactive

$ systemctl list-units --failed

$ systemctl list-units --type=service
```
### Check the status of unit
```
$ systemctl status

$ sysystemctl status bluetooth.service

$ systemctl -H root@rhel7.example.com status httpd.service

$ systemctl is-active application.service

$ systemctl is-failed application.service

$ systemctl is-enabled application.service
```
### Manage unit
```
$ sudo systemctl start apache.service

$ sudo systemctl stop apache.service

$ sudo systemctl restart apache.service

$ sudo systemctl kill apache.service

$ sudo systemctl reload apache.service

$ sudo systemctl daemon-reload

$ systemctl show httpd.service

$ systemctl show -p CPUShares httpd.service

$ sudo systemctl set-property httpd.service CPUShares=500
```
### Dependency
```
systemctl list-dependencies nginx.service
systemctl list-dependencies --all nginx.service #show the target
```
### Unit configuration
Systemd reads configuration from `/etc/systemd/system/` by defualt, in which are mostly symblinks pointing to `/usr/lib/systemd/system/`.
`systemctl enable` can create symlink between them.
```
$ sudo systemctl enable clamd@scan.service

$ sudo ln -s  '/usr/lib/systemd/system/clamd@scan.service' '/etc/systemd/system/multi-user.target.wants/clamd@scan.service'
```
`systemctl disable` cancel the symlink.
`sudo systemctl disable clamd@scan.service`
### List all the files
```
$ systemctl list-unit-files

$ systemctl list-unit-files --type=service
```
There are four status:
enabled
disabled
static
masked

Configuration will take effect after restart:
```
$ sudo systemctl daemon-reload
$ sudo systemctl restart httpd.service
```
Configuration files are text format, and case sensitive(A/a).
```
$ systemctl cat atd.service

[Unit]
Description=ATD daemon

[Service]
Type=forking
ExecStart=/usr/bin/atd

[Install]
WantedBy=multi-user.target
```
### [Unit] block
First block.
Define the basic information and the relationship with other units.
```
Description：简短描述
Documentation：文档地址
Requires：当前 Unit 依赖的其他 Unit，如果它们没有运行，当前 Unit 会启动失败
Wants：与当前 Unit 配合的其他 Unit，如果它们没有运行，当前 Unit 不会启动失败
BindsTo：与Requires类似，它指定的 Unit 如果退出，会导致当前 Unit 停止运行
Before：如果该字段指定的 Unit 也要启动，那么必须在当前 Unit 之后启动
After：如果该字段指定的 Unit 也要启动，那么必须在当前 Unit 之前启动
Conflicts：这里指定的 Unit 不能与当前 Unit 同时运行
Condition...：当前 Unit 运行必须满足的条件，否则不会运行
Assert...：当前 Unit 运行必须满足的条件，否则会报启动失败
```
### [Install] block
Last block.
Define how to start.
```
WantedBy：它的值是一个或多个 Target，当前 Unit 激活时（enable）符号链接会放入/etc/systemd/system目录下面以 Target 名 + .wants后缀构成的子目录中
RequiredBy：它的值是一个或多个 Target，当前 Unit 激活时，符号链接会放入/etc/systemd/system目录下面以 Target 名 + .required后缀构成的子目录中
Alias：当前 Unit 可用于启动的别名
Also：当前 Unit 激活（enable）时，会被同时激活的其他 Unit
```
Details: https://www.freedesktop.org/software/systemd/man/systemd.unit.html
### [Service] block
Only for Service type unit.
```
Type：定义启动时的进程行为。它有以下几种值。
Type=simple：默认值，执行ExecStart指定的命令，启动主进程
Type=forking：以 fork 方式从父进程创建子进程，创建后父进程会立即退出
Type=oneshot：一次性进程，Systemd 会等当前服务退出，再继续往下执行
Type=dbus：当前服务通过D-Bus启动
Type=notify：当前服务启动完毕，会通知Systemd，再继续往下执行
Type=idle：若有其他任务执行完毕，当前服务才会运行
ExecStart：启动当前服务的命令
ExecStartPre：启动当前服务之前执行的命令
ExecStartPost：启动当前服务之后执行的命令
ExecReload：重启当前服务时执行的命令
ExecStop：停止当前服务时执行的命令
ExecStopPost：停止当其服务之后执行的命令
RestartSec：自动重启当前服务间隔的秒数
Restart：定义何种情况 Systemd 会自动重启当前服务，可能的值包括always（总是重启）、on-success、on-failure、on-abnormal、on-abort、on-watchdog
TimeoutSec：定义 Systemd 停止当前服务之前等待的秒数
Environment：指定环境变量
```
## Target
Target is a series of unit, like unit group.
```
$ systemctl list-unit-files --type=target

$ systemctl list-dependencies multi-user.target

$ systemctl get-default

$ sudo systemctl set-default multi-user.target

# 切换 Target 时，默认不关闭前一个 Target 启动的进程，
# systemctl isolate 命令改变这种行为，
# 关闭前一个 Target 里面所有不属于后一个 Target 的进程
$ sudo systemctl isolate multi-user.target
```
Target with traditional initd:
```
Traditional runlevel      New target name     Symbolically linked to...

Runlevel 0           |    runlevel0.target -> poweroff.target
Runlevel 1           |    runlevel1.target -> rescue.target
Runlevel 2           |    runlevel2.target -> multi-user.target
Runlevel 3           |    runlevel3.target -> multi-user.target
Runlevel 4           |    runlevel4.target -> multi-user.target
Runlevel 5           |    runlevel5.target -> graphical.target
Runlevel 6           |    runlevel6.target -> reboot.target
```
Difference between them:
```
1）默认的 RunLevel（在/etc/inittab文件设置）现在被默认的 Target 取代，位置是/etc/systemd/system/default.target，通常符号链接到graphical.target（图形界面）或者multi-user.target（多用户命令行）。

（2）启动脚本的位置，以前是/etc/init.d目录，符号链接到不同的 RunLevel 目录 （比如/etc/rc3.d、/etc/rc5.d等），现在则存放在/lib/systemd/system和/etc/systemd/system目录。

（3）配置文件的位置，以前init进程的配置文件是/etc/inittab，各种服务的配置文件存放在/etc/sysconfig目录。现在的配置文件主要存放在/lib/systemd目录，在/etc/systemd目录里面的修改可以覆盖原始设置。
```
## Journal
Systemd manages all the logs. `journalctl` can be used to check kernel log and app logs. It's located at `/etc/systemd/journald.con`.

```
$ sudo journalctl

# 查看内核日志（不显示应用日志）
$ sudo journalctl -k

# 查看系统本次启动的日志
$ sudo journalctl -b
$ sudo journalctl -b -0

# 查看上一次启动的日志（需更改设置）
$ sudo journalctl -b -1

# 查看指定时间的日志
$ sudo journalctl --since="2012-10-30 18:17:16"
$ sudo journalctl --since "20 min ago"
$ sudo journalctl --since yesterday
$ sudo journalctl --since "2015-01-10" --until "2015-01-11 03:00"
$ sudo journalctl --since 09:00 --until "1 hour ago"

# 显示尾部的最新10行日志
$ sudo journalctl -n

# 显示尾部指定行数的日志
$ sudo journalctl -n 20

# 实时滚动显示最新日志
$ sudo journalctl -f

# 查看指定服务的日志
$ sudo journalctl /usr/lib/systemd/systemd

# 查看指定进程的日志
$ sudo journalctl _PID=1

# 查看某个路径的脚本的日志
$ sudo journalctl /usr/bin/bash

# 查看指定用户的日志
$ sudo journalctl _UID=33 --since today

# 查看某个 Unit 的日志
$ sudo journalctl -u nginx.service
$ sudo journalctl -u nginx.service --since today

# 实时滚动显示某个 Unit 的最新日志
$ sudo journalctl -u nginx.service -f

# 合并显示多个 Unit 的日志
$ journalctl -u nginx.service -u php-fpm.service --since today

# 查看指定优先级（及其以上级别）的日志，共有8级
# 0: emerg
# 1: alert
# 2: crit
# 3: err
# 4: warning
# 5: notice
# 6: info
# 7: debug
$ sudo journalctl -p err -b

# 日志默认分页输出，--no-pager 改为正常的标准输出
$ sudo journalctl --no-pager

# 以 JSON 格式（单行）输出
$ sudo journalctl -b -u nginx.service -o json

# 以 JSON 格式（多行）输出，可读性更好
$ sudo journalctl -b -u nginx.serviceqq
 -o json-pretty

# 显示日志占据的硬盘空间
$ sudo journalctl --disk-usage

# 指定日志文件占据的最大空间
$ sudo journalctl --vacuum-size=1G

# 指定日志文件保存多久
$ sudo journalctl --vacuum-time=1years
```
## Target Exec Sequence
```
1. /etc/systemd/system/default.target，pointing to /usr/lib/systemd/system/graphical.target，dependencies: multi-user.target => basic.target => sysinit.target 。

2. local-fs.target: exec file system related kernel units according to /etc/fstab and /etc/inittab.

3. sysinit.target: start system units, like mount, swap, device, kernel, etc. 

4. basic.target: general units, especially graphical management unit, according to `/etc/systemd/system/basic.target.wants`

5. multi-user.target: sub units are located at `/etc/systemd/system/multi-user.target.wants`, set environment for multi-user, and firewall units.

6. systemd-logind.service: login. See more details with `systemctl help systemd-logind.service`, exct with `/usr/lib/systemd/system/getty@.service`.

Dependency: [System bootup process](https://www.freedesktop.org/software/systemd/man/bootup.html).
```
