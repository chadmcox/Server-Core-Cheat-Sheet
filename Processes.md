# Working with Processes using PowerShell

* List all Processes
```
get-process | more

get-process | select * | more
```
* Display a single process
```
get-process -name lsass
```
* View the user using a process
```
Get-Process -name notepad -IncludeUserName
Get-Process -name * -IncludeUserName | more
```
* Start a process
```
start-process -filepath "notepad.exe"
```
* Stop a process
```
stop-process -name notepad
stop-process -id 123
```
# Using Win32_Process WMI/CIM Class
```
Get-CimInstance -classname Win32_Process | more
```
* Get Process owner using method
```
$proc = Get-CimInstance Win32_Process -Filter "name = 'notepad.exe'"
Invoke-CimMethod -InputObject $proc -MethodName GetOwner
```
# Using Windows CMDs
* list all task
```
tasklist | more
```
* List process with additional information like user
```
tasklist /v | more
```
* List processes with service information
```
tasklist /svc | more
```
* Stop a process
```
taskkill /F /PID 1242
taskkill /IM "notepad.exe" /F
```

# Dump Process using Task Manager
```
taskmgr
```
* In Process tab locate process to dump
* Right clich and select create dump file
# Dump Process using Proc Dump
* Requires [ProcDump from Sysinternals](https://docs.microsoft.com/en-us/sysinternals/downloads/procdump)
```
procdump lsass
```
