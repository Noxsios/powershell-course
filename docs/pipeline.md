## The Pipeline

The [**pipeline**](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pipelines?view=powershell-7) is one of the most important concepts in PowerShell.

The docs provide a much better explanation than I can provide, so please read them.

Boiled down, the pipeline simply means taking the output of one command as the input of a sequential command, with both being separated by a "pipe" -> | .

ex.

```powershell
# Simple ping sweep
$ping = New-Object System.Net.NetworkInformation.Ping
1..255 | ForEach-Object { if ($ping.Send("192.168.0.$_", 30).Status -eq "Success") { Write-Host "192.168.0.$_ responded." } }
```

Here I am taking the output of the command `1..255` which normally returns an array containing all integers between the bounds specified as the input of the following command.

`ForEach-Object` takes each one of those numbers and performs a ping to `192.168.0.X` where X equals that integer. I set a timeout of 30ms, because for this range everything should realistically be reached within 1-5ms.

If the ping status return code is "Success" then it will `Write-Host` a string with the IP and a statement saying that it responded.

> In a ForEach-Object the \$\_ represents the current iteration object, ie. for the first iteration it is 1, then the second it is 2, then 3 etc...
