# WordPress Attack Analysis
https://blueteamlabs.online/home/challenge/log-analysis-compromised-wordpress

One of our WordPress sites has been compromised but we're currently unsure how. The primary hypothesis is that an installed plugin was vulnerable to a remote code execution vulnerability which gave an attacker access to the underlying operating system of the server.

## Identify the URI of the admin login panel that the attacker gained access to (include the token)

Using lnav. Filtering for "admin" and filtering out a private IP address
![](./images/09_Log_Analysis_WordPress-1792146403884.webp)

## Can you find two tools the attacker used?

Searching through the traffic caused by the attacker IP adresses
![](./images/09_Log_Analysis_WordPress-1792157618457.webp)

![](./images/09_Log_Analysis_WordPress-1792163545921.webp)

## The attacker tried to exploit a vulnerability in ‘Contact Form 7’. What CVE was the plugin vulnerable to? (Do some research!)
![](./images/09_Log_Analysis_WordPress-1792161579979.webp)
## What is the name of the PHP web shell file?

![](./images/09_Log_Analysis_WordPress-1792149326851.webp)

## What plugin was exploited to get access?
![](./images/09_Log_Analysis_WordPress-1792164163837.webp)
## What was the HTTP response code provided when the web shell was accessed for the final time?


![](./images/09_Log_Analysis_WordPress-1792162090595.webp)
