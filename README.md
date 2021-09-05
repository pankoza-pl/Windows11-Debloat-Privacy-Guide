# Windows 11 Debloat / Optimization / Privacy Guide

## IMPORTANT

This guide is meant for advanced users who wants to get rid off Windows 11's bloatware and telemetry, if you have no experience of such thing then you can consider this guide for ease. <br>
<br>
**Note : You're doing this at your own risk, I am not responsible for any data loss or damage that may occur.**
<br>
Last tested on Windows 11 Insider Build : 22000.176 / 22449.1000

## Credits 

This guide is based on Adolf Intel's Windows 10 Privacy Guide with many modifications to make it working on Windows 11 <br>
Thanks to PPGSource 502#3112 from my discord server for stripping Windows 11 to barebones <br>
Made by The World Of PC#0077 <br>

### Pros

Get rid of bloatware <br>
Disable most of the telemetry <br>
Gain performance <br>
Optimize Windows 11 for gaming as well as productivity <br>
Strip Windows 11 to barebones (In Advanced removal below) <br>

### Cons

Breaks Sysprep <br>
Won't be able to use sfc/scannow command

## Pre-Requisite

NTFS Access <br>
Install_Wim_Tweak.exe <br>
DISM++ (Optional but recommended) <br>
WinAeroTweaker <br>
Linux Live or Dualboot (if you want to strip to barebone)

## Debloating Windows 11 (Just The Bloatwares)

### Before you debloat!
At the end of the setup process, create a local account, don't use Cortana and turn off everything in the privacy settings. <br>

![Screenshot (01)](https://user-images.githubusercontent.com/85176292/132122504-1412f80f-2bac-4671-93f0-fa5204082b59.png)
![Screenshot (02)](https://user-images.githubusercontent.com/85176292/132122505-95823c80-06cc-4037-a48a-7e4a2e0a904a.png)

**Make sure you are doing this on a temporary user account because you'll be deleting this later on** <br>
Copy and paste the "install_wim_tweak.exe" to C:\Windows\System32 <br>

![Screenshot (03)](https://user-images.githubusercontent.com/85176292/132123362-f68c5829-c739-4628-94be-7ca2dc27fb54.png)

Before debloating if you have recently updated your copy of Windows 11 or just fresh installed it, I would recommend you to cleanup the component store with /resetbase command or use DISM++ for ease, it clears the temp files with update leftovers in WinSxS. <br>

![Screenshot (04)](https://user-images.githubusercontent.com/85176292/132123367-6e2ebe05-9f93-4c18-86cf-ffb1f7cc34ea.png)

![Screenshot (05)](https://user-images.githubusercontent.com/85176292/132123387-5c0b6700-0497-4561-a01f-2ba419455c46.png)

**Note : If DISM++ gives error while cleaning up the component store use this command (Command Prompt as Admin Obviously)**

```
DISM /Online /Cleanup-Image /StartComponentCleanup /ResetBase
```
After the cleanup is done you can start debloating Windows 11. <br>
You can debloat using my debloat tool and then continue further optimization <br>

![Screenshot (06)](https://user-images.githubusercontent.com/85176292/132124748-0a480c76-367e-4138-848f-c400b6db98a3.png)

Or you can start from here <br>
### Microsoft Store 
In the PowerShell, type: <br>
```
Get-AppxPackage -AllUsers *store* | Remove-AppxPackage
```
You can ignore any error that pops up.<br>

In Command Prompt, type: <br>
```
install_wim_tweak /o /c Microsoft-Windows-ContentDeliveryManager /r
install_wim_tweak /o /c Microsoft-Windows-Store /r
```

### Removing Services (Not Recommended if you are going to use any UWP app)

In Command Prompt, type: <br>
```
reg add "HKLM\Software\Policies\Microsoft\WindowsStore" /v RemoveWindowsStore /t REG_DWORD /d 1 /f
reg add "HKLM\Software\Policies\Microsoft\WindowsStore" /v DisableStoreApps /t REG_DWORD /d 1 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\AppHost" /v "EnableWebContentEvaluation" /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\PushToInstall" /v DisablePushToInstall /t REG_DWORD /d 1 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SilentInstalledAppsEnabled /t REG_DWORD /d 0 /f
sc delete PushToInstall
```

### Music, TV
In the PowerShell, type: <br>
```
Get-AppxPackage -AllUsers *zune* | Remove-AppxPackage
Get-WindowsPackage -Online | Where PackageName -like *MediaPlayer* | Remove-WindowsPackage -Online -NoRestart
```

### Xbox and Game DVR
In the PowerShell, type: <br>
```
Get-AppxPackage -AllUsers *xbox* | Remove-AppxPackage
```

### Removing Services (Not Recommended if you are going to use it in future)
In Command Prompt, type: <br>
```
sc delete XblAuthManager
sc delete XblGameSave
sc delete XboxNetApiSvc
sc delete XboxGipSvc
reg delete "HKLM\SYSTEM\CurrentControlSet\Services\xbgm" /f
schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTask" /disable
schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTaskLogon" /disable
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR" /v AllowGameDVR /t REG_DWORD /d 0 /f
```

### Sticky Notes
In the PowerShell, type: <br>
```
Get-AppxPackage -AllUsers *sticky* | Remove-AppxPackage
```

### Maps
In the PowerShell, type: <br>
```
Get-AppxPackage -AllUsers *maps* | Remove-AppxPackage
```

### Removing Services
In Command Prompt, type: <br>
```
sc delete MapsBroker
sc delete lfsvc
schtasks /Change /TN "\Microsoft\Windows\Maps\MapsUpdateTask" /disable
schtasks /Change /TN "\Microsoft\Windows\Maps\MapsToastTask" /disable
```

### Alarms and Clock
```
Get-AppxPackage -AllUsers *alarms* | Remove-AppxPackage
Get-AppxPackage -AllUsers *people* | Remove-AppxPackage
```
You can ignore any error that pops up.

### Mail, Calendar, ...
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *comm* | Remove-AppxPackage
Get-AppxPackage -AllUsers *mess* | Remove-AppxPackage
```

### OneNote
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *onenote* | Remove-AppxPackage
```

### Photos
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *photo* | Remove-AppxPackage
```
Enable Classic Photoviewer using [WinAeroTweaker](https://winaero.com/download-winaero-tweaker/)

### Camera
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *camera* | Remove-AppxPackage
````
Ignore any error that pops up

### Weather, News, ...
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *bing* | Remove-AppxPackage
```

### Calculator
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *calc* | Remove-AppxPackage
```
Download Classic Calulator from [Here](https://winaero.com/get-calculator-from-windows-8-and-windows-7-in-windows-10/)

### Sound Recorder
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *soundrec* | Remove-AppxPackage
```
Alternatives [Audacity](http://www.audacityteam.org/)

### Microsoft Edge (Chromium)

![Screenshot (07)](https://user-images.githubusercontent.com/85176292/132125057-ab8b2dbb-bb0a-4dc3-88c2-418f683e5332.png)

Go to `C:\Program Files (x86)\Microsoft\Edge\Application\%your_version%\Installer\` <br>
Now open powershell as Administrator and type: <br>
```
.\setup.exe -uninstall -system-level -verbose-logging -force-uninstall
```
Microsoft Edge is now uninstalled but you still can see a broken icon on start menu to get rid off it open command prompt and type: <br>

![Screenshot (08)](https://user-images.githubusercontent.com/85176292/132125728-0bca64ec-243b-4d22-865a-2f17ac82d478.png)

```
install_wim_tweak.exe /o /l
install_wim_tweak.exe /o /c "Microsoft-Windows-Internet-Browser-Package" /r
install_wim_tweak.exe /h /o /l
```
Restart is required after this (you can restart later when you are done debloating everything)

### Contact Support, Get Help
In the command prompt, type:
```
install_wim_tweak /o /c Microsoft-Windows-ContactSupport /r
```

In Powershell, type:
```
Get-AppxPackage -AllUsers *GetHelp* | Remove-AppxPackage
```

### Microsoft Quick Assist
In the PowerShell, type:
```
Get-WindowsPackage -Online | Where PackageName -like *QuickAssist* | Remove-WindowsPackage -Online -NoRestart
```

### Connect
In the command prompt, type:
```
install_wim_tweak /o /c Microsoft-PPIProjection-Package /r
```

### Your Phone
In the PowerShell, type:
```
Get-AppxPackage -AllUsers *phone* | Remove-AppxPackage
```

### Hello Face
In the PowerShell, type:
```
Get-WindowsPackage -Online | Where PackageName -like *Hello-Face* | Remove-WindowsPackage -Online -NoRestart
```

In the command prompt, type:
```
schtasks /Change /TN "\Microsoft\Windows\HelloFace\FODCleanupTask" /Disable
```

### Windows Defender (Breaks Windows Updates)
In the command prompt, type:
```
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SmartScreenEnabled /t REG_SZ /d "Off" /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\AppHost" /v "EnableWebContentEvaluation" /t REG_DWORD /d "0" /f
reg add "HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Storage\microsoft.microsoftedge_8wekyb3d8bbwe\MicrosoftEdge\PhishingFilter" /v "EnabledV9" /t REG_DWORD /d "0" /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender" /v DisableAntiSpyware /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\Spynet" /v SpyNetReporting /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\Spynet" /v SubmitSamplesConsent /t REG_DWORD /d 2 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\Spynet" /v DontReportInfectionInformation /t REG_DWORD /d 1 /f
reg delete "HKLM\SYSTEM\CurrentControlSet\Services\Sense" /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\MRT" /v "DontReportInfectionInformation" /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\MRT" /v "DontOfferThroughWUAU" /t REG_DWORD /d 1 /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "SecurityHealth" /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run" /v "SecurityHealth" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\SecHealthUI.exe" /v Debugger /t REG_SZ /d "%windir%\System32\taskkill.exe" /f
install_wim_tweak /o /c Windows-Defender /r
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Notifications\Settings\Windows.SystemToast.SecurityAndMaintenance" /v "Enabled" /t REG_DWORD /d 0 /f
reg delete "HKLM\SYSTEM\CurrentControlSet\Services\SecurityHealthService" /f
```
**Restart your PC** after that use NTFS Access and take ownership of C:\Program Files\WindowsApps\ & C:\ProgramData\Microsoft

![Screenshot (09)](https://user-images.githubusercontent.com/85176292/132126349-d91c4b65-f3c4-412e-a0c9-bba4c039ac30.png)

In WindowsApps delete the SecHealthUI folder

![Screenshot (10)](https://user-images.githubusercontent.com/85176292/132126362-c47be7df-d62f-4212-bd07-97714fd47041.png)

In ProgramData\Microsoft delete every folder related to Windows Defender

![Screenshot (11)](https://user-images.githubusercontent.com/85176292/132126653-1cbec29b-4c31-49f0-b596-b230913f4f30.png)

### Windows Defender (Keeping Windows Updates)

Just take the ownership of C:\Program Files\WindowsApps\ and C:\ProgramData\Microsoft <br>
Then delete the SecHealthUI folder insider WindowsApps and every folder related to Windows Defender inside ProgramData <br>
Now disable Windows Defender through WinAeroTweaker

### FINALIZING

Now since you have removed all the bloatware let's just finally delete the leftovers from C:\Program Files\WindowsApps <br>
Take the ownership as we did above <br>
Now delete every folder except these...

## You Have Successfully Debloated Windows 11!

## Basic Tweaking

### Removing Options from Settings Apps
Now since you have removed the bloatware it is recommended to remove the options related to them from the settings apps <br>
Open Regedit and go to `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer` <br>
Create new string named 'SettingsPageVisibility' <br>
now type 
```
hide:cortana;crossdevice;easeofaccess-speechrecognition;holographic-audio;mobile-devices;privacy-automaticfiledownloads;privacy-feedback;recovery;remotedesktop;speech;sync;sync;easeofaccess-closedcaptioning;easeofaccess-highcontrast;easeofaccess-keyboard;easeofaccess-magnifier;easeofaccess-mouse;easeofaccess-narrator;easeofaccess-otheroptions;privacy-location;backup;findmydevice;quiethours;tabletmode
```

TIP : Add `;windowsdefender` at the end of the string value if you have removed Windows Defender as well (doesn't matter if u kept updates or not)

### Edit with 3D Paint / 3D Print
It is now possible to remove 3D Paint and 3D Print, but they forgot to remove the option in the context menu when you remove them. To remove it, run this in the command prompt:
```
for /f "tokens=1* delims=" %I in (' reg query "HKEY_CLASSES_ROOT\SystemFileAssociations" /s /k /f "3D Edit" ^| find /i "3D Edit" ') do (reg delete "%I" /f )
for /f "tokens=1* delims=" %I in (' reg query "HKEY_CLASSES_ROOT\SystemFileAssociations" /s /k /f "3D Print" ^| find /i "3D Print" ') do (reg delete "%I" /f )
```
### Disabling Cortana
Open our command prompt again and use this command:
```
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 0 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules"  /v "{2765E0F4-2918-4A46-B9C9-43CDD8FCBA2B}" /t REG_SZ /d  "BlockCortana|Action=Block|Active=TRUE|Dir=Out|App=C:\windows\systemapps\microsoft.windows.cortana_cw5n1h2txyewy\searchui.exe|Name=Search  and Cortana  application|AppPkgId=S-1-15-2-1861897761-1695161497-2927542615-642690995-327840285-2659745135-2630312742|" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Search" /v BingSearchEnabled /t REG_DWORD /d 0 /f
```

### Turn off Windows Error reporting
In the command prompt, type:
```
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Error Reporting" /v Disabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting" /v Disabled /t REG_DWORD /d 1 /f
```

### Windows Updates (Keeping Store Unaffected)
By doing this you will still be able to use Windows Store (Windows Updates service will run in background) without downloading any update <br>
Open Regedit and go to `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer` <br>
Open the string we created earlier and type `;windowsupdate` at the end

### Disabling Windows Updates (Effects Windows Store)
By doing this you will not be able to use Microsoft Store or any other app which requires Windows Updates to be enabled
Open Command Prompt and type:

```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wuauserv" /v Start /t REG_DWORD /d 4 /f
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\UsoSvc" /v Start /t REG_DWORD /d 4 /f
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DoSvc" /v Start /t REG_DWORD /d 4 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoUpdate /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v AUOptions /t REG_DWORD /d 2 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v ScheduledInstallDay /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v ScheduledInstallTime /t REG_DWORD /d 3 /f
```

### Disable sync
It doesn't really affect you if you're not using a Microsoft Account, but it will at least disable the Sync settings from the Settings app.
In the command prompt, type:
```
reg add "HKLM\Software\Policies\Microsoft\Windows\SettingSync" /v DisableSettingSync /t REG_DWORD /d 2 /f
reg add "HKLM\Software\Policies\Microsoft\Windows\SettingSync" /v DisableSettingSyncUserOverride /t REG_DWORD /d 1 /f
```

### Removing Telemetry and other unnecessary services
In the command prompt type the following commands:
```
sc delete DiagTrack
sc delete dmwappushservice
sc delete WerSvc
sc delete OneSyncSvc
sc delete MessagingService
sc delete wercplsupport
sc delete PcaSvc
sc config wlidsvc start=demand
sc delete wisvc
sc delete RetailDemo
sc delete diagsvc
sc delete shpamsvc 
sc delete TermService
sc delete UmRdpService
sc delete SessionEnv
sc delete TroubleshootingSvc
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "wscsvc" ^| find /i "wscsvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "OneSyncSvc" ^| find /i "OneSyncSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "MessagingService" ^| find /i "MessagingService"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "PimIndexMaintenanceSvc" ^| find /i "PimIndexMaintenanceSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "UserDataSvc" ^| find /i "UserDataSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "UnistoreSvc" ^| find /i "UnistoreSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "BcastDVRUserService" ^| find /i "BcastDVRUserService"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "Sgrmbroker" ^| find /i "Sgrmbroker"') do (reg delete %I /f)
sc delete diagnosticshub.standardcollector.service
reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Siuf\Rules" /v "NumberOfSIUFInPeriod" /t REG_DWORD /d 0 /f
reg delete "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Siuf\Rules" /v "PeriodInNanoSeconds" /f
reg add "HKLM\SYSTEM\ControlSet001\Control\WMI\AutoLogger\AutoLogger-Diagtrack-Listener" /v Start /t REG_DWORD /d 0 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v AITEnable /t REG_DWORD /d 0 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v DisableInventory /t REG_DWORD /d 1 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v DisablePCA /t REG_DWORD /d 1 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v DisableUAR /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\MicrosoftEdge\PhishingFilter" /v "EnabledV9" /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\System" /v "EnableSmartScreen" /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Internet Explorer\PhishingFilter" /v "EnabledV9" /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v "NoRecentDocsHistory" /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\CompatTelRunner.exe" /v Debugger /t REG_SZ /d "%windir%\System32\taskkill.exe" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\DeviceCensus.exe" /v Debugger /t REG_SZ /d "%windir%\System32\taskkill.exe" /f
```

### Scheduled tasks
In command prompt type:
```
schtasks /Change /TN "Microsoft\Windows\AppID\SmartScreenSpecific" /disable
schtasks /Change /TN "Microsoft\Windows\Application Experience\AitAgent" /disable
schtasks /Change /TN "Microsoft\Windows\Application Experience\Microsoft Compatibility Appraiser" /disable
schtasks /Change /TN "Microsoft\Windows\Application Experience\ProgramDataUpdater" /disable
schtasks /Change /TN "Microsoft\Windows\Application Experience\StartupAppTask" /disable
schtasks /Change /TN "Microsoft\Windows\Autochk\Proxy" /disable
schtasks /Change /TN "Microsoft\Windows\CloudExperienceHost\CreateObjectTask" /disable
schtasks /Change /TN "Microsoft\Windows\Customer Experience Improvement Program\BthSQM" /disable
schtasks /Change /TN "Microsoft\Windows\Customer Experience Improvement Program\Consolidator" /disable
schtasks /Change /TN "Microsoft\Windows\Customer Experience Improvement Program\KernelCeipTask" /disable
schtasks /Change /TN "Microsoft\Windows\Customer Experience Improvement Program\Uploader" /disable
schtasks /Change /TN "Microsoft\Windows\Customer Experience Improvement Program\UsbCeip" /disable
schtasks /Change /TN "Microsoft\Windows\DiskDiagnostic\Microsoft-Windows-DiskDiagnosticDataCollector" /disable
schtasks /Change /TN "Microsoft\Windows\DiskFootprint\Diagnostics" /disable
schtasks /Change /TN "Microsoft\Windows\FileHistory\File History (maintenance mode)" /disable
schtasks /Change /TN "Microsoft\Windows\Maintenance\WinSAT" /disable
schtasks /Change /TN "Microsoft\Windows\PI\Sqm-Tasks" /disable
schtasks /Change /TN "Microsoft\Windows\Power Efficiency Diagnostics\AnalyzeSystem" /disable
schtasks /Change /TN "Microsoft\Windows\Shell\FamilySafetyMonitor" /disable
schtasks /Change /TN "Microsoft\Windows\Shell\FamilySafetyRefresh" /disable
schtasks /Change /TN "Microsoft\Windows\Shell\FamilySafetyUpload" /disable
schtasks /Change /TN "Microsoft\Windows\Windows Error Reporting\QueueReporting" /disable
schtasks /Change /TN "Microsoft\Windows\WindowsUpdate\Automatic App Update" /disable
schtasks /Change /TN "Microsoft\Windows\License Manager\TempSignedLicenseExchange" /disable
schtasks /Change /TN "Microsoft\Windows\Clip\License Validation" /disable
schtasks /Change /TN "\Microsoft\Windows\ApplicationData\DsSvcCleanup" /disable
schtasks /Change /TN "\Microsoft\Windows\Power Efficiency Diagnostics\AnalyzeSystem" /disable
schtasks /Change /TN "\Microsoft\Windows\PushToInstall\LoginCheck" /disable
schtasks /Change /TN "\Microsoft\Windows\PushToInstall\Registration" /disable
schtasks /Change /TN "\Microsoft\Windows\Shell\FamilySafetyMonitor" /disable
schtasks /Change /TN "\Microsoft\Windows\Shell\FamilySafetyMonitorToastTask" /disable
schtasks /Change /TN "\Microsoft\Windows\Shell\FamilySafetyRefreshTask" /disable
schtasks /Change /TN "\Microsoft\Windows\Subscription\EnableLicenseAcquisition" /disable
schtasks /Change /TN "\Microsoft\Windows\Subscription\LicenseAcquisition" /disable
schtasks /Change /TN "\Microsoft\Windows\Diagnosis\RecommendedTroubleshootingScanner" /disable
schtasks /Change /TN "\Microsoft\Windows\Diagnosis\Scheduled" /disable
schtasks /Change /TN "\Microsoft\Windows\NetTrace\GatherNetworkInfo" /disable
del /F /Q "C:\Windows\System32\Tasks\Microsoft\Windows\SettingSync\*" 
```

## Congratulations! Your copy of Windows is now Debotnetted!
Things will change in the future, and I'll do what I can to keep this guide updated. As of August 2021, this guide works on Windows 11 (22000.160)

## Can Windows revert these changes?
When a major update is installed, almost all changes will be reverted and you'll have to repeat this procedure. Major updates come out about twice a year.

[!["Buy Me A Coffee"](https://cdn.discordapp.com/attachments/837916532003962910/880500524934369352/BMC.png)](https://www.buymeacoffee.com/TheWorldOfPC)
