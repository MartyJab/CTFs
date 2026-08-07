
# File System FAT32

>A filesystem is the structure that an OS uses to organize, store and retrieve files and directories. It also includes the metadata of the files. FAT32 is not state of the art anymore because of the maximum filesize of 4GB.

## FAT32 Structure: Reserved and FAT Areas

### We have a hypothetical file B and its cluster chain starts at cluster F and ends at cluster 10 . What would be the value of the FAT entry at cluster F?

Bytes per sector are saved in the bootsector. 00 02 in big endian equals 512
![](./images/Pasted_image_20260806132249.png)


One Sector per Cluster
![](./images/Pasted_image_20260806132249.png)


Reserved Sectors equals 6270 in decimal (big endian)
![](./images/Pasted_image_20260806122843.png)


Number of FATs is 2
![](./images/Pasted_image_20260806123547.png)


FAT starts at 6270 * 512 (Reserved Sectors * Bytes per Sector) = 3.210.240 = 0x30FC00

Start of the FAT
![](./images/Pasted_image_20260806124026.png)


Adding the Cluster F Offset 0x30FC00 + 0xF *4 (Cluster F * 4 bytes) = 0x30FC3C 

Apperently the task doesn't want you to find it in the image. You are just supposed to caculate it:
Cluster F stores the value of the next cluster which is 10. Each cluster is 4 bytes long so in little endian it is 10 00 00 00.

### At which offset does the FAT2 table start?

Sectors per FAT
![](./images/Pasted_image_20260806131900.png)


Start of FAT2 = Reserved Sectors + Sectors per FAT (for the first FAT) = 7231 Sectors --> Offset 3702272 = 0x387E00
![](./images/Pasted_image_20260806132249.png)


## FAT32 Structure: Data Area

### What is the filename of the file that starts at cluster 9?

Already found:
Bytes per Sector = 512
Reserved Sectors = 6270
Sectors per FAT = 961
Number of FATs = 2
FAT1 starts at 0x30FC00
FAT2 starts at (6270 + 961) * 512 = 0x387E00

Data Area/Root Directory starts at (6270 + 961 + 961) * 512 = 0x400000

Every Directory Entry shows the first cluster number in the 5th and 6th last byte.
![](./images/FAT32-Analysis-1.png)

So the name of the filename is careers.txt

### What is the creation time of the file that starts at cluster 9?

These bytes show the creation time
![](./images/FAT32-Analysis-2.png)

In big endian it is 84F4 --> 4:39:40 PM

## Hidden Files and Directories

### What is the short file name of the hidden file in the M@lL0v3 directory?

BEMYVA~1
![](./images/FAT32-Analysis-3.png)

### What is the flag found during automated analysis?

THM{F0uNdTh3H!Dd3nF1l3}
![](./images/FAT32-Analysis-4.png)

## Timestomp

### What is the Accessed timestamp of the discovered suspicious file?

Using the Timeline feature of Autopsy
![](./images/FAT32-Analysis-5.png)
2018-01-10 00:00:00

### What is the flag found during the automated analysis?

Text found in the of the file
![](./images/FAT32-Analysis-6.png)
THM{T1m3St0Mp3D}

## File Deletion and Clear Persistence
What is the output of the deleted PowerShell script after executing it?

The deleted File
![](./images/FAT32-Analysis-8.png)

Script output is THM{r3Tr!3v3D_3v!d3nC3}
![](./images/FAT32-Analysis-7.png)


