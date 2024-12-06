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











