# Using PowerShell




# Using Legacy Commands

[REG](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/reg)

Read a registry key
```
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa
```
Read a registry value
```
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa /v restrictanonymoussam
```
Read all subkeys and values
```
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa  /s
```
Export Registry Keys and Values
```

```
Import registry keys and values
```

```
Create new Key
```

```
Create new value
```

```
Change Value
```

```
Delete Value
```

```
Delete Key
```

```
