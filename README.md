# PowerShell
Data and Notes related to PowerShell  

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

How to use a variable value in a string:  
"Hello, $myname"  





