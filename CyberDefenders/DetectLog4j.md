## CyberDefenders - DetectLog4j CTF ##

### WriteUp ###
***

1 What is the computer hostname?

Evidencia:

    c:\windows\debug\netsetup.log

Respuesta:

    VCW65

***
2 What is the Timezone of the compromised machine?

Evidencia:

    Registry
    Currentcontrolset\control\timezoneinformation - PST

Respuesta:

    UTC-8
***
3 What is the current build number on the system?

Evidencia:

    export explorer.exe
    right-click properties, details tab

Respuesta:

    14393
***
4 What is the computer IP?

Respuesta:

    192.168.112.139
***
5 What is the domain computer was assigned to?

Respuesta:

    cyberdefenders.org
***
6 When was myoussef user created?

Evidencia:

    security.evtx
    event id: 4720
    scrape user creation from Registry - SAM Hive
    from the SAM registry 61006e00 = 2021-07-27 20:35:12 UTC
Respuesta:

    2021-12-28 06:57:23 UTC
***
7 What is the user mhasan password Evidencia?

Evidencia:

    Registry - SAM Hive
Respuesta:

    https://www.linkedin.com/in/0xmohamedhasan/
***
8 What is the version of the VMware product installed on the machine?

Evidencia:

    Registry - Software Hive
Respuesta:

    6.7.0.40322
***
9 What is the version of the log4j library used by the installed VMware product?

Respuesta:

    2.11.2
***
10 What is the log4j library log level specified in the configuration file?

Respuesta:

    info
***
11 The attacker exploited log4shell through an HTTP login request. What is the HTTP header used to inject payload?

Respuesta:

    x-forward-for

***
12 The attacker used the log4shell.huntress.com payload to detect if vcenter instance is vulnerable. What is the first link of the log4huntress payload?

Evidencia:

    C:\ProgramData\VMware\VCenterServer\runtime\VmwareServiceSTS\logs\
Respuesta:

    log4shell.huntress.com:1389/b1292f3c-a652-4240-8fb4-59c43141f55a
***
13 When was the first successful login to vsphere WebClient?

    audit_events.log
Respuesta:

    28/12/2021 20:39:29 UTC
***
14 What is the attacker’s IP address?

    powershell logs - reverse payload

192.168.112.128
***
15 What is the port the attacker used to receive the cobalt strike reverse shell?

Evidencia:

    Powershell event logs
    Administrator PS ReadLine history
    Cyberchef
    mandiant’s speakeasy
    speakeasy

Respuesta:

    1337
***
16 What is the script name published by VMware to mitigate log4shell vulnerability?

Evidencia:

    VMware Article 87081
Respuesta:

    vc_log4j_mitigator.py
***
17 In some cases, you may not be able to update the products used in your network. What is the system property needed to set to ‘true’ to work around the log4shell vulnerability?

Respuesta:

    log4j2.formatMsgNoLookups
***
18 What is the log4j version which contains a patch to CVE-2021-44228?

Evidencia:

    Google
Respuesta:

    2.15.0
***
19 Removing JNDIlookup.class may help in mitigating log4shell. What is the sha256 hash of the JNDILookup.class?

Respuesta:

    0F038A1E0AA0AFF76D66D1440C88A2B35A3D023AD8B2E3BAC8E25A3208499F7E
***
20 Analyze JNDILookup.class. What is the value stored in the CONTAINER_JNDI_RESOURCE_PATH_PREFIX variable?

Respuesta:

    java:comp/env/
***
21 To gain some persistence the attacker dropped a malicious exe file. What is the malicious executable name?

Evidencia:

    Registry
    Run keys
    User Hive
    NTUSER.DAT
Respuesta:

    baaaackdooor.exe
***
22 When was the first submission of ransomware to virustotal?

Respuesta:

    2021-12-11 22:57:01
***
23 The ransomware downloads a text file from an external server. What is the key used to decrypt the URL?

Evidencia:

    ilspy

Respuesta:

    GoaahQrc
***
24 What is the ISP that owns that IP that serves the text file?

Evidencia:

    whois 3.145.115.94
Respuesta:

    Amazon
***
25 The ransomware check for extensions to exclude them from the encryption process. What is the second extension the ransomware checks for?

Evidencias:

    ilspy
    67 1d 2f 2e ^ ItAGEocK
    ilspy 2

Respuesta:

    ini

***
### Tools ###
<pre>
FTK Imager
Regripper 
ilSpy 
Mandiant’s Speakeasy 
CyberChef 
JAD (Java Decompiler) 
Eventviewer 
</pre>
