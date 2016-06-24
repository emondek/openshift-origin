# OpenShift Origin with Azure Active Directory

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fharoldwongms%2Fopenshift-origin%2Fmaster%2Fno-ad-int-keyvault%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fharoldwongms%2Fopenshift-origin%2Fmaster%2Fno-ad-int-keyvault%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

This template deploys OpenShift Origin with basic username / password for authentication to OpenShift. It includes the following resources:

|Resource           |Properties                                                                                                                          |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
|Virtual Network    |**Address prefix:** 10.0.0.0/16<br />**Master subnet:** 10.0.0.0/24<br />**Node subnet:** 10.0.1.0/24                               |
|Load Balancer      |2 probes and two rules for TCP 80 and TCP 443                                                                                       |
|Public IP Addresses|OpenShift Master public IP<br />OpenShift Router public IP attached to Load Balancer                                                |
|Storage Accounts   |2 Storage Accounts                                                                                                                  |
|Virtual Machines   |Single master<br />User-defined number of nodes<br />All VMs include a single attached data disk for Docker thin pool logical volume|

## Prerequisites

### Generate SSH Keys

You'll need to generate a pair of SSH keys in order to provision this template. Ensure that you do not include a passcode with the private key.

### Create Key Vault to store SSH Private Key

You will need to create a Key Vault to store your SSH Private Key that will then be used as part of the deployment.

1. Create KeyVault using Powershell<br/>
<&nbsp><&nbsp>   a.  Create new resource group: New-AzureRMResourceGroup -Name 'ResourceGroupName' -Location 'West US'<br/>
<&nbsp><&nbsp>   b.  Create key vault: New-AzureRmKeyVault -VaultName 'KeyVaultName' -ResourceGroup 'ResourceGroupName' -Location 'West US'<br/>
<&nbsp><&nbsp>  c.  Create variable with sshPrivateKey: $securesecret = ConvertTo-SecureString -String '[copy ssh Private Key here - including line feeds]' -AsPlainText -Force<br/>
<&nbsp><&nbsp>   d.  Create Secret: Set-AzureKeyVaultSecret -Name 'SecretName' -SecretValue $securesecret -VaultName 'KeyVaultName'<br/>
   
### azuredeploy.Parameters.json File Explained

1.  masterVmSize: Select from one of the allowed VM sizes listed in the azuredeploy.json file
2.  nodeVmSize: Select from one of the allowed VM sizes listed in the azuredeploy.json file
3.  openshiftMasterHostName: Host name for the Master Node
4.  openshiftMasterPublicIpDnsLabelPrefix: A unique Public DNS name to reference the Master Node by
5.  nodeLbPublicIpDnsLabelPrefix: A unique Public DNS name to reference the Node Load Balancer by.  Used to access deployed applications
6.  nodePrefix: prefix to be prepended to create host names for the Nodes
7.  nodeInstanceCount: Number of Nodes to deploy
8.  adminUsername: Admin username for both OS login and OpenShift login
9.  adminPassword: Admin password for both OS login and OpenShift login
10. sshPublicKey: Copy your SSH Public Key here
11. sshPrivateKey - id: Should adhere to: "/subscriptions/[subscirption GUID]/resourceGroups/[ResourceGroupName]/providers/Microsoft.KeyVault/vaults/[KeyVaultName]"
    a. Example: "/subscriptions/111a111b-1234-1abc-aa1a-11df2345ab67/resourceGroups/MyResourceGroup/providers/Microsoft.KeyVault/vaults/MyKeyVault"
12. sshPrivateKey - SecretName: The Secret Name you used when creating the Secret

## Deploy Template

Once you have collected all of the prerequisites for the template, you can deploy the template by populating the *azuredeploy.parameters.json* file and executing Resource Manager deployment commands with PowerShell or the xplat CLI.

### NOTE

The OpenShift Ansible playbook does take a while to run when using VMs backed by Standard Storage. VMs backed by Premium Storage are faster. If you want Premimum Storage, select a DS or GS series VM.
<hr />
Be sure to follow the OpenShift instructions to create the ncessary DNS entry for the OpenShift Router for access to applications.

## Post-Deployment Operations
 
## Additional OpenShift Configuration Options
 
You can configure additional settings per the official [OpenShift Origin Documentation](https://docs.openshift.org/latest/welcome/index.html).
