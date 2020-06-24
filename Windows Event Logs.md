# Event Logs Using PowerShell

* View the System Log Properties
```
Get-WinEvent -ListLog system
```
* View all logs and their properties
```
Get-WinEvent -ListLog * | more
```
* View log providers and associated logs
```
Get-WinEvent -ListProvider * | more
```
* View all events in a particular log
```
get-winevent -LogName system | more
get-winevent -LogName 'Microsoft-Windows-AAD/Operational' | more
```
* Return the first 15 events from log
```
get-winevent -LogName system -MaxEvents 15
```
* Return Events from a remote computer
```
get-winevent -LogName system -MaxEvents 15 -computername server1,server2
```
# Use Filtering with filterhashtable, RECOMMENDED!!!!
* Retrieve Events logs for Today using filterhashtable
```
Get-WinEvent -FilterHashtable @{logname='system'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
```
* View Events over the last 5 Days
```
Get-WinEvent -FilterHashtable @{logname='system'; StartTime=(Get-Date) - (New-TimeSpan -Day 5)} | ft -AutoSize –Wrap | more
```
*Note: (Get-Date) - (New-TimeSpan -Day 5)  this will grab the date for 5 days ago.*
*Note: or (get-date).adddays(-5)*
* Filter based on Event ID for Today
```
Get-WinEvent -FilterHashtable @{logname='system'; id=1502; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
```
# Domain Controller Event Log Examples
* View Today's Events
```
Get-WinEvent -FilterHashtable @{logname='System'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
Get-WinEvent -FilterHashtable @{logname='Security'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
Get-WinEvent -FilterHashtable @{logname='Application'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
Get-WinEvent -FilterHashtable @{logname='Directory Service'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
Get-WinEvent -FilterHashtable @{logname='DFS Replication'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
Get-WinEvent -FilterHashtable @{logname='DNS Server'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
Get-WinEvent -FilterHashtable @{logname='Active Directory Web Services'; StartTime=(Get-Date).date} | ft -AutoSize –Wrap | more
```
# Clean Export to CSV File
*Problem is the message field doesnt export correctly*
```
Get-WinEvent -FilterHashTable @{LogName='System'; StartTime=(Get-Date).date} -ErrorAction SilentlyContinue | `
    Select-Object Machinename, TimeCreated, ID, UserId,LevelDisplayName,ProviderName,`
    @{n= "Message";e={ ($_.Message -Replace “`r`n|`r|`n”,” ”).Trim() }} | Export-Csv .\system.csv -NoTypeInformation
```


# Event Logs using CMD
* View Events all events from a log
```
wevtutil qe System /format:text | more /c 
wevtutil qe Microsoft-Windows-AAD/Operational /format:text | more
```
* Return the first 15 events from log
```
wevtutil qe System /c:15 /rd:true /f:text | more /c
```
* Return Events from a remote computer (maynot work due to firewall settings on remote computer)
```
wevtutil qe System /format:text /r:server1 | more /c 
```
* Return based on Event ID
```
WEVTUtil qe System /count:20 /rd:true /format:text /q:"Event[System[(EventID=12)]]"
```
