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

