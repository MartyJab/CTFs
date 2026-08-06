# Partitioning Scheme MBR & GPT

https://tryhackme.com/room/mbrandgptanalysis?utm_campaign=social_share&utm_medium=social&utm_content=share-completed-room&utm_source=copy&sharerId=67a5a61f353210ecc7a1169d

> A partitioning scheme defines how a storage device is organized into partitions and where these partitions start and end. Today GPT is the standard and MBR is obsolete.
## MBR (Master Boot Record)
### What is the size of the second partition in the MBR file found in `C:\Analysis\MBR\`? (rounded to the nearest GB)

The partition table starts at the byte 446. Each partition table entry is 16 Bytes long. So the second partition entry start at byte 462.
![]Pasted_image_20260805141550.png)

The last 4 Bytes contain the number of sectors.
![[./images/Pasted image 20260805141757.png]]

00 38 EB 01 is little endian and 01 EB 38 00 in big endian which is 3219512 in decimal.
![[Pasted image 20260805142054.png]]
Sector size is 512 Byte so 32192512 * 512 Byte = 16482566144 Byte ≈ 16 GB

## 2nd MBR:

### How many partitions are on the disk?

After the first partition entry every Byte is 0. So there is only one partition.
![[Pasted image 20260805154003.png]]
![[Pasted image 20260805153941.png]]

### What is the first byte at the starting LBA of the partition? (represented by two hexadecimal digits)

The logical address of the first partition is 00 08 00 00 (shown in the challenge). 00 08 00 00 in big endian equals 2048 in decimal. So the first partition starts at the offset 2048 * 512 = 1048576.
![[Pasted image 20260805155328.png]]
![[Pasted image 20260805155342.png]]
The first Byte is EB.

### What is the type of the partition?
It shows at the start of the partition.
![[Pasted image 20260805155525.png]]

### What is the size of the partition? (rounded to the nearest GB)

FTK Imager shows the size in MebiBytes. 
![[Pasted image 20260805161849.png]]
30 718 MiB * 1024 * 1024  = 32 210 157 568 Byte ≈ 32GB
### What is the flag hidden in the Administrator's Documents folder?

![[Pasted image 20260805162717.png]]

## GPT (GUID Partition Table)

### What is the partition type GUID of the 2nd partition given in the attached GPT file?

The partition type GUID is saved in the first 16 bytes of the partition array entry.

The partition array is in the 3rd sector of the disk. A sector is 512 bytes long and an partition entry 128 bytes.

We multiply 512 by 2 because the first sector starts at byte 0 and not 512.
We add 128 for the second partition entry.
512 * 2 + 128 = 1152
![[Pasted image 20260805170540.png]]
![[Pasted image 20260805170556.png]]
We need to convert it to big endian which comes to: 
E3C9E316-0B5C-4DB8-817D-F92DF00215AE

## Bootkits in .efi files

### What is the malicious string embedded in the bootloader?

Using a hint.
![[Pasted image 20260805172856.png]]

Finding random bytes which look like base64 format.
![[Pasted image 20260805172928.png]]
![[Pasted image 20260805173421.png]]
