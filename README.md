# patch_checker
PowerShell script to use the PSWindowsUpdate module to check for available patches.

```
# Script to use the PSWindowsUpdate module to check for available patches.
# You will need to: Install-Module -Name PSWindowsUpdate

$category = "Security Updates"
$sender = "nobody@example.com" # email address that will appear as the FROM address
$recipient = "nobody@example.com" # email address that will recieve the alerts
$smtp_server = "192.168.1.x"
# ---------------------------------------

$patches = Get-WUList -Category $category

# Should spit out something like this...
# ComputerName Status     KB          Size Title
# ------------ ------     --          ---- -----
# WIN-3ADKL... -------    KB4577586   21KB Update for Removal of Adobe Flash Player for Windows Server 2019 ...
# WIN-3ADKL... -D-----    KB890830    38MB Windows Malicious Software Removal Tool x64 - v5.95 (KB890830)
# WIN-3ADKL... -D-----    KB5007298   76MB 2021-11 Cumulative Update for .NET Framework 3.5, 4.7.2 and 4.8 f...
# WIN-3ADKL... -D-----    KB5007206   16GB 2021-11 Cumulative Update for Windows Server 2019 (1809) for x64-...

if ($patches.Count -gt 0) {

    foreach ($patch in $patches) {
        #echo "$($patch.KB): $($patch.Title)"
        Send-MailMessage -From $sender -To $recipient -Subject "$($patch.ComputerName) patches available" -Body "$($patch.KB): $($patch.Title)" -SmtpServer $smtp_server
    }

}

```
