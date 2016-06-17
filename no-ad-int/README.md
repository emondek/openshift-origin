# OpenShift Origin with Azure Active Directory

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fanweiss%2Fazure-quickstart-templates%2Fopenshift-origin-rhel%2Fopenshift-origin-rhel%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fanweiss%2Fazure-quickstart-templates%2Fopenshift-origin-rhel%2Fopenshift-origin-rhel%2Fazuredeploy.json" target="_blank">
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

## Deploy Template

Once you have collected all of the prerequisites for the template, you can deploy the template using the **Deploy to Azure** button at the top or by populating the *azuredeploy.parameters.json* file and executing Resource Manager deployment commands with PowerShell or the xplat CLI.

### NOTE

The OpenShift Ansible playbook does take a while to run when using VMs backed by Standard Storage. VMs backed by Premium Storage are faster. If you want Premimum Storage, select a DS or GS series VM.
<hr />
Be sure to follow the OpenShift instructions to create the ncessary DNS entry for the OpenShift Router for access to applications.

## Post-Deployment Operations
 
## Additional OpenShift Configuration Options
 
You can configure additional settings per the official [OpenShift Origin Documentation](https://docs.openshift.org/latest/welcome/index.html).
