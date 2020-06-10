## With PowerShell

```
get-command -noun counter
get-help get-counter
```

* Shows the base system counters
```
Get-counter
```
* Gets list of objects available
```
Get-counter –listset * | more
```
* Cleaner list of counters
```
Get-Counter -ListSet * | Format-Wide CounterSetName | more
```
* Gets list of available counter under a object
```
Get-Counter -ListSet Memory | select -expandproperty Paths
```
* To get information on a single counter
```
Get-Counter -Counter "\physicaldisk(_total)\current disk queue length“
```
* To get 5 collections of a counter
```
Get-Counter -Counter "\physicaldisk(_total)\current disk queue length" -MaxSamples 5 
```
* To get a continuous collection of a counter
```
Get-Counter -Continuous -Counter "\physicaldisk(_total)\current disk queue length" 
```
* View Counters in a clearer format using calculated properties
```
get-counter  | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}},@{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object
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
get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object | more
```
* Ouput to flat file
```
get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object | out-file .\DC_Perf.txt
```
* Export to csv
```
get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | export-csv ".\DC_Perf.csv" -notypeinformation
 ```
 * Create a blg getting sample every 15 seconds, with a max of 40 samples
 ```
 get-counter -Counter $counters -PipelineVariable perfcounter -SampleInterval 15 -MaxSamples 40 | `
    Export-Counter -Path ".\dc_perf_collection.blg"
 ```
 * Create a blg getting sample every 15 seconds, with a max file size of 100 mb, circular
 ```
 get-counter -Counter $counters -PipelineVariable perfcounter -SampleInterval 15  | Export-Counter -Path ".\dc_perf_collection.blg" -maxsize (100mb) -circular
 ```
## View ADFS / WAP Related Counters
```
$counters = "\AD FS\*","\AD FS Proxy\*","\Memory\*","\PhysicalDisk(*)\*","\Process(*)\*","\Processor(*)\*","\TCPv4\*"
get-counter -Counter $counters | select -ExpandProperty CounterSamples | where cookedvalue -gt 0 | select `
    @{n= "Object";e={($_.path -split("\\"))[3]}}, @{n= "Counter";e={($_.path -split("\\"))[4]}}, `
    instancename, CookedValue | format-table -AutoSize -wrap -GroupBy object | more
 ```
