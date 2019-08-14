$vms = Import-CSV "C:\NewVMs.csv"
 foreach ($vm in $vms){
      #Assign Variables
      $Template          = $vm.Template
      $Cluster           = $vm.Cluster
      $Datastore         = $vm.Datastore
      $Custom            = "win2012"
      $vCPU              = $vm.vCPU
      $Memory            = $vm.Memory
      $HardDrive         = $vm.Harddrive
      $HardDrive2        = $vm.Harddrive2
      $Network           = $vm.Network
      $Location          = $vm.Location
      $VMName            = $vm.Name
      $IP                = $vm.IP
      $SNM               = $vm.SubnetMask
      $GW                = $vm.Gateway
      $DNS1              = $vm.DNS1
      $DNS2              = $vm.DNS2
      $WINS1             = $vm.WINS1
      $WINS2             = $vm.WINS2
      $Description       = $VM.Description
      $OU                = $vm.OU
      $Agency            = $VM.Agency
      $Application       = $VM.Application
      $Billed            = $VM.Billed
      $Contact           = $VM.Contact
      $Phone             = $VM.ContactPhone
      $CreatedOn         = Get-Date -Format dd-MMM-yyyy
      $Managed           = $VM.Managed
      $RebootPolicy      = $VM.RebootPolicy
      $TicketNumber      = $VM.TicketNumber
      $vCenter_Server    = $vCenter_Server
      $Password          = $Password
      $Domain            = $Domain
      $Domainuser        = $DomainUser
      $DomainPassword    = $Password
      $VMHost              = $VM.Host
      $IFADAccountExists = ""

      
      #Generate a new OSCustomizationSpec so this will add the servers to the domain, and configure the NIC
      $OSCustomizationSpecExists = Get-OSCustomizationSpec -name temp-win2012
      IF ($OSCustomizationSpecExists -eq $True){
          Remove-OSCustomizationSpec -OSCustomizationSpec temp-win2012 -Confirm:$false
          }
      Write-Host "Generating OSCustomizationSpec file" -ForegroundColor Yellow
      New-OSCustomizationSpec -OrgName "Organization_Name" -OSType Windows -ChangeSid -Server $vCenter_Server -Name win2012 -FullName "Company_Name" -Type Persistent -AdminPassword $Password -TimeZone 'China' -AutoLogonCount 3 -Domain $Domain_Name -Domainuser $Domainuser -Domainpassword $Password -NamingScheme Vm -Description "PowerCli Use only" -LicenseMode PerServer -LicenseMaxConnections 5 -Confirm:$false
      Write-Host "Generating OSCustomizationNicMapping file" -ForegroundColor Yellow
      Get-OSCustomizationNicMapping -OSCustomizationSpec $Template | Set-OSCustomizationNicMapping -Position 1 -IpMode UseStaticIP -IpAddress $IP -SubnetMask $SNM -DefaultGateway $GW -Dns $DNS1,$DNS2 -Wins $WINS1,$WINS2 -Confirm:$false
            
      Write-Host "Generating new VM per spec sheet" -ForegroundColor Yellow
      Start-Job -ScriptBlock {New-VM  -Name $VMName  -Datastore $Datastore -Template $Template -OSCustomizationSpec $Template -VMHost $VMHost -confirm:$False}
}

      wait-Job | Get-Job
foreach ($vm in $vms){
      #Assign Variables
      $Template          = $vm.Template
      $Cluster           = $vm.Cluster
      $Datastore         = $vm.Datastore
      $Custom            = "win2012"
      $vCPU              = $vm.vCPU
      $Memory            = $vm.Memory
      $HardDrive         = $vm.Harddrive
      $HardDrive2        = $vm.Harddrive2
      $Network           = $vm.Network
      $Location          = $vm.Location
      $VMName            = $vm.Name
      $IP                = $vm.IP
      $SNM               = $vm.SubnetMask
      $GW                = $vm.Gateway
      $DNS1              = $vm.DNS1
      $DNS2              = $vm.DNS2
      $WINS1             = $vm.WINS1
      $WINS2             = $vm.WINS2
      $Description       = $VM.Description
      $OU                = $vm.OU
      $Agency            = $VM.Agency
      $Application       = $VM.Application
      $Billed            = $VM.Billed
      $Contact           = $VM.Contact
      $Phone             = $VM.ContactPhone
      $CreatedOn         = Get-Date -Format dd-MMM-yyyy
      $Managed           = $VM.Managed
      $RebootPolicy      = $VM.RebootPolicy
      $TicketNumber      = $VM.TicketNumber
      $vCenter_Server    = $vCenter_Server
      $Password          = $Password
      $Domain            = $Domain
      $Domainuser        = $DomainUser
      $DomainPassword    = $Password
      $VMHost              = $VM.Host
      $IFADAccountExists = ""
      #Sets the new VM as a variable to make configuration changes faster
      $NewVM = Get-VM -Name $VMName

      Write-host "Setting Memory and vCPU on $VMName" -ForegroundColor Yellow
      $NewVM | Set-VM -MemoryGB $Memory -NumCpu $vCPU -Confirm:$false
  

            
      #Primary Harddrive
      $NewVMHddSize = ($NewVM | Get-HardDisk | Where {$_.Name -eq "Hard disk 1"}).CapacityGB
      IF ($HardDrive -gt $NewVMHddSize){$NewVM | Get-HardDisk | Where {$_.Name -eq "Hard disk 1"} | Set-HardDisk -CapacityGB $HardDrive -Confirm:$false}
            
      
         
      

      #Powers on the server
      Write-host "Powering on $VMName" -ForegroundColor Yellow
      Start-VM -VM $VMName -Confirm:$False
      
}
