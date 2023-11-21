---
layout: page
title: mySystemStats
description: A tool to track CPU and memory usage 
img: assets/img/7.jpg
importance: 3
category:
---

mySystemStats is a program implemented in C that displays metrics about the machine's CPU and memory usage to the user. The user can input how many samples of data will be collected and how often they will be collected. This program is similar to htop on Linux machines.

Please visit the [GitHub repository](https://github.com/AB20CS/mySystemStats) for more information on how to run this program on your machine.

### Sample Output 

    Nbr of samples: 10 -- every 1 secs
    Memory usage: 756 kilobytes
    ---------------------------------------
    ### Memory ### (Phys.Used/Tot -- Virtual Used/Tot)
    10.87 GB / 16.95 GB -- 11.17 GB / 68.49 GB      |o 0.00 (11.17)
    10.88 GB / 16.95 GB -- 11.18 GB / 68.49 GB      |o 0.00 (11.18)
    10.87 GB / 16.95 GB -- 11.18 GB / 68.49 GB      |@ -0.00 (11.18)
    10.88 GB / 16.95 GB -- 11.18 GB / 68.49 GB      |o 0.00 (11.18)
    10.88 GB / 16.95 GB -- 11.18 GB / 68.49 GB      |@ -0.00 (11.18)
    10.88 GB / 16.95 GB -- 11.19 GB / 68.49 GB      |o 0.00 (11.19)
    10.90 GB / 16.95 GB -- 11.21 GB / 68.49 GB      |o 0.00 (11.21)
    10.90 GB / 16.95 GB -- 11.21 GB / 68.49 GB      |o 0.00 (11.21)
    10.90 GB / 16.95 GB -- 11.21 GB / 68.49 GB      |o 0.00 (11.21)
    10.96 GB / 16.95 GB -- 11.27 GB / 68.49 GB      |o 0.00 (11.27)
    ---------------------------------------
    Number of cores: 8
    total cpu use = 11.60%
            * 0.00%
            * 0.88%
            || 1.34%
            ||| 2.62%
            || 1.10%
            * 0.87%
            |||||||||||| 11.63%
            |||||||||||| 11.74%
            |||| 3.09%
            |||||||||||| 11.60%
    ---------------------------------------
    ### Sessions/users ###

    user1       pts/0 (138.51.12.217)  

    user1       pts/1 (tmux(3773782).%0)

    user2       tty7 (:0)  

    user1       pts/2 (tmux(3773782).%1)  

    user1       pts/3 (tmux(3773782).%3)

    user1       pts/4 (tmux(3773782).%4)
    ---------------------------------------
    ### System Information ###
    System Name = Linux
    Machine Name = DESKTOP-O6RL1IB
    Version = #1237-Microsoft Sat Sep 11 14:32:00 PST 2021
    Release = 4.4.0-19041-Microsoft
    Architecture = x86_64
    ---------------------------------------
