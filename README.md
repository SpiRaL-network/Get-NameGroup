# Get-NameGroup
Get the name of a local group based on his SID to always get the right name independently of the OS language

I have found it problematic to get the correct name of a local group such as "Administrator" or "Everyone" based on a non-English based OS language.

This can solve a lot of problems, like creating an SMB share on a remote computer using Invoke-Command, where best practice is to set permissions share to full acces for the group "Everyone". 

For exemple in french this group "Everyone" is called "Tout le monde". 
Then if you want to do an SMB share on a French based OS Language like: 
$shareName = "MyShareName"
$shareDescription = "MyShareDescription"
New-SmbShare -Name $shareName -Description $shareDescription -Path $destinationPath -FullAccess "Everyone"

It will throw you and error, stating that this group doesn't exist.

The solution is simple :
$shareName = "MyShareName"
$shareDescription = "MyShareDescription"
$SIDTarget = "S-1-1-0" # This is the well known SID for Everyone group
$everyoneSID = (New-Object System.Security.Principal.SecurityIdentifier($SIDTarget)).Translate([System.Security.Principal.NTAccount])
New-SmbShare -Name $shareName -Description $shareDescription -Path $destinationPath -FullAccess $everyoneSID.Value

This way you can create an SMB share indenpendtly of the name of the "Everyone" group.
