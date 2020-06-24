# Network Trace

*ETL traces can be read using netmon.  There are also ways to covert etl files into other formats*

## Using PKTMON (This works in 2019 server)
* Help
```
pktmon help
```
* Filter based on IP
```
pktmon filter add MyServerX -i 10.10.10.10
pktmon start --etw
#duplicate issue
pktmon stop
```
* Filter by port
```
#add the ports you want to trace
pktmon filter add myportfilter -p 389
pktmon filter add myportfiltera -p 636
#start monitoring
pktmon start --etw
#duplicate issue
pktmon stop
```
[pktmon](https://ss64.com/nt/pktmon.html)  
[How to use network sniffer](https://www.thewindowsclub.com/network-sniffer-tool-pktmon-exe-in-windows-10/)  

## Using Netsh Trace
```
#Start the trace
netsh trace start scenario=netconnection capture=yes tracefile=c:\perflog\networkcapture.etl
#duplicate issue
netsh trace stop
```
[Netsh Commands for Network Trace in Windows Server 2008 R2 and Windows 7](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd878517(v=ws.10)?redirectedfrom=MSDN)   

## Using Powershell
```
#get-command *netevent*
#get-command -module NetEventPacketCapture 
#set up network trace
New-NetEventSession -Name "Session2" -LocalFilePath c:\data\capture.etl
Add-NetEventProvider -Name "Microsoft-Windows-TCPIP" -SessionName "Session2”
#start trace
Start-NetEventSession -Name "Session2"
#stop trace
Stop-NetEventSession -name "session2"

#use get-winevent to read network trace
$log = get-winevent -Path C:\data\capture.etl -Oldest
$log.message 
```
[Use PowerShell to Parse Network Trace Logs](https://devblogs.microsoft.com/scripting/use-powershell-to-parse-network-trace-logs/)  
[Packet Sniffing with PowerShell: Getting Started](https://devblogs.microsoft.com/scripting/packet-sniffing-with-powershell-getting-started/)  
