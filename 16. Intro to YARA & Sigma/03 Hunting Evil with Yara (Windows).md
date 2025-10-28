---
title: 03 Hunting Evil with Yara (Windows)
updated: 2025-10-28 04:24:44Z
created: 2025-10-26 01:04:57Z
---

# Hunting Evil with YARA (Windows Edition)

## 1\. Overview

This section demonstrates how **YARA** can be used on **Windows systems** to detect and hunt threats both **on disk** and **in memory**, as well as within **ETW (Event Tracing for Windows)** data using **SilkETW**.

The exercises are performed on a Windows target machine accessible via RDP.

* * *

## 2\. Hunting Malicious Files on Disk

- The malware sample `dharma_sample.exe` is analyzed using **HxD** (a hex editor).
    
- Two unique strings are identified:
    
    - `C:\crysis\Release\PDB\payload.pdb`
        
    - `sssssbsss`
        
- These strings are converted into **hex patterns** and embedded into a custom YARA rule:
    

```
rule ransomware_dharma {
    strings:
        $string_pdb = {433A5C6372797369735C52656C656173655C5044425C7061796C6F61642E706462}
        $string_ssss = {73 73 73 73 73 62 73 73 73}
    condition:
        all of them
}

```

- The rule is executed using:
    
- ```
      yara64.exe -s C:\Rules\yara\dharma_ransomware.yar C:\Samples\YARASigma\ -r 2>null
      
    ```
    
- **Detected Files:** `dharma_sample.exe`, `check_updates.exe`, `microsoft.com`, `KB5027505.exe`, `pdf_reader.exe`.
    

* * *

## 3\. Scanning Running Processes for Meterpreter Shellcode

- A **Meterpreter reverse TCP shellcode** YARA rule is loaded to detect in-memory payloads.

```
rule meterpreter_reverse_tcp_shellcode {
    strings:
        $s1 = {fce8 8?00 0000 60}
        $s2 = {648b ??30}
        $s3 = {4c77 2607}
        $s4 = "ws2_"
        $s5 = {2980 6b00}
        $s6 = {ea0f dfe0}
        $s7 = {99a5 7461}
    condition:
        5 of them
}

```

- The sample `htb_sample_shell.exe` injects Meterpreter shellcode into `cmdkey.exe` (PID 9084).
    
- Using PowerShell:
    
- ```
      Get-Process | ForEach-Object { & "yara64.exe" "C:\Rules\yara\meterpreter_shellcode.yar" $_.id }
      
    ```
    
- **Result:** PID 9084 (cmdkey.exe) is flagged as infected with Meterpreter shellcode.
    

* * *

## 4\. Scanning ETW Data with SilkETW

### What is ETW?

**Event Tracing for Windows (ETW)** is a high-performance event logging framework that records activity from:

- **Providers** (event sources),
    
- **Controllers** (that start/stop traces), and
    
- **Consumers** (that process the logged data).
    

Useful providers include:

- `Microsoft-Windows-Kernel-Process` – process monitoring
    
- `Microsoft-Windows-PowerShell` – script execution
    
- `Microsoft-Windows-DNS-Client` – DNS queries
    
- `Microsoft-Windows-Kernel-Network` – network activity
    
- `Microsoft-Antimalware-Service` – AV monitoring
    

* * *

### SilkETW Overview

**SilkETW** allows capturing ETW events with built-in **YARA integration** to tag or filter event data.  
Located at `C:\Tools\SilkETW\v8\SilkETW`.

* * *

### Example 1: PowerShell ETW Scanning

Command:

```
SilkETW.exe -t user -pn Microsoft-Windows-PowerShell -ot file -p ./etw_ps_logs.json -l verbose -y C:\Rules\yara -yo Matches

```

YARA rule (`etw_powershell_hello.yar`):

```
rule powershell_hello_world_yara {
    strings:
        $s0 = "Write-Host"
        $s1 = "Hello"
        $s2 = "from"
        $s3 = "PowerShell"
    condition:
        3 of ($s*)
}

```

PowerShell command executed:

`Invoke-Command -ScriptBlock {Write-Host "Hello from PowerShell"}`

**Result:** YARA detects the string sequence — match confirmed in SilkETW output.

* * *

### Example 2: DNS ETW Scanning

Command:

```
SilkETW.exe -t user -pn Microsoft-Windows-DNS-Client -ot file -p ./etw_dns_logs.json -l verbose -y C:\Rules\yara -yo Matches

```

YARA rule (`etw_dns_wannacry.yar`):

```
rule dns_wannacry_domain {
    strings:
        $s1 = "iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com"
    condition:
        $s1
}

```

DNS command executed:

`ping iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com`

**Result:** SilkETW flags a match — confirming detection of Wannacry’s kill-switch domain.

* * *

## 5\. Key Takeaways

- **YARA** effectively detects malware across **disk**, **memory**, and **event data**.
    
- **Custom rules** can be crafted using hex patterns or unique identifiers.
    
- **SilkETW** extends YARA’s reach into **real-time event tracing**, providing a powerful forensic and detection toolset.
    
- **Combining disk, process, and ETW scanning** offers layered visibility for threat hunting in Windows environments.
    

&nbsp;