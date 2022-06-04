---
title: Initialization system System V
subtitle: Linux

# Summary for listings and search engines
summary: 

# Link this post with a project
projects: []

# Date published
date: '2022-06-04T00:00:00Z'

# Date updated
lastmod: '2022-06-04T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'featured.jpg'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin

tags:
  - Academic

categories:
  - programming languages
---


# - Что это и зачем?
System V is a version of the Unix operating system released in 1983.

The initialization system in UNIX and Linux is a set of programs for managing the formation of a working environment: a text / graphical working environment or a service node of a computer network. The traditional name for a main program is init. Her PID = 1.

Init is the parent of all processes. Its main task is to create scripted processes from the /etc/inittab file. This file usually contains entries that tell init to spawn getty for each line that users can log in to. It also controls the offline processes required by any system. A runlevel is a programmatic configuration of a system that allows only a given group of processes to exist. The processes spawned by init at each of these runlevels are defined in the /etc/inittab file.

Essentially, init organizes and maintains all user space, which also includes checking and mounting file systems, starting the necessary user services, and switching to user state when the system has finished booting up.

# - How does it work?
Init systems start daemons with scripts, with each script starting one daemon and each successive script waiting for the previous script to complete.

A daemon is a process that runs in the background with no connection to a GUI or terminal. Daemons are typically started at system boot and remain operational until the system is shut down. In modern technical documentation, daemons are most commonly referred to as services.

## Process ID 1

The bootloader transfers control of the system to the OS kernel. After a short period of time, the OS kernel starts the init system daemon. This init system daemon (/sbin/init) is the first daemon started within the system, so the corresponding process gets the process ID 1 (PID 1). The init system daemon never exits.

## Configuration options in the /etc/inittab file

After the binary file /sbin/init is executed, the configuration file /etc/inittab is read first. In this file, the daemon will look for the value of the initdefault variable (equal to 3 in the example below).

## Variable initdefault

The value of the initdefault variable specifies the default runlevel. On some Linux distributions, the /etc/inittab file contains a short description of runlevels similar to the following translated description from the corresponding Red Hat Enterprise Linux 4 distribution file.

Execution level 0 corresponds to system shutdown. Runlevel 1 is used for troubleshooting because only the root user can log in, and only the console can log in. Runlevel 3 is typical for servers, while runlevel 5 is typical for desktop computers (those that log in to the system in graphical mode). With the exception of runlevels 0, 1, and 6, the various runlevels may differ depending on the distribution. For example, the Debian distribution and derivative Linux distributions at runlevels 2 and 5 have the ability to log in using a network connection and a GUI. Therefore, you should always refer to the correct description of your system's runlevels.

## sysinit script

Script /etc/rc.d/rc.sysinit

The following line in the /etc/inittab configuration file on Red Hat and derivative distributions looks like this:

>si::sysinit:/etc/rc.d/rc.sysinit

This entry means that regardless of the chosen runlevel, the init system will execute the /etc/rc.d/rc.sysinit script. This script initializes the hardware, sets some basic environment variables, populates the /etc/mtab file when mounting filesystems, mounts the swap partition, and performs other necessary actions.

The egrep command above can be replaced with the similar grep command below:

>grep "^# Ini∥Sta∥Che".

## Initialization scripts

The init daemon will continue reading the /etc/inittab configuration file and jump to the following section:

>l0:0:wait:/etc/rc.d/rc 0
>l1:1:wait:/etc/rc.d/rc 1
>l2:2:wait:/etc/rc.d/rc 2
>l3:3:wait:/etc/rc.d/rc 3
>l4:4:wait:/etc/rc.d/rc 4
>l5:5:wait:/etc/rc.d/rc 5
>l6:6:wait:/etc/rc.d/rc 6

The init daemon must run the init script with a single parameter that matches the runlevel. In fact, the fields in the configuration file /etc/inittab are separated by colons. The second field describes the execution level at which the command on this line should be executed. Thus, in both cases, only one of the seven commands is executed, depending on the current runlevel set using the initdefault variable.

## Directories for storing init scripts

If you look at the contents of one of the /etc/rcX.d/ directories, you can find many scripts (or script references) whose names start with either the letter K or uppercase S.

>[root@RHEL52 rc3.d]# ls -l | tail -4
>lrwxrwxrwx 1 root root 19 окт 11  2008 S98haldaemon -> ../init.d/haldaemon
>lrwxrwxrwx 1 root root 19 окт 11  2008 S99firstboot -> ../init.d/firstboot
>lrwxrwxrwx 1 root root 11 янв 21 04:16 S99local -> ../rc.local
>lrwxrwxrwx 1 root root 16 янв 21 04:17 S99smartd -> ../init.d/smartd

The /etc/rcX.d/ directories only contain links to scripts located in the /etc/init.d/ directory. Links make it possible to use scripts with distinct names. When you move to a new runlevel, all scripts whose names start with the letter K or uppercase S start executing after being sorted alphabetically by name. Scripts whose names begin with the letter K will be executed first with a single parameter, stop. The rest of the scripts whose names begin with the letter S will be executed with a single start parameter passed.

All of these operations are performed by the /etc/rc.d/rc script on the Red Hat distribution and the /etc/init.d/rc script on the Debian distribution.

## mingetty daemons

Description of mingetty daemons in /etc/inittab configuration file

Almost at the end of the /etc/inittab configuration file is a section describing the conditions for starting and restarting several mingetty daemons.

### Run gettys in standard runlevels

>1:2345:respawn:/sbin/mingetty tty1
>2:2345:respawn:/sbin/mingetty tty2
>3:2345:respawn:/sbin/mingetty tty3
>4:2345:respawn:/sbin/mingetty tty4
>5:2345:respawn:/sbin/mingetty tty5
>6:2345:respawn:/sbin/mingetty tty6

### Mingetty daemons and /bin/login executable

The /sbin/mingetty daemon prints a message to the virtual console and allows you to enter a user ID. It then executes the /bin/login binary, passing in the user ID entered. The /bin/login program checks if the user information is present in the /etc/passwd file and asks for a password (and also checks that it is correct). If the password is correct, the /bin/login program transfers control to the shell specified in the /etc/passwd file.

#### What are the features?
- Writing service startup files in bash;
- Sequential start of services;
- Sort launch order using numbers in filenames;
- Commands to start, stop and check the status of services.
