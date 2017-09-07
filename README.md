# HIBP
Find if your password have been pwned without online check

## Get the pwned passwords library
- First go to https://haveibeenpwned.com/Passwords
- Download the 7zip files containing SHA1 hash of thousands of pwned passwords
- Extract these (the first dump is more than 12GB text file... stored in a 5GB 7zip container)

## Hash the passwords you want to test
use the following script : (made by [Jon Gurgul](https://gallery.technet.microsoft.com/scriptcenter/Get-StringHash-aa843f71#content)
```
#http://jongurgul.com/blog/get-stringhash-get-filehash/ 
Function Get-StringHash([String] $String,$HashName = "MD5") 
{ 
$StringBuilder = New-Object System.Text.StringBuilder 
[System.Security.Cryptography.HashAlgorithm]::Create($HashName).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($String))|%{ 
[Void]$StringBuilder.Append($_.ToString("x2")) 
} 
$StringBuilder.ToString() 
}
```


## Search for the hashed password
```
sls [paswordhash] [path\to\pwfile]
```  

## NOW :Cry  

## Example
### Add the command to your function drive
#### look at your cmdlet available ```dir function:\Get-S*```
```
PS C:\Users\mathi\Documents\HIBP> dir function:\Get-S*
PS C:\Users\mathi\Documents\HIBP>
```
Nothing shows up showing this cmdlet isn't available  

#### "dot-source" the script containing the command
If you called the script ```HashString.ps1``` and placed it where you opened the PS terminal:
```
. .\HashString.ps1
```  

#### look at your cmdlet available ```dir function:\Get-S*```
```
PS C:\Users\mathi\Documents\HIBP> dir function:\Get-S*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-StringHash
```  

### Hash your password
```
PS C:\Users\mathi\Documents\HIBP> Get-StringHash "password" "SHA1"
5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8
```  

### Search for the hashed password in the file
```
PS C:\Users\mathi\Documents\HIBP> sls 5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8 .\pwned-passwords-1.0.txt
pwned-passwords-1.0.txt:109236128:5BAA61E4C9B93F3F0682250B6CF8331B7EE68FD8
```
if PS finds it, then your password has been pwned.

# OneLineCommand
```. .\HashString.ps1; sls -Pattern (Get-StringHash "password" "SHA1") .\pwned-passwords-1.0.txt```
