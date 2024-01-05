# Windows Periodic Task for File Cleanup

## Overview

This guide provides instructions on how to create a Windows periodic task using Task Scheduler to automatically remove unnecessary files from a specific directory. The process involves creating a PowerShell script that will be scheduled to run at specified intervals and this task will be auto run when you log in to your PC or you can customize it as you want it to begin execution.


#### Targeted Directories
- C:\Windows\Temp
- C:\<PC_USERNAME>\AppData\Local
- C:\Windows\Prefetch

## steps
Create a PowerShell script to remove unnecessary files. Save the script with a .ps1 extension. Adjust the file patterns and target directory as needed.

in this example the file name is `CleanupScript1.ps1`

copy the following code to your `.ps1` file

```PowerShell

# Example PowerShell script to remove unnecessary files
$gtemp = "C:\Windows\Temp\*"
$itemp = "$env:USERPROFILE\AppData\Local\*"
$prefetchdir = "C:\Windows\Prefetch\*"
# Add below any other extensions you want to remove
$filePatterns = @("*.tmp", "*.bak", "*.ico", "*.log", "*.pf")
$filesToRemove = Get-ChildItem -Path $itemp -File -Include $filePatterns
$filesToRemove += Get-ChildItem -Path $prefetchdir -File -Include $filePatterns
$filesToRemove += Get-ChildItem -Path $gtemp -File -Include $filePatterns

foreach ($file in $filesToRemove) {
    try {
        # Attempt to remove the file
        Remove-Item -Path $file.FullName -Force -Recurse -ErrorAction Stop
        Write-Host "Removed file: $($file.FullName)"
    } catch {
        # Handle the error (e.g., file cannot be removed)
        Write-Host "Error removing file: $($file.FullName)"
        Write-Host "Error Message: $($_.Exception.Message)"
    }
}

$userInput = Read-Host "All Done ! Enter any key to exit"

```


### To Execute this task manually 
1) Righ Click on => CleanupScript1.ps1
2) Select Run with `PowerShell`

### To Make it As A Task At Startup
#### 1- Hit `windows button` + `S` and search Task Scheduler
#### 2- Select Create Basic Task

![image](https://github.com/YoussefRashed/C-Drive-Periodic-Cleaner/assets/88270964/ffd6328e-e915-43ab-8662-d07b4b3707f8)
#### 3- Add the name of task and Description

<img width="525" alt="1" src="https://github.com/YoussefRashed/C-Drive-Periodic-Cleaner/assets/88270964/837279a4-e104-4811-8c89-03756cea2b77">

#### 4- Select When I log on

<img width="522" alt="2" src="https://github.com/YoussefRashed/C-Drive-Periodic-Cleaner/assets/88270964/748fe6ea-3a87-4d08-9b74-14d0a441d40a">

#### 5- Upload `.ps1` File 
<img width="523" alt="4" src="https://github.com/YoussefRashed/C-Drive-Periodic-Cleaner/assets/88270964/5430bec0-9790-478f-b7fd-2bbedd331a08">

#### 6- Then Hit Finish

Try to log out then log in again






