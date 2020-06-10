# There are a few ways to map a network drive

## Using PSDrive in PowerShell
```
get-help about_providers
```

* No seperate credentials needed
```
New-PSDrive -Name "X" -PSProvider FileSystem -Root $network_path
```
* with seperate credentials
```
$cred = get-credential
New-PSDrive -Name "X" -PSProvider FileSystem -Root $network_path -credential $cred
```
* Disconnect drive
```
remove-psdrive x:
```
## Using Net Use CMD
#https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg651155(v=ws.11)

* Using current logged on cred
```
net use x: \\server\share
```
* using seperate credential
```
net use x: \\server\share /user:contoso\user
```
* To disconnect share
```
net use x: /delete
```
