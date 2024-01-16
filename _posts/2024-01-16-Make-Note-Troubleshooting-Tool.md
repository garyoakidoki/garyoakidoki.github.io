---
title: My First Blog Post Entry
date: 2024-01-16 17:50:00 +0800
categories: [Blogging]
tags: [WindowsOS]     # TAG names should always be lowercase
math: true
comments: true
youtubeId: 
---

### What every Windows user must know about DISM and SFC
This blog post is NOT original! The SOURCE and ALL CREDITS GO TO HERE: https://confidentialfiles.wordpress.com/2023/08/10/what-every-windows-user-must-know-about-dism-and-sfc/

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
    DISM.exe /Online /Cleanup-Image /RetoreHealth
    SFC.exe /ScanNow

### Mistakes to avoid

    Do not change the order of the commands. The correct order, as mentioned above, is the disk checkup, DISM, and SFC.
    If one command returned an error message, do not proceed to the next. These error messages are indicators of the problems you need to solve.
    Do not run anything else while you’re running a disk checkup, DISM, or SFC. Close as many apps, processes, and tasks as possible beforehand.
    Do not run DISM with /StartComponentCleanup, /CheckHealth, or /ScanHealth. If you don’t know what they are doing, you’re not the right person to run them.
    Do not run chkdsk c: /f. This command is obsolete.

### Limitations

As explained above, DISM and SFC have narrow uses. They repair the component store and legacy components respectively. More specifically, DISM and SFC cannot repair the following:

    Microsoft Store apps bundled with Windows: To fix these, try resetting them via the Settings app or the Appx module of PowerShell.
    Registry: The only surefire way of fixing the Windows Registry is through prevention via backup.
    Defender Antivirus: Short of reinstalling Windows, there is no way to fix this item because it breaks easily and resists repair. To add insult to injury, most people, including those who work for Microsoft, don’t know the difference between “Defender Antivirus” and “Windows Security.” (Yes, they are distinct. On Windows Server, you can install them separately.)
    Edge: To fix this item, download a fresh copy from Microsoft’s website. However, it might resist repair because Microsoft is actively looking for ways to prevent users from removing it.
    OneDrive: To fix this item, just reinstall it. You can grab a fresh copy from Microsoft’s website.

### DISM’s log

DISM creates a log at C:\Windows\Logs\DISM\dism.log. You can change the log’s location and level of details, but don’t bother. It contains nothing of value. It consists of 113 entries that DISM inserts at the moment of execution and 48 others at termination. DISM logs nothing for the entire length of its operation, which takes 10 to 20 minutes.

DISM and SFC are frontends for Component-Based Servicing (CBS), which keeps a log at C:\Windows\Logs\CBS\CBS.log, where you can find the details of changes that DISM and SFC have made to your system.

### Further reading

    OS integrity diagnosis cheat sheet: This short Microsoft Support article sums up this article in 4 bullet points, telling you how to run DISM and SFC in correct order. This is your cheat sheet.
    Fix Windows Update errors by using the DISM or System Update Readiness tool: This Microsoft Learn article has in-depth info about using DISM (and its Windows 7 counterpart, SUR) to fix corrupt or missing OS files.
    Use the System File Checker tool to repair missing or corrupted system files: This Microsoft Support article has in-depth info on the second step of fixing corrupt or missing OS files, which SFC. It even teaches you how to automate extracting repair details from the CBS.log file.
    Understanding Component-Based Servicing: This TechNet article, written by CC Hameed (not Craig Marcho), contains valuable details on CBS, the backend that DISM and SFC use.
