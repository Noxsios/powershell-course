### Get-NetTCPConnection [Docs](https://docs.microsoft.com/en-us/powershell/module/nettcpip/get-nettcpconnection?view=win10-ps)

Functionally similar to `NETSTAT`, but instead of simply returning a string output, this returns an [array](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-7) of objects, which is a million times more powerful.

!!! tldr "What ports is Google Chrome communicating on?"

    Copy this simple one-liner and try it on your local machine.

        #!powershell
        Get-Process -Name "chrome" | ForEach-Object { Get-NetTCPConnection -OwningProcess $_.Id -ErrorAction SilentlyContinue }

---

### Get-Process [Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-process?view=powershell-7)

I used this in the above command to get all processes with the name of "chrome". This command is similar to `tasklist`, but like `Get-NetTCPConnection` returns an array of objects.

=== "All"

        #!powershell
        Get-Process

=== "CPU time > 1000s"

        #!powershell
        Get-Process | Where-Object { $_.CPU -gt 1000 }

=== "All with a Main Window Title"

    > This command shows all processes that have a `.mainWindowTitle` property, a really good indicator they have a visual interface / window.

        #!powershell
        Get-Process | Where-Object {$_.mainWindowTitle} | Format-Table Id, Name, mainWindowtitle -AutoSize

---

### Custom Function - **WhoOwns**

This simple [function](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-7) serves to demonstrate the advantages of using PowerShell cmdlets over Windows Executables or native binaries.

> Goal: I want a simple function where I can specify a TCP port on my local machine and know if any process is communicating on that port, and if so what that process' name and PID is.

=== "Dirty"

        #!powershell
        function WhoOwns($port) {
          try {
            $search_pid = ((NETSTAT.EXE -ano | findstr.exe ":$port`t").split(" "))[-1]
            $service = ((tasklist.exe | findstr.exe $search_pid).split(" "))[0]
          }
          catch {
            $service = "No one"
            $search_pid = "no"
          }
          finally {
            Write-Host "$service owns port $port with $search_pid PID."
          }
        }

    > This _"dirty"_ version uses `NETSTAT` and `tasklist`. While it does accomplish its goal, it is ugly and relies on parsing strings. Such an approach is not _wrong_, but it is not best practice, and does not scale well.

=== "Clean"

        #!powershell
        function WhoOwns($port) {

          $port_pid = (Get-NetTCPConnection -LocalPort $port -ErrorAction SilentlyContinue).OwningProcess

          if ($null -ne $port_pid) {
            $service = (Get-Process -Id $port_pid).ProcessName
            Write-Host "$service owns port $port with $port_pid PID."
          }
          else {
            Write-Host "No one owns port $port."
          }
        }

    > This _"clean"_ version utilizes the native PowerShell cmdlets `Get-NetTCPConnection` and `Get-Process` , and is a lot more scalable and readable.

---

### Test-Path [Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/test-path?view=powershell-7)

> "The Test-Path cmdlet determines whether all elements of the path exist. It returns $True if all elements exist and $False if any are missing. It can also tell whether the path syntax is valid and whether the path leads to a container or a terminal or leaf element. If the Path is whitespace an empty string, then $False is returned. If the Path is $null, array of $null or empty array, a non-terminating error is returned." - [Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/test-path?view=powershell-7)

In even simpler terms, `Test-Path` determines whether the entire path to a file / directory exists, including whether that file / directory exists.

!!! tldr "About Booleans"

    PowerShell is different from other languages in that when you test for a boolean and want to view the result your terminal will return __True__ and __False__, but when using in scripts and commands, you must use `$True` and `$False`.

    ex. `Test-Path $HOME` will return __True__, but if you want to use that in an if statement you must use `Test-Path $HOME -eq $True`

---

### Clear-RecycleBin [Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/clear-recyclebin?view=powershell-7)

Simple command to empty recycle bin. Performs same operation as right clicking recycle bin and selecting ++"Empty Recycle Bin"++

=== "All"

        #!powershell
        Clear-RecycleBin

=== "Specified Drive"

        #!powershell
        Clear-RecycleBin -DriveLetter C

=== "No Confirm"

        #!powershell
        Clear-RecycleBin -Force

---

### Tidbits

- Adding `-Confirm:$false` to the end of a command that may prompt your for confirmation will bypass that prompt.

- Adding the `-WhatIf` flag to commands that accept it will show you a dry run of that command running without changing anything.

- For checking if a value is `$null`, have null as the first argument for the comparison, not the second.

- Add simple functions to your [profile](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7) so that you can access them in every PS session.

- To see all the properties and methods on a specific object or class, pipe the command output to `Get-Member`, ex: `Get-Process | Get-Member`
