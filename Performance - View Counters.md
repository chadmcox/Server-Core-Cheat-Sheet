## With PowerShell

```
get-command -noun counter
get-help get-counter
```

* Shows the base system counters
```
Get-counter
```
* Gets list of performance objects available
```
Get-counter –listset * | more
```
* Cleaner list of counter objects
```
Get-Counter -ListSet * | Format-Wide CounterSetName | more
```
* Gets list of available counters under an object
```
Get-Counter -ListSet Memory | select -expandproperty Paths
```
* To get value on a single counter
```
Get-Counter -Counter "\physicaldisk(_total)\current disk queue length“
```
* To get values from all counters in an object on a single counter
```
Get-Counter -Counter "\physicaldisk(_total)\*“
```
* To get 5 values of a counter
```
Get-Counter -Counter "\physicaldisk(_total)\current disk queue length" -MaxSamples 5 
```
* To get values from two seperate counters
```
Get-Counter -Counter "\physicaldisk(_total)\current disk queue length","\Memory\Available MBytes"
```
* To get a continuous values from a counter
```
Get-Counter -Continuous -Counter "\physicaldisk(_total)\current disk queue length" 
```
* View Counters in a cleaner format using calculated properties
```
get-counter  | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}},@{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object
```
* view counters from remote computer
```
Get-Counter -computername Server1,Server2,Server3 -MaxSamples 5
```

# Useful Examples
## View Domain Controller Related Counters
* Store Domain Controller Counter Objects in Array for the cmdlet to use
```
$counters = "\Netlogon(_Total)\*","\Security System-Wide Statistics\NTLM Authentications", `
  "\Security System-Wide Statistics\Kerberos Authentications","\DirectoryServices(*)\*","\Database(lsass)\*", `
  "\NTDS\*","\Memory\*","\PhysicalDisk(*)\*","\Processor(*)\*","\TCPv4\*","\TCPv6\*","\DNS\*"
 ```
* View in console
```
$counters = "\Netlogon(_Total)\*","\Security System-Wide Statistics\NTLM Authentications", `
  "\Security System-Wide Statistics\Kerberos Authentications","\DirectoryServices(*)\*","\Database(lsass)\*", `
  "\NTDS\*","\Memory\*","\PhysicalDisk(*)\*","\Processor(*)\*","\TCPv4\*","\TCPv6\*","\DNS\*"
  
get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object | more
```
* Ouput to flat file
```
$counters = "\Netlogon(_Total)\*","\Security System-Wide Statistics\NTLM Authentications", `
  "\Security System-Wide Statistics\Kerberos Authentications","\DirectoryServices(*)\*","\Database(lsass)\*", `
  "\NTDS\*","\Memory\*","\PhysicalDisk(*)\*","\Processor(*)\*","\TCPv4\*","\TCPv6\*","\DNS\*"
  
get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object | out-file .\DC_Perf.txt
```
* Export to csv
```
$counters = "\Netlogon(_Total)\*","\Security System-Wide Statistics\NTLM Authentications", `
  "\Security System-Wide Statistics\Kerberos Authentications","\DirectoryServices(*)\*","\Database(lsass)\*", `
  "\NTDS\*","\Memory\*","\PhysicalDisk(*)\*","\Processor(*)\*","\TCPv4\*","\TCPv6\*","\DNS\*"
  
get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | export-csv ".\DC_Perf.csv" -notypeinformation
 ```
 
## View ADFS / WAP Related Counters
```
$counters = "\AD FS\*","\AD FS Proxy\*","\Memory\*","\PhysicalDisk(*)\*","\Process(*)\*","\Processor(*)\*","\TCPv4\*"

get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object | more
 ```
 #Using CMD
* To get value on a single counter, continuous
```
typeperf "\physicaldisk(_total)\current disk queue length“
```
