# GCC-Get-users-last-logon-time-O365




PS C:\users\sbailey\downloads> Connect-ExchangeOnline -UserPrincipalName egis@renegadematerials.onmicrosoft.us -ExchangeEnvironmentName O365USGovGCCHigh
>>

----------------------------------------------------------------------------------------
This V3 EXO PowerShell module contains new REST API backed Exchange Online cmdlets which doesn't require WinRM for Client-Server communication. You can now run these cmdlets after turning off WinRM Basic Auth in your client machine thus making it more secure.

Unlike the EXO* prefixed cmdlets, the cmdlets in this module support full functional parity with the RPS (V1) cmdlets.

V3 cmdlets in the downloaded module are resilient to transient failures, handling retries and throttling errors inherently.

REST backed EOP and SCC cmdlets are also available in the V3 module. Similar to EXO, the cmdlets can be run without WinRM basic auth enabled.

For more information check https://aka.ms/exov3-module

Starting with EXO V3.7, use the LoadCmdletHelp parameter alongside Connect-ExchangeOnline to access the Get-Help cmdlet, as it will not be loaded by default
----------------------------------------------------------------------------------------

PS C:\users\sbailey\downloads> # Import Exchange module
>> Import-Module ExchangeOnlineManagement
>>
>> # Connect to Exchange Online (GCC High)
>> Connect-ExchangeOnline `
>>     -UserPrincipalName egis@renegadematerials.onmicrosoft.us `
>>     -ExchangeEnvironmentName O365USGovGCCHigh
>>
>> # Prepare output folder
>> $outputPath = "C:\egistemp"
>> if (-not (Test-Path $outputPath)) {
>>     New-Item -Path $outputPath -ItemType Directory -Force
>> }
>>
>> # Retrieve all mailboxes and last logon times
>> $users = Get-Mailbox -ResultSize Unlimited | ForEach-Object {
>>     $stats = Get-MailboxStatistics -Identity $_.PrimarySmtpAddress
>>     [PSCustomObject]@{
>>         DisplayName   = $_.DisplayName
>>         EmailAddress  = $_.PrimarySmtpAddress.ToString()
>>         LastLogonTime = $stats.LastLogonTime
>>     }
>> }
>>
>> # Export to CSV
>> $csvFile = Join-Path $outputPath "Renegade_O365_UserList.csv"
>> $users | Export-Csv -Path $csvFile -NoTypeInformation -Encoding UTF8
>>
>> # Disconnect session
>> Disconnect-ExchangeOnline -Confirm:$false
>>
>> Write-Host "Export complete: $csvFile"
>>

----------------------------------------------------------------------------------------
This V3 EXO PowerShell module contains new REST API backed Exchange Online cmdlets which doesn't require WinRM for Client-Server communication. You can now run these cmdlets after turning off WinRM Basic Auth in your client machine thus making it more secure.

Unlike the EXO* prefixed cmdlets, the cmdlets in this module support full functional parity with the RPS (V1) cmdlets.

V3 cmdlets in the downloaded module are resilient to transient failures, handling retries and throttling errors inherently.

REST backed EOP and SCC cmdlets are also available in the V3 module. Similar to EXO, the cmdlets can be run without WinRM basic auth enabled.

For more information check https://aka.ms/exov3-module

Starting with EXO V3.7, use the LoadCmdletHelp parameter alongside Connect-ExchangeOnline to access the Get-Help cmdlet, as it will not be loaded by default
----------------------------------------------------------------------------------------

WARNING: The user hasn't logged on to mailbox
'DiscoverySearchMailbox{D919BA05-46A6-415f-80AD-7E09334BB852}@renegadematerials.onmicrosoft.us'
('a028de52-9139-4de1-85f9-842d9c382990'), so there is no data to return. After the user logs on, this warning will no
longer appear.
Export complete: C:\egistemp\Renegade_O365_UserList.csv
PS C:\users\sbailey\downloads>



