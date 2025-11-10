---
title: What every Windows user must know about DISM and SFC
date: 2024-01-16 17:50:00 +0800
categories: [Blogging]
tags: [WindowsOS]     # TAG names should always be lowercase
math: true
comments: true
youtubeId: 
---

# This blog post is NOT original! The SOURCE and ALL CREDITS GO TO HERE: https://confidentialfiles.wordpress.com/2023/08/10/what-every-windows-user-must-know-about-dism-and-sfc/

### What every Windows user must know about DISM and SFC

Microsoft Windows offers two built-in utilities for repairing the operating system: DISM and SFC. As of this writing, these utilities have gained farcical popularity! People who see the slightest sign of something wrong with their dog run them.

This article briefly covers what everyone who would ever run these tools must know.
Overview

Disk Image Servicing Manager (DISM) is a comprehensive tool with many functions, one of which is to repair the Windows component store. For example, DISM can replace the following file if it ever becomes corrupt: C:\Windows\WinSxS\x86_wwf-system.workflow.runtime_31bf3856ad364e35_10.0.19041.1_none_beed4f9987070f23

You can invoke DISM via its CLI executable file (dism.exe) or PowerShell module. DISM only repairs Windows 8 and later. By default, DISM uses the cloud as its source of repair. You can specify a different source, though.

System File Checker (SFC) is older. It has been in every version of Windows since 1998. It repairs legacy Windows components. For example, SFC can restore C:\Windows\System32\xcopy.exe if it ever goes missing. SFC uses the component store as its source of repair.
How to use them correctly

As mentioned above, SFC’s repair source is DISM’s repair target. Therefore, to ensure that SFC has a healthy repair source, you must run SFC immediately after DISM. Do not run anything that requires a system restart before, between, or alongside those two.

In addition, healthy files need a healthy disk. So, you may benefit from running a disk checkup before DISM or SFC.

To repair the running OS from PowerShell, issue the following commands. If you ever forgot them, PowerShell’s inline completion is there to help you.

    Repair-Volume -DriveLetter C -Scan
    # ----- Restart your system now -----
    Repair-WindowsImage -Online -RestoreHealth
    SFC.exe /ScanNow

To repair the running OS from the legacy Command Prompt, issue the following commands. Unlike PowerShell, Windows Command Prompt provides no coding aids.

    ChkDsk c: /Scan /ForceOfflineFix
    REM ----- Restart your system now -----
    DISM.exe /Online /Cleanup-Image /RestoreHealth
    SFC.exe /ScanNow

To repair the Component Store from a local WIM/ESD file using the local source files (without using Windows Update online services), run the following command (remember to specify the Windows version index in the image file):


    ChkDsk c: /Scan /ForceOfflineFix
    REM ----- Restart your system now -----
    DISM /online /cleanup-image /restorehealth /source:WIM:D:\sources\install.wim:1 /limitaccess
    SFC.exe /ScanNow
    
      or from ESD file:
      DISM /online /cleanup-image /restorehealth /source:ESD:D:\sources\install.esd:1 /limitaccess

### Mistakes to avoid

1. Do not change the order of the commands. The correct order, as mentioned above, is the disk checkup, DISM, and SFC.
2. If one command returned an error message, do not proceed to the next. These error messages are indicators of the problems you need to solve.
3. Do not run anything else while you’re running a disk checkup, DISM, or SFC. Close as many apps, processes, and tasks as possible beforehand.
4. Do not run DISM with /StartComponentCleanup, /CheckHealth, or /ScanHealth. If you don’t know what they are doing, you’re not the right person to run them.
5. Do not run chkdsk c: /f. This command is obsolete.

### Limitations

As explained above, DISM and SFC have narrow uses. They repair the component store and legacy components respectively. More specifically, DISM and SFC cannot repair the following:

* Microsoft Store apps bundled with Windows: To fix these, try resetting them via the Settings app or the Appx module of PowerShell.
* Registry: The only surefire way of fixing the Windows Registry is through prevention via backup.
* Defender Antivirus: Short of reinstalling Windows, there is no way to fix this item because it breaks easily and resists repair. To add insult to injury, most people, including those who work for Microsoft, don’t know the difference between “Defender Antivirus” and “Windows Security.” (Yes, they are distinct. On Windows Server, you can install them separately.)
* Edge: To fix this item, download a fresh copy from Microsoft’s website. However, it might resist repair because Microsoft is actively looking for ways to prevent users from removing it.
* OneDrive: To fix this item, just reinstall it. You can grab a fresh copy from Microsoft’s website.

### DISM’s log

DISM creates a log at C:\Windows\Logs\DISM\dism.log. You can change the log’s location and level of details, but don’t bother. It contains nothing of value. It consists of 113 entries that DISM inserts at the moment of execution and 48 others at termination. DISM logs nothing for the entire length of its operation, which takes 10 to 20 minutes.

DISM and SFC are frontends for Component-Based Servicing (CBS), which keeps a log at C:\Windows\Logs\CBS\CBS.log, where you can find the details of changes that DISM and SFC have made to your system.

### Further reading

<li><strong><a rel="noreferrer noopener" href="https://support.microsoft.com/en-us/windows/using-system-file-checker-in-windows-365e0031-36b1-6031-f804-8fd86e0ef4ca" target="_blank">OS integrity diagnosis cheat sheet</a>:</strong> This short Microsoft Support article sums up this article in 4 bullet points, telling you how to run DISM and SFC in correct order. This is your cheat sheet.</li>

<li><strong><a rel="noreferrer noopener" href="https://learn.microsoft.com/en-us/troubleshoot/windows-server/deployment/fix-windows-update-errors" target="_blank">Fix Windows Update errors by using the DISM or System Update Readiness tool</a>:</strong> This Microsoft Learn article has in-depth info about using DISM (and its Windows 7 counterpart, SUR) to fix corrupt or missing OS files.</li>

<li><strong><a rel="noreferrer noopener" href="https://support.microsoft.com/en-us/topic/use-the-system-file-checker-tool-to-repair-missing-or-corrupted-system-files-79aa86cb-ca52-166a-92a3-966e85d4094e" target="_blank">Use the System File Checker tool to repair missing or corrupted system files</a>:</strong> This Microsoft Support article has in-depth info on the second step of fixing corrupt or missing OS files, which SFC. It even teaches you how to automate extracting repair details from the CBS.log file.</li>

<li><strong><a rel="noreferrer noopener" href="https://techcommunity.microsoft.com/t5/ask-the-performance-team/understanding-component-based-servicing/ba-p/373012" target="_blank">Understanding Component-Based Servicing</a>:</strong> This TechNet article, written by CC Hameed (not Craig Marcho),  contains valuable details on CBS, the backend that DISM and SFC use.</li>

<li><strong><a rel="noreferrer noopener" href="https://woshub.com/dism-cleanup-image-restorehealth/" target="_blank">Using DISM to Check and Repair Windows Image</a>:</strong> This article contains valuable details on using DISM and Repair Windows Image.</li>
