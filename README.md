<p align="center">
  <b>Azure CLI Commands
  </b>
</p>

PowerShell - Install Az module to your local machine using: 
Install latest version of Powershell before installing Az module

Step1: Install a supported version of [PowerShell version 7 or higher](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.4)

Launch PowerShell as ADMIN and copy paste

<winget search Microsoft.PowerShell

<b>Step2: Install PowerShell or PowerShell Preview using the id parameter</b>
<br />winget install --id Microsoft.Powershell.Preview --source winget

The installer will prompt to run as admin or you can also install using installer [here](https://github.com/PowerShell/PowerShell/releases/download/v7.4.0/PowerShell-7.4.0-win-x64.msi)

<b>Step3: Run this command to determine Powershell version</b>
<br />$PSVersionTable.PSVersion

<b>Step4: Check the PowerShell execution policy</b><br />
Get-ExecutionPolicy -List

<b>Step5: Set the PowerShell execution policy to remote signed</b><br />
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

<b>Step 6: Use the Install-Module cmdlet to install the Az PowerShell module
</b>
<br />Install-Module -Name Az -Repository PSGallery -Force

<b>Step 7: Use Update-Module to update to the latest version of the Az PowerShell module
</b>
<br />Update-Module -Name Az -Force

<b>Sign in: Use command below to prompt an Azure sign in</b>
<br />Connect-AzAccount

<b>Create Resource group
</b>
<br />New-AzResourceGroup -Name resourceGroupName -Location “location”

Ex
<br />New-AzResourceGroup -Name RG1 -Location “East US”

<b>Using Positional parameters
</b>
<br />New-AzResourceGroup RG01 “East US”

<b>Delete Resource group
</b>
<br />Remove-AzResourceGroup -Name {resourceGroupName}

<b>Create a Storage account
</b>
<br />New-AzStorageAccount -ResourceGroupName MyResourceGroup -Name mystorageaccount -Location “East US” -SkuName Standard_GRS

<b>Create a Blob Storage account with BlobStorage Kind and hot AccessTier
</b>
<br />New-AzStorageAccount -ResourceGroupName MyResourceGroup -Name mystorageaccount -Location westus -SkuName Standard_GRS -Kind BlobStorage -AccessTier Hot

<b>Transfer Objects to/from Azure Blob storage
Create Container to hold the blob using
Need to define the context before running the command
</b>
</br>$ContainerName = 'quickstartblobs'
New-AzStorageContainer -Name $ContainerName -Context $Context<br />
or<br />
New-AzStorageContainer -Name "ContainerName" -Permission Off

<b>Create multiple containers
</b><br />"container1 container2 container3".split() | New-AzStorageContainer -Permission Container

<b>Create Vms - Source</b><br />
<b>Step1: Create ResourceGroup</b></br>New-AzResourceGroup -Name RG1 -Location “East US”

<b>Step 2: Create admin creds for vm
</b><br />$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

<b>Step 3: Create a vm
</b></br>Lets use Powershell splatting to pass parameters to the Azure PowerShell cmdlets<br />
    $vmParams = @{<br />
    ResourceGroupName = 'TutorialResources'<br />
    Name = 'TutorialVM1'<br />
    Location = 'eastus'<br />
    ImageName = 'Win2016Datacenter'<br />
    PublicIpAddressName = 'tutorialPublicIp'</br>
    Credential = $cred<br />
    OpenPorts = 3389<br />
    Size = 'Standard_D2s_v3'<br />
    }<br />
    $newVM1 = New-AzVM @vmParams


<b>Verify VM name and admin account
</b><br />$newVM1.OSProfile | Select-Object -Property ComputerName, AdminUserName

Use the public IP to connect via RDP on your local device.


<b>Creating a new VM on the existing subnet
</b></br>$vm2Params = @<br />
  ResourceGroupName = 'TutorialResources'<br />
  Name = 'TutorialVM2'<br />
  ImageName = 'Win2016Datacenter'<br />
  VirtualNetworkName = 'TutorialVM1'<br />
  SubnetName = 'TutorialVM1'<br />
  PublicIpAddressName = 'tutorialPublicIp2'</br>
  Credential = $cred<br />
  OpenPorts = 3389<br />
}<br />
$newVM2 = New-AzVM @vm2Params<br />
$newVM2

Notice we did not indicate the Location and the but referenced the existing VirtualNetworkName of TutorialVM1 and the existing subnetName of TutorialVM1



