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

User-created aliases can be removed using the remove-item cmdlet.  
Remove-Item alias:\gpss  

Creating Variables:  
$myname="Salman"  
${My Name is}="Salman"  
Note: Letter, alphabet, symbols and special characters can be used while creating a variable but enclose it inside {}.  

To get object type of a variable:  
$myname.GetType()  

To see properties and methods available for a variable:  
$myname | Get-Member  

To assign the output of a cmdlet in a variable:  
$commands=Get-Command  

To use a variable value in a string:  
"Hello, $myname"  

If $a=10 and $B="20", then $a+$b=30 and $b+$a=2010 (Order of data type matters)  
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

Where-Object: Used to limit or filter the object passed to the pipeline.  
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
Get-Service | Select-Object -First 5  
Get-Service | Select-Object -Last 3  

Out-File (This cmdlet sends output to a file)  
Get-Service | Out-File -FilePath C:\File.txt  
Get-Service > C:\File.txt (Shortcut to send output to file)  
Get-Service >> C:\File.txt (This appends the data)  

Export-Csv (This cmdlet sends output to a file in CSV format)  
Get-Service | Select-Object -Last 10 | Export-Csv -Path C:\File.csv  

To convert the output to comma separated values we can use the following command.  
get-process | Select -first 2 | ConvertTo-Csv | Out-File -filepath D:\FA1_PowerShell\file4.csv  

To redirect the output to html file we can use the following command:  
get-process | select -first 2 | ConvertTo-Html | Out-File -filepath D:\FA1_PowerShell\file1.html  

To redirect the output to xml file we can use the following command:  
get-process | Export-Clixml -path D:\FA1_PowerShell\file2.xml  

Get-Process (To retrieve all running processes in local or remote computer)  
Get-Process -Name powershell (Show details of powershell process only)  
Get-Process -Id 37567 (See details based on process id)  
Get-Process -Name win*,pow* (See details of multiple processes using wildcards and commas)  

Sort-Object - Displays object is sorted order. Ascending by default.  
Get-Process | Sort-Object -Property CPU -Descending  

Measure-Object - This cmdlet performs calculations on property values of objects.  
Get-Process | Measure-Object (Retrieves total no of processes)  
Get-Process | Measure-Object -Property VM -Sum -average -Maximum -Minimum  
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

Risk Mitigation  
-WhatIf parameter will show you messages on the potential effect of PowerShell commands instead of executing them.
Get-Childitem C:\OldFiles\*.txt -Recurse | Remove-Item -WhatIf  
The -Confirm parameter provides a confirmation prompt to the user prior to executing the PowerShell commands.  
Get-Childitem C:\OldFiles\*.txt -Recurse | Remove-Item -Confirm  

Background Jobs:  
Start-Job -Name J1 -ScriptBlock {Get-Process}  
The Start-Job command returns an object that represents the job. The job object contains useful information about the job, but it does not contain the job results.  
Get-Job (check out the status of all scheduled jobs)  
Get-Job -Id 1 (check out the status of a particular scheduled job)  
Receive-Job -Id 1 (To see what output the background job generated)  
Once the “HasMoreData” value turns to false, we cannot receive the job again.  
When Receive-Job returns results, by default, it deletes those results from the cache where job results are stored. If you run another Receive-Job command, you get only the results that are not yet received.  
To prevent Receive-Job from deleting the job results that it has returned, use the Keep parameter. As a result, Receive-Job returns all of the results that have been generated until that time.  
Receive-job -id 3 -Keep  
Wait-Job -ID 3 (Wait-Job lets you wait for a particular job, for all jobs, or for any of the jobs to be completed.)  
As a result, the PowerShell prompt is suppressed until the job is completed.  
wait-job -id 3 -timeout 120 (Timeout parameter to limit the wait to 120 seconds)  
When the time expires, the command prompt returns, but the job continues to run in the background if its not completed.  

$job = Start-Job -ScriptBlock {Get-EventLog -Log System}  
$job | Stop-Job (The following command stops the job. It uses a pipeline operator (|) to send the job in the $job variable to Stop-Job.)  
Remove-Job -id 7(To delete a background job)  

Windows Management Instrumentation(WMI) is a collection of objects which keeps information about computer’s hardware and software.  
The Get-WMIObject cmdlet is used to retrieve information about the various classes in the WMI.  
Get-WMIObject -List (To get a list of all WMI classes)  
Get-WMIObject -Class win32_operatingsystem (To get the Operating system details)  
Get-WMIObject -Class win32_logicaldisk (To get the disk details)  

Powershell Remoting  
PowerShell Remoting connects an administrator's local PowerShell session with a session running on a remote system.  
The commands are entered in the local system, sent to a remote computer and executed locally.  
The remote system then sends the results back to the local system. Windows Remote Management is used to run authentication and communication during this process.  
To execute PowerShell commands or scripts on a remote computer, you need to create a session and it is just like an SSH session to an operating system.  
Enable-PSRemoting -Force -SkipNetworkProfileCheck ()
This command starts the WinRM service and creates a firewall rule to allow incoming connections. The -force option avoids PowerShell to prompt you for confirmation at each step.In a lab environment -SkipNetworkProfileCheck can be used to skip the network check.    
Set-Item wsman:\localhost\client\trustedhosts -Value *  
Configure TrustedHosts: On both computers, configure the TrustedHosts setting so they know each other.  
Create a PowerShell Session and Execute Commands: Now when both computers have been configured, you can create a session using the following commands  
Method 1:  
Enter-PSSession -ComputerName system1 -Credential Administrator (Execute the following command and when prompted, provide the administrator's credentials)  
Observe the [System1] being append in the PowerShell console. Now we have entered the Poershell console of remote system.  
Type "exit" to come out of remote session.  
Method 2: 
Execute the below command and when prompted, provide the administrator's credentials  
Invoke-Command -ComputerName system1 -Credential Administrator -ScriptBlock {Get-Process}

$Error - A global variable which logs all errors during the current PowerShell session.  
It is a collection of PowerShell Error Objects with the most recent error at index 0.  
We can store the non-terminating errors in user defined variables also. The -ErrorVariable parameter allows us to do so.  
Along with the created error variable, $Error variable will also be updated with the error details.  
Stop-Process -Name fakeprocess -ErrorVariable var ($var and $Error will store the error)  

ErrorAction  
For a specific cmdlet, the user can specify the action to be taken if a non-terminating error occurs.  
Continue: The default option.Errors will display and execution will continue.  
SilentlyContinue: Error messages are suppressed and execution continues.  
Get-Process -Name Powershell,Ntpad -ErrorAction silentlycontinue  
Stop: Forces execution to stop, making it like a terminating error.  
Get-Process -Name Powershell,Ntpad -ErrorAction stop  
Ignore (new in v3): The error is ignored and not logged to the error variable($Error).  
Get-Process -Name Powershell,Ntpad -ErrorAction ignore  
Inquire : Prompts the users for the action.  
Get-Process -Name Powershell,Ntpad -ErrorAction Inquire  
$ErrorActionPreference  
PowerShell has a built in variable which allows the user to specify the action to be taken for a non-terminating error.  
The default action preference is “Continue”.  
We can change it for the current session using the following command:  
$ErrorActionPreference = "Silentlycontinue"  

The Try, Catch and Finally statements allow us to control script flow when we encounter errors.  
The behavior of try/catch is to catch the terminating errors.  
Non-terminating errors inside a try block will not trigger the Catch.  
To trigger the Catch due to non-terminating errors, an error action needs to be specified.  
Try block  
Used to enclose a set of statements that might throw an exception.  
The try block must be followed by a Catch or Finally block.  
Catch block  
Used to handle the error thrown by the try block.  
It must be used after the try block only.  
You can use multiple catch block with a single try.  
Finally block  
Finally block follows try or catch block.  
It encloses a block of statements that needs to be executed regardless of whether or not an exception occurs within the try block.  
Try  
{  
Write-Host "Attempting dangerous operation"  
$content = Get-Content -Pathh c:\somefile.txt  
}  
Catch  
{  
Write-Host "Caught an exception"  
Write-Host "Exception type: $($_.Exception.gettype().Fullname)"  
Write-Host "Exception message: $($_.Exception.Message)"  
}  
Finally  
{  
Write-Host "FInally block reached"  
}  
 
[math]::Round(1.23456,2)  













