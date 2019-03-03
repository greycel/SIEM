In order to build a lab for Windows logs, a Windows system is required. The content on this page will focus on setting up a victim system with advanced logging. While production systems may not have such high levels of logging, it remains important to understand how attacks and activities can be logged. It may be the case that observations in a lab environment warrant increasing logging on production systems to allow detection.

# Recommend Setup

## Disable Windows Firewall
Windows Security > Firewall & Network Protection > Private Network > Turn Off

## Disable Password Protected Sharing

- Control Panel > Network and Internet > Network and Sharing Center > Advanced Sharing Settings > All Networks > ...
  - Public Folder Sharing > Turn on...
  - Password Protected Sharing > Turn off...



## Sysmon Logging
- Copy Sysmon to virtual host and install as a service
- c:\windows\sysmon\sysmon64.exe -i
- %SystemRoot%\system32\winevt\logs\Microsoft-Windows-Sysmon%4Operational.evtx

## PowerShell Logging
- %SystemRoot%\system32\winevt\logs\Microsoft-Windows-PowerShell%4Operational.evtx

### Increase Log Size
- Event Viewer > Application and Service Logs > Microsoft > Windows > PowerShell > Operational
- Right Click > Properties
- Add at least a 0 to the end of "Maximum Log Size ( KB )"

### Enable ScriptBlock Logging
(Event ID 4104)
- Create they key path: HKLM\SOFTWARE\Wow6432Node\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging
  - Create new DWORD
  - EnableScriptBlockLogging = 1
  - EnableScriptBlockInvocationLogging = 1
- Event ID 4104 will populate the Microsoft-Windows-PowerShell/Operational log

### Enable Module Logging
- Create the key path: HKLM\SOFTWARE\Wow6432Node\Policies\Microsoft\Windows\PowerShell\ModuleLogging
  - Create new DWORD EnableModuleLogging = 1
- Create the key path: HKLM\SOFTWARE\Wow6432Node\Policies\Microsoft\Windows\PowerShell\ModuleLogging\ModuleNames
    - Create new STRING VALUE \* = * (an asterisk for the value and data)
- Event ID 4103 will populate the Microsoft-Windows-PowerShell/Operational log

### Transcription
- Create the key path: HKLM\SOFTWARE\Wow6432Node\Policies\Microsoft\Windows\PowerShell\Transcription\
  - Create new DWORD EnableInvocationHeader = 1
  - Create new DWORD EnableTranscripting = 1
  - Create new STRING VALUE OutputDirectory = <path_to_directory>
- Logs will be stored in .txt files in teh specified directory, using the format `..\YYYYMMDD\PowerShell_transcript.PCNAME.RANDOM.YYYYMMDDHHMMSS.txt`