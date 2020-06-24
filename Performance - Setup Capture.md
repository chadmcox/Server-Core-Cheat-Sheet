# With PowerShell

## View Domain Controller Related Counters

* Create a blg getting sample every 15 seconds, with a max of 40 samples
 ```
 $counters = "\Netlogon(_Total)\*","\Security System-Wide Statistics\NTLM Authentications", `
  "\Security System-Wide Statistics\Kerberos Authentications","\DirectoryServices(*)\*","\Database(lsass)\*", `
  "\NTDS\*","\Memory\*","\PhysicalDisk(*)\*","\Processor(*)\*","\TCPv4\*","\TCPv6\*","\DNS\*"
  
 get-counter -Counter $counters -PipelineVariable perfcounter -SampleInterval 15 -MaxSamples 40 | `
    Export-Counter -Path ".\dc_perf_collection.blg"
 ```
 * Create a blg getting sample every 15 seconds, with a max file size of 100 mb, circular
 ```
 $counters = "\Netlogon(_Total)\*","\Security System-Wide Statistics\NTLM Authentications", `
  "\Security System-Wide Statistics\Kerberos Authentications","\DirectoryServices(*)\*","\Database(lsass)\*", `
  "\NTDS\*","\Memory\*","\PhysicalDisk(*)\*","\Processor(*)\*","\TCPv4\*","\TCPv6\*","\DNS\*"
  
 get-counter -Counter $counters -PipelineVariable perfcounter -SampleInterval 15  | Export-Counter -Path ".\dc_perf_collection.blg" -maxsize (100mb) -circular
 ```
