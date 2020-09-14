# Day 1

## Environment Setup

The first thing any good SysAdmin / Programmer does is setup their development environment.

- First we are going to make sure that we know how to start and run _PowerShell_

- Press the ++windows++ key, then type `PowerShell`, then hit ++enter++, this opens a new **non-elevated** _PowerShell_ instance

- <small> You may see there are some versions of PowerShell and PowerShell ISE ending with `(x86)`, these are 32-bit versions of PowerShell </small>

- If you open a new PowerShell window, but this time right-click and select `Run as Administrator`, you will open a new PowerShell window with elevated permissions

- <small> Notice the prompt ends in `system32`, this is a good indicator that the current window was launched as Admin</small>

- Windows comes with a simple PowerShell editor, called PowerShell ISE (Integrated Scripting Environment), open `Windows PowerShell ISE`

!!! info "ISE Goodness"

    The ISE is a great tool to use. Shipped with every install of PowerShell, it includes syntax highlighting, intellisense, debugging, command search and so much more.

- A better IDE is [VSCode](https://code.visualstudio.com/) with the PowerShell extension. Let me know if you want me to walk you through this install.

---

### Getting used to using the PowerShell CLI

#### Command Aliases

If you come from a Linux / Mac background, the PowerShell CLI will be very familiar. Many Bash commands are also **aliased** to their PowerShell counterpart ex.

=== "Bash"

        #!bash
        cd # change directory
        pwd # print working directory
        ls # list

=== "PowerShell"

        #!powershell
        Set-Location
        Get-Locaton
        Get-ChildItem

=== "View Aliases"

        #!powershell
        Get-Alias # To view all alias'
        Get-Alias "command" # To view the alias of a command, ex. Get-Alias rmdir

#### Getting Help

Like every other shell, PowerShell has an integrated helpfile system.

=== "Searching by Name"

        #!powershell
        Get-Help *file*

=== "Retrieving by Name"

        #!powershell
        Get-Help Get-FileHash

=== "With Examples"

        #!powershell
        Get-Help Get-FileHash -Examples

#### Navigating the File System

The first thing you should master using any CLI is how to navigate directories. This transfers over to every other shell seamlessly. The only difference is that Windows uses `\` during navigation and Linux/Mac use `/` during navigation.

First, navigate to your Window partition's root folder.

??? tldr "Hint"

    You will have to use a combination of `cd`/`Set-Location` and `ls`/`Get-ChildItem`

Your prompt should now look like this: `PS C:\> `

Navigate back to your user folder, so that your prompt returns back to `PS C:\Users\Username>`

Now navigate to your **Desktop**, -> `PS C:\Users\Username\Desktop>`

View the contents of your desktop using `ls` or `Get-ChildItem`

Finally navigate back to your home folder using `cd ..`

> In PowerShell as well as Bash, `.` when utilized in a path argument refers to the current directory, and `..` refers to the current
> parent directory.

??? info "Fastest Route"

    To accomplish the above using the least amount of typing, the commands are:

        #!powershell
        cd \
        cd $home
        cd .\Desktop
        cd ..

    The second command uses one of the preset PowerShell variables, `$home` which returns the home directory of the current logged in user.
