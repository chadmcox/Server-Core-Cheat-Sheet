# Using PowerShell
[Registry provider](https://docs.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Core/About/about_Registry_Provider?view=powershell-7)

* Read a registry key
```
Get-Item -Path HKLM:\SYSTEM\CurrentControlSet\Services\Spooler
```
* Read a registry value
```
Get-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Spooler
```
* Read all subkeys and values
```

```
* Export Registry Keys and Values
```

```
* Import registry keys and values
```

```
* Create new Key
```

```
* Create new value
```

```
* Change Value
```

```
* Delete Value
```

```
* Delete Key
```

```


# Using Legacy Commands

[REG Command](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/reg)

* Read a registry key
```
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa
```
* Read a registry value
```
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa /v restrictanonymoussam
```
* Read all subkeys and values
```
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa  /s
```
* Export Registry Keys and Values
```

```
* Import registry keys and values
```

```
* Create new Key
```

```
* Create new value
```

```
* Change Value
```

```
* Delete Value
```

```
* Delete Key
```

```
