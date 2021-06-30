function handler($context, $payload) {

  # Build PowerShell variables
  $VMName = $payload.resourceNames
  $cpu = $payload.customProperties.cpu
  $mem = $payload.customProperties.mem
  $description = $payload.customProperties.description
  $IPaddress = $payload.addresses
  $emailAddress = $payload.customProperties.emailAddress
  $RootPartition = $payload.customProperties.RootPartition
  $BuildBY = $payload.customProperties.BuildBY
  $BuildRequestor = $payload.customProperties.BuildRequestor
  $OSVersion = $payload.customProperties.OSVersion
    
  # Show PS variables
  Write-Host 'VMName:' $VMName
  Write-Host 'CPU:' $cpu
  Write-Host 'Memory:' $mem
  Write-Host 'Description:' $description
  Write-Host 'IPaddress:' $IPaddress
  Write-Host 'Email:' $emailAddress
  Write-Host 'Root:' $RootPartition
  Write-Host 'Build By:' $BuildBY
  Write-Host 'Build BuildRequestor:' $BuildRequestor
  Write-Host 'OS Version:' $OSVersion

  # ----- [ Define Username/ Get Password ] ------------------------------------------------------------------

  $HashiVaultPW = 'vCROCS#1'
  Write-Host $HashiVaultPW

  $username = 'administrator@vsphere.local'
  Write-Host $username

  # ----- [ Connect vCenter and run PS Script on PS Host ] ------------------------------------------------------------------
  
  # Connect to vCenter to be able to connect to PS Host
  Connect-VIServer -Server vCenter01.vCROCS.info -User $username -Password $HashiVaultPW -Protocol https -Force
  
  # PS Host Name
  $PSHost = 'vCROCSPSHost'
  Write-Host 'PS Host:'$PSHost
  
  # PS Script text
  $PSParameters = "-VMNAME '" + $VMName + "' -emailAddress '" + $emailAddress + "' -RootPartition '" + $RootPartition + "'"
  Write-Host 'PS Parameters:'$PSParameters
  
  $PSScript = 'G:\Scripts\Create-Linux-Server-Step-3-v01.ps1'
  Write-Host 'PS Script:'$PSScript
  
  $PSText = 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe "' + $PSScript + '" ' + $PSParameters
  Write-Host 'Complete PS String:'$PSText
  
  # Run PS Script on PS Host
  $result = Invoke-VMScript -VM $PSHost -ScriptType Bat -ScriptText $PSText -GuestUser $username -GuestPassword $HashiVaultPW
  Write-Host $result.ScriptOutput
  
  return $LASTEXITCODE
}
