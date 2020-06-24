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
* Capture all network traffic to and from computer
```
#Start the trace
netsh trace start capture=yes tracefile=c:\perflog\trace.etl fileMode=single maxSize=500
#duplicate issue
netsh trace stop
```
* Capture traffic between local computer and destination computer with IP
```
netsh trace start capture=yes Ethernet.Type=IPv4 IPv4.Address=10.10.10.10 tracefile=c:\perflog\trace.etl
#duplicate issue
netsh trace stop
```
[Netsh Commands for Network Trace in Windows Server 2008 R2 and Windows 7](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd878517(v=ws.10)?redirectedfrom=MSDN)   

## Using Powershell
* Capture all TCP/IP network traffic
```
#get-command *netevent*
#get-command -module NetEventPacketCapture 
#set up network trace
New-NetEventSession -Name "Session" -LocalFilePath c:\perflog\capture.etl
Add-NetEventProvider -Name "Microsoft-Windows-TCPIP" -SessionName "Session"
#start trace
Start-NetEventSession -Name "Session"
#stop trace
Stop-NetEventSession -name "session"
Remove-NetEventSession -Name "Session"
```
* Capture between local computer and a destination computer
```
New-NetEventSession -Name "Session" -LocalFilePath c:\perflog\capture.etl
Add-NetEventPacketCaptureProvider -SessionName "Session" -Level 4 -CaptureType Physical -EtherType 0x0800 -IPAddresses 10.10.10.10
Start-NetEventSession -Name "Session"
#duplicate issue
Stop-NetEventSession -name "Session"
Remove-NetEventSession -Name "Session"
```
* Review data
```
#use get-winevent to read network trace
$log = get-winevent -Path C:\perflog\capture.etl -Oldest
$log.message 
```
[Use PowerShell to Parse Network Trace Logs](https://devblogs.microsoft.com/scripting/use-powershell-to-parse-network-trace-logs/)  
[Packet Sniffing with PowerShell: Getting Started](https://devblogs.microsoft.com/scripting/packet-sniffing-with-powershell-getting-started/)  
