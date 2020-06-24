# Network Trace

* Using PKTMON (This works in 2019 server)

```
#help
pktmon help
#add the ports you want to trace
pktmon filter add -p 389
#start monitoring
pktmon start --etw -p 0
#stop
pktmon stop
```
[pktmon](https://ss64.com/nt/pktmon.html)
[How to use network snigger](https://www.thewindowsclub.com/network-sniffer-tool-pktmon-exe-in-windows-10/)

* Using Netsh Trace





* Using Powershell
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
