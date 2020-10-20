After last week's feedback, the style of this course will be transitioning. Instead of teaching you the PowerShell language, I will be focusing on teaching you certain `functions`, `cmdlets` and utilities.

---

### Get-FileHash [Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7)

We reviewed this cmdlet last week, but here is a usage example.

!!! tldr "Get-FileHash Example"

    - Right click this [File](./assets/test_hash.txt), and select ++"Save Link as..."++ and save to your desktop.

    - Open a new PowerShell window and navigate to your desktop with `#!powershell cd $HOME\Desktop`

    - Calculate the SHA256 hash using `#!powershell Get-FileHash -Path .\test_hash.txt -Algorithm SHA256`

    > As you get better at knowing the order of certain command parameters you can omit the flagged names, ie `-Path` and `-Algorithm`, for within script usage, it is better to leave the statements verbose so that when you or another person return to the code it is clearer what the code is doing.

    - Now open the file in notepad, either by double clicking on the file or using `#!powershell notepad.exe .\test_hash.txt`, then delete the last character (a period) then save and close the file.

    - To test to see if the file hash changed, we can use a simple comparison, I have calculated the before value for you.

            #!powershell
            $oldHash = "914523DA2E60C0AE77E21AD1EEB827F25508A6B6D984AF3E80E8D292D4ADD24B"

            $newHash = Get-FileHash -Path .\test_hash.txt -Algorithm SHA256

            $hashesEqual = $oldHash -eq $newHash.Hash

            if ($hashesEqual) {
                $result = "are"
            } else {
                $result = "are not"
            }

            Write-Output "The files $result equal."

            # Should return : "The files are not the same."

    - Now re-add the period back in at the end of the file, and recalculate the above. The result should now be `The files are equal.`

For hashing explanation - [Expl](https://www.sentinelone.com/blog/what-is-hash-how-does-it-work/)

---

### Test-NetConnection [Docs](https://docs.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=win10-ps)

If a question asks you to check if **Port 80** is open on a remote server. Then `ping` will be of no use to you. This is where `Test-NetConnection` comes into play.

Test-NetConnection allows you to ping a machine and also perform a TCP connection test on a specified port.

=== "Testing Internet Connection"

        #!powershell
        PS C:\Users\UserName> Test-NetConnection

        ComputerName           : internetbeacon.msedge.net
        RemoteAddress          : 13.107.4.52
        InterfaceAlias         : Ethernet
        SourceAddress          : 192.168.0.3
        PingSucceeded          : True
        PingReplyDetails (RTT) : 13 ms

=== "Testing a TCP Port"

        #!powershell
        PS C:\Users\UserName> Test-NetConnection -ComputerName google.com -Port 80

        ComputerName     : google.com
        RemoteAddress    : 172.217.22.14
        RemotePort       : 80
        InterfaceAlias   : Ethernet
        SourceAddress    : 192.168.0.3
        TcpTestSucceeded : True

=== "Testing a TCP Port - Verbose"

        #!powershell
        PS C:\Users\UserName> Test-NetConnection -ComputerName 8.8.8.8 -Port 53 -InformationLevel Detailed

        ComputerName            : 8.8.8.8
        RemoteAddress           : 8.8.8.8
        RemotePort              : 53
        NameResolutionResults   : 8.8.8.8
                                dns.google
        MatchingIPsecRules      :
        NetworkIsolationContext : Internet
        IsAdmin                 : False
        InterfaceAlias          : Ethernet
        SourceAddress           : 192.168.0.3
        NetRoute (NextHop)      : 192.168.0.1
        TcpTestSucceeded        : True

!!! tldr "Simple Port Sweep"

    PowerShell can also perform a basic port sweep if you want to:

    This example uses `192.168.0.8` for my example IP, ==only portscan devices on your local network==, __doing so across an internet service will likely result in your IP becoming banned__.

        #!powershell
        22..80 | % { Test-NetConnection 192.168.0.8 -Port $_ -WarningAction SilentlyContinue | % { Write-Host "Port $($_.RemotePort) : $($_.TcpTestSucceeded)" } }

    > % is an alias of ForEach-Object, within scripts utilize the full version, I am using the alias here for saving space

---

### netstat [Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/netstat)

NETSTAT.EXE is a crossplatform utility that has been built into Windows for a very long time.

This program: _"Displays active TCP connections, ports on which the computer is listening, Ethernet statistics, the IP routing table, IPv4 statistics ..., and IPv6 statistics"_.

In short, `netstat` is the first tool you should turn to when a question asks about an open/listening port on the local machine. This command's usage should transfer over seamlessly to both Mac and Linux.

Some of my favorite combos:

```
#!powershell
netstat -ano

<#
  All TCP and UDP
  No DNS lookup
  PID shown
#>

netstat -r

<#
  Routing table, useful for viewing your default gateway and listing physical interfaces
#>

netstat -o 5

<#
  Display active TCP connections and PIDs, loop every 5s
#>

netstat -a | findstr LISTENING

<#
  List all listening connections / ports.
#>

netstat -ano | findstr 80

<#
  Filter by number
#>

```

---

### Get-Process / tasklist

> Moved to next week...

---
