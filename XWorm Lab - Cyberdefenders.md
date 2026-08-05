Scenario:
An employee accidentally downloaded a suspicious file from a phishing email. The file executed silently, triggering unusual system behavior. As a malware analyst, your task is to analyze the sample to uncover its behavior, persistence mechanisms, communication with Command and Control (C2) servers, and potential data exfiltration or system compromise.

Decompile the file:
![[Question0_Decompile 1.png]]

Question 1: What is the compile timestamp (UTC) of the sample?

Investigate the PE-Headers using Detect it Easy
![[Question1.png]]


Question 2: Which legitimate company does the malware impersonate in an attempt to appear trustworthy?

Defined in the AssemblyInfo.cs File
![[Question2.png]]


Question 3: How many anti-analysis checks does the malware perform to detect/evade sandboxes and debugging environments? 

Anti-Analysis checks are probably in the entry point class.

Code seems to be obfuscated (no result)
![[Pasted image 20260723142148.png]]

Looking for the enrypoint in IL.
![[Pasted image 20260723142249.png]]

You can see what that the function that includes the .entrypoint in IL is called here.
![[Pasted image 20260723143052.png]]

Search for the function name in the c# code using grep.
![[Pasted image 20260723143314.png]]

Count the functions with a bool return type because that is likely to be an Anti-Analysis check.
![[Pasted image 20260723143450.png]]


Question 4: What is the name of the scheduled task created by the malware to achieve execution with elevated privileges?

schtasks.exe for creating a task (line 83).
![[Pasted image 20260803202445.png]]

After tn comes name of the new task.
![[Pasted image 20260803202614.png]]

Definition of the method.
![[Pasted image 20260803202804.png]]

![[Pasted image 20260803203530.png]]

![[Pasted image 20260803203725.png]]

Method input.
![[Pasted image 20260803202207.png]]

Method dependency.
![[Pasted image 20260804165257.png]]
![[Pasted image 20260804165341.png]]

Static and isolated execution by copying and modifying the method shows the decrypted name:
![[Pasted image 20260804165150.png]]

![[Pasted image 20260804165205.png]]