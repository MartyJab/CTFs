# Volatility3

https://blueteamlabs.online/home/challenge/memory-analysis-ransomware
## The Account Executive called the SOC earlier and sounds very frustrated and angry. He stated he can’t access any files on his computer and keeps receiving a pop-up stating that his files have been encrypted. You disconnected the computer from the network and extracted the memory dump of his machine and started analyzing it with Volatility. Continue your investigation to uncover how the ransomware works and how to stop it!

## Run “vol.py -f infected.vmem --profile=Win7SP1x86 psscan” that will list all processes. What is the name of the suspicious process?

@WannaDecryptor
![](./images/08_Memory_Analysis_Ransomware-1792080792377.webp)

## What is the parent process ID for the suspicious process?
2732 - Look in the column PPID

## What is the initial malicious executable that created this process?
![](./images/08_Memory_Analysis_Ransomware-1792080896441.webp)

## If you drill down on the suspicious PID (vol.py -f infected.vmem --profile=Win7SP1x86 psscan | grep (PIDhere)), find the process used to delete files

![](./images/08_Memory_Analysis_Ransomware-1792082285776.webp)

## Find the path where the malicious file was first executed
![](./images/08_Memory_Analysis_Ransomware-1792082597032.webp)


## Can you identify what ransomware it is?
WannaCry  
![](./images/08_Memory_Analysis_Ransomware-1792082667340.webp)

## What is the filename for the file with the ransomware public key that was used to encrypt the private key? (.eky extension)
![](./images/08_Memory_Analysis_Ransomware-1792083067945.webp)

