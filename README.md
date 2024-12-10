# PowerShell
<b>Data and Notes related to PowerShell</b>   

To know the PowerShell version and details:  
Get-Host  

To retrieve all Powershell commands:  
Get-Command  

To search for a specific command:  
Get-Command -Name Get-Host  

To search command with specific Noun:  
Get-Command -Noun Host  
Ex: To get all commands related to printer management: Get-Help -Noun *printer*

To search command with specific Verb:  
Get-Command -Verb Get  

To see Help:  
Get-Help  

To retrieve all help topics:  
Get-Help *

To see help related to a command:  
Get-Command Get-Host  

Three types of parameters: Optional, Mandatory and Positional Parameters.  

To see list of all aliases (Built-in or user-defined):  
Get-Alias  

To see list of all aliases starting with a particular letter(upper or lower):  
Get-Alias -Name w*  

To see aliases of a particular command:  
Get-Alias -Definition Get-Command  

To set alias for cmdlets:  
New-Alias -Name command Get-command  
Note: We cannot assign same alias to another cmdlet using New-Alias. User-defined alias are not saved after we exit from Powershell.  

To assign an existing alias to another cmdlet:  
Set-Alias -Name commands Get-Host  
Note: If commands alias does not exist, Set-Alias will create the alias and assign it to the cmdlet. We cannot assign an inbuilt alias.  

Creating Variables:  
$myname="Salman"  
${My Name is}="Salman"  
Note: Letter, alphabet, symbols and special characters can be used while crceating a variable but enclose it inside {}.  

To get object type of a variable:  
$myname.GetType()  

To see properties and methods available for a variable:  
$myname | Get-Member  

To assign the output of a cmdlet in a variable:  
$commands=Get-Command  

To use a variable value in a string:  
"Hello, $myname"  

If $a=10 and B="20", then $a+$b=30 and $b+$a=2010 (Order of data type matters)  
variable casting : [int]$b="20"  

Operator precedence in Poweshell(from left to right):  
(), negative number, *, /, %,"+ or -"  

Below are some of commonly used data types in PowerShell:  

[int] 32-bit signed integer  
[long] 64-bit signed integer  
[string] Fixed-length string of characters  
[char] 16-bit character  
[bool] True/false value  
[byte] 8-bit integer  
[double] Double-precision 64-bit floating point number  
[decimal] 128-bit decimal value  
[single] Single-precision 32-bit floating point number  
[array] List of similar values  

Automatic variable in Powershell:  

$HOME - Full path of user's home directory.  
$PSHOME - Evaluates to the full path of PowerShell installation directory.  
$PSVersionTable - Displays details of the PowerShell version running in the current session.  
$NULL - Empty value.  
$FALSE - Contains False.    
$TRUE - Contains True.  
$OFS - Stores a string that we want to use as an output field seperator.  

To read a line of input from console:  
$Name=Read-Host "Please enter name"  

To save input as secure string(asterisk will be displayed instead of input values):  
$Name=Read-Host "Please enter name" -AsSecureString  

To write a message to console:  
Write-Host "My name is $name"  

Extension of PowerShell script:.ps1  
By default, PowerShell execution policy restricts any script to run.By default, execution policy is set to Restricted.  
Restricted: Prevents any script from being run.  
AllSigned: Script will run if signed by trusted publisher.  
RemoteSigned: Script created locally will run but those downloaded from internet will not run unless it is digitally signed by a trusted publisher.  
Unsigned: All scripts will run.  

To check current execution policy:  
Get-ExecutionPolicy  

To change the execution policy(user requires administrative privilege):  
Set-ExecutionPolicy Unrestricted  

Variable scopes in PowerShell:  
Global scope: Default scope for PowerShell objects. Can be accessed by all objects in current PowerShell session.  
Script scope: Variable defined inside script can be accessed only from within the scripts.  
Private scope: Can be accessed within current scope.  
Numbered scope: Used to refer the variables with the same name in different scopes. Variables are identified by referring it using a name or a number.  

Get-Service - Display all the services on a local or remote computer.  
Table contents of Get-Service output : Status, Service name and Display name.  
Usage: Get-Service -Name DHCP  
       Get-Service -Name "*net*"  
       Get-Service -DisplayName "*net*"  

Where-Object: Used to limit or filter the object passed tothe pipeline.  
Usage: Get-Service | Where-Object {$_.Status -eq "Running"}  
Note: $_. is the current instance of the object being evaluated.  
Use Cases:  
Get-Service | Where-Object {$_.Name -Like "n*"} - This will display service whose name starts with n(Any case).  
Get-Service | Where-Object {$_.Name -cLike "N*"} - Same as above but will perform case sensitive operation.  
Get-Service | Where-Object {$_.Status -eq "Running" -and $_.Name -like "A*"} - and or or operators can be used also.  
-lt, -le, -gt, -ge, -eq, -ne, -like, -clike (Operators for filtering)  

Select-Object - Filters specific properties of object based on the parameter specified:  
Get-Service | Select-Object -Property Name,Status  
Get-service | Select-Object -ExpandProperty -Name,Status (This removes the table headline of output.)  
Get-Service| Select-Object -Head 5  
Get-Service | Select-Object -Last 3  

Out-File (This cmdlet sends output to a file)  
Get-Service | Out-File -FilePath C:\File.txt  
Get-Service > C:\File.txt (Shortcut to send output to file)  
Get-Service >> C:\File.txt (This appends the data)  

Export-Csv (This cmdlet sends output to a file in CSV format)  
Get-Service | Select-Object -Last 10 | Export-Csv -Path C:\File.csv  

Get-Process (To retrieve all running processes in local or remote computer)  
Get-Process -Name powershell (Show details of powershell process only)  
Get-Process -Id 37567 (See details based on process id)  
Get-Process -Name win*,pow* (See details of multiple processes using wildcards and commas)  

Sort-Object - Displays object is sorted order. Ascending by default.  
Get-Process | Sort-Object -Property CPU -Descending  

Measure-Object - This cmdlet performs calculations on property values of objects.  
Get-Process | Measure-Object (Retrieves total no of processes)  
Get-Process | Measure-Object -Property VM -Sum _average -Maximum -Minimum  
Get-Process | Measure-Object -Property VM -Line -Word -Character  

Get-Content (This cmdlet retrieves contents of file from a specified location)  
Get-Content -Path C:\Test.txt  
Import-Csv -Path C:\Text.csv  

Get-EventLog -list (To view all available event logs)  
Get-EventLog -LogName Application (To display specific event log)  
Get-EventLog -LogName Application -EntryType Error (displays the details of those events having errors in the application event log)  

Format-Wide (To retrieve single-item data (such as a process name) and display that data in one or more columns)  
Format-Wide only displays a single property as its -Property parameter only takes a single value.  
Get-EventLog -LogName Application | Format-Wide -Property Source  
Get_eventLog -LogName Application | Format-Wide -Property Source -Column 3  
The –Column parameter can be used to specify the number of columns in which the data has to be displayed.  

Format-List (Displays an object in the form of a listing, with each property labeled and displayed on a separate line)  
The property parameter of Format-List can accept one or more property names.  
Get-EventLog -LogName Application | Format-List  

Format-Table (formats the output in a tabular format)  
The -AutoSize parameter when used with Format-Table cmdlet will calculate column widths based on the actual data to be displayed.  
Get-EventLog -LogName Application | Select -First 10 | Format-Table -Autosize  

Get-ChildItem (to retrieve all the files and folders from any specified directory).  
By default, the Get-ChildItem retrieves the contents of the current working directory if no specific location is specified.  
Get-ChildItem -Path C:\PowerShell  
Get-ChildItem -Path C: -Recurse (-Recurse parameter is used to get objects recursively from the specified directory path)  

New-Item (creates a new file or folder on your computer)  
New-Item -Path C:\ -Name MyData -Type Directory  
New-Item -Path C:\ -Name MyData -Type Directory -Value "This is the content" (-Value can be used to add some data to your new file)  

Copy-Item (cmdlet copies an item from one location to another)  
Copy-Item -Path C:\OldLocation\Test1.txt -Destination C:\NewLocation  

Move-Item (cmdlet moves an item, including its properties, contents, and child items, from one location to another location)  
Move-Item will not overwrite any existing files in the target folder  
Move-Item -Path C:\OldLocation\Test1.txt -Destination C:\NewLocation\Test2.txt (moves a file from one directory to another directory and renames it)  
Move-Item -Path C:\MyData -Destination C:\Temp (move a directory to another directory)  

Remove-Item (It can delete many different types of items, including files, directories, registry keys, variables, aliases, and functions)  
Remove-Item -Path C:\MyData\Test1.txt  
Remove-Item -Path C:\MyData\* (Removes everything inside MyData directory,asks for prompt if its non-empty directory)  

Module (set of related Windows PowerShell functionalities)  
$ENV:PSModulePath environment variable contains a list of the directories in which Windows PowerShell modules are stored.  
System location: %windir%\System32\WindowsPowerShell\v1.0\Modules  
User location: %UserProfile%\Documents\WindowsPowerShell\Modules  
Import-Module: Adds one or more modules to the current PowerShell session.  
Get-Module: Retrieves information about the modules that has been loaded into the current session.  
Remove-Module: Removes the specified module from the current session.  
Import-Module C:\MyModule\Mymodule.psm1 (Module added to current session)   
Get-Module (Retrieve details about the modules that has been loaded in the current PowerShell session)  
Now, function in the module can be loaded directly.  
Remove-Module (Removes module from current session)  










