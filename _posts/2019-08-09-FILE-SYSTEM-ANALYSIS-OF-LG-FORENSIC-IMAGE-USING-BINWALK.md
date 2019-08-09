---
layout: post
title: "Resources for digital forensic practitioners"
icon: star-o
author: 2018alex
tags: [student life]
---
###INTRODUCTION
[Binwalk]( https://tools.kali.org/forensics/binwalk) is a simple Linux tool used for analysis of binary image files. Analyzing binary image files may include; reverse engineering, extracting firmware images, file systems, embedded files or executable codes from the binary images.  These binary images could be from firmware of routers, IOT devices or any digital device.
To understand the way the tool works, I ran analysis on a forensic image obtained from 4GB [eMMC]( https://www.datalight.com/solutions/technologies/emmc/what-is-emmc) chip off LG Smart TV. The image was obtained by [chip-off]( https://www.gillware.com/digital-forensics/chip-off-forensics-services/) forensics method and read through HancomGMD product MD-RED. From my analysis using Binwalk, it revealed the following: Squashfs filesystem, Ext4 Linux filesystem, an Ubiquiti partition header and a YAFFS filesystem. 

The steps taken to achieve this is what I would like to share and described below.

###LG IMAGE ANALYSIS USING BINWALK
1.	Install Binwalk: First run the command “apt get install binwalk” to install the tool
2.	Running Binwalk tool: On the LG Forensic Image (KLM4G1FETE-B041_ChipOff_20190806_ALLPART.mdf) as seen in the syntax in screen shot image below 
<img src="tool.png" style="float: left; margin-right:" />
3.	Outcome of Running Binwalk: Screenshots of outcome header 
<img src="header.png" style="float: left; margin-right: 10px;" />
Showing Ubiquiti partition header, Squashfs filesystem and linux Ext4 filesystem.
4.	Extracting the squashfs filesystem: using the dd command to copy out the Squashfs file system file from input file KLM4G1FETE-B041_Chipoff_ALLPART.mdf on offset 4718592 bit by bit to output file called KLM4G.sqfs with the syntax as seen on the screen shot below 
<img src="dd_cmd_sqfs.png" style="float: left; margin-right: 10px;" />
A Squash file called KLM4G.sqfs was created which is 3.9GB
5.	Running the unsquashfs command on the KLM4G.sqfs using the syntax below
<img src="running unsquashfs.png" style="float: left; margin-right: 10px;" />
Showing 451 inodes and 532 blocks
6.	Outcome of unsquashfs is seen below
<img src="outcome_of_unsquashfs.png" style="float: left; margin-right: 10px;" />
Uncompressed 134 files, 201 directories
7.	Hexdump of ubiquiti filesystem: to view the partition in hexadecimal at offset 1048792 with the syntax on the screen shot to have a view of what partition contained 
<img src="hexdump ubiquiti.png" style="float: left; margin-right: 10px;" />
<img src="endhex dump.png" style="float: left; margin-right: 10px;" />
8.	YAFFS FILESYSTEM: The screenshot showing the presence of YAFFS filesystem below
<img src="YAFFS FILESYSTEM.png" style="float: left; margin-right: 10px;" />

### WHAT IS SQUASHFS FILE SYSTEM
Squashfs is a highly compressed read-only filesystem for Linux. Squashfs compresses both files, inodes and directories, and supports block sizes up to 1Mbytes for greater compression it uses Mksquashfs and Unsquashfs utility tools for compressing files or embedding one filesystem into another or extract same from another file. 
For further read on Squashfs File system, its’ usage, features and the impact of compression on computer hardware and other features refer to the these 
* [Sourceforge]( https://sourceforge.net/p/squashfs/news/)
* [SquashFS-HowTo]( http://tldp.org/HOWTO/SquashFS-HOWTO/whatis.html) 

### WHAT IS THE IMPLICATION OF SQUASHFS ON OUR FILE
The LG webos system files are housed in this partition file which when uncompressed is a large file with size 3.7GB. Squashfs filesystem made it possible to compress these files, this is the reason the original size of the entire image file was about 3.64G 
<img src=" created file and root folder.png" style="float: left; margin-right: 10px;" />
<img src="rootfolder.png" style="float: left; margin-right: 10px;" />
This shows the uncompressed folder structure for the Squash-root

### EXT4 LINUX FILESYSTEM
This file system supports large addressing of blocks and this file partition contains the user activities files. For further reading refer to article on [ext4 filesystem]( https://opensource.com/article/18/4/ext4-filesystem) 

### UBIQUITI PARTITION HEADER
The Ubiquiti partition header makes it possible to hold several partitions and implement hashes between the partitions. For further reading refer to this article on [ubiquiti partition](http://www.minupilv.ee/review_ubnt_unifi.txt)

### YAFFS
YAFFS means Yet Another Flash File System it is implemented in NAND flash devices or embedded systems like our eMMC LG flash so that data can be written for persistent storage. For further reading refer to this article on [YAFFS](http://processors.wiki.ti.com/images/tmp/f1260678820-891405877.html)






