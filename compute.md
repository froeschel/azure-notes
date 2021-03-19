## Compute

### IaaS
* VM price can be reduced if you shut it down. You still need to pay for storage.
* Spot instance: Machine at a reduced price for a limited amoount of time.
* Disk is stored as VHD (virtual hard disk). Three options: `Premium SSD`, `Standard SSD`, `Standard HDD`.
* Disk performance is expressed with IOPS and throughput (MBps).
* For `Premium SSD` performance is guaranteed.
* Disk is encrypted at rest with platform encryption key.
* Three types of disks: OS Disk, Data Disk and Temporary Disk.
* Most disk for VMs are managed (managed disk stored as PageBlobs in storage account that you don't need to manage).
* Data disks can be atached and detached to a VM.
* Vnet for VM needs to be in the same region. 
* When using RDP to connect remember to open ports in NSG.
* Use gen1 VMs unless you have specific requirements.
* When creating a VM it needs a name, location, group, vnet, subnet, nsg.
* 1 VM is made of `Machine`,`Network Interface`, `Data Disk`, `Public IP`, `NSG`, `VNET`, `Data Disk`.
* A group of identical VMs can be created by using scale sets.
* VMs of generation 1 support disk encryption. Generation 2 has no support for disk encryption. Fixed disk is required. 
* Update-AzVM: Updating the state of a VM. E.g. with a an IdentityId [Update-AzVM - IdentityId]. This sets the number of identities.
* `az vm identity assign` to assign an identity that has been created with `az identity create`
* Configure remote access on Windows: Use the portal to get the RDP connection.
* Configure remote access on Linux: Generate a keypair: Public key to vm, private key on client.
* When creating a resource one can download a ARM template [template.json + parametrs.json].
* In the portal a custom deployment can be created by uploading the template and parameters file.
* Deploy ARM with CLI: `az deployment group create --resource-group Responza-KB-Test --template-file template.json --parameters parameters.json`
* `az group export` captures the complete resource group as a template.
* When one wants to publish an image to the ACR it needs to be tagged the following way [reg-name].azurecr.io/[name-of-image].
* ACR a private repository of images.
* Create a ACR: `az acr create`.
* Generate an image: `az acr build.` When image is build it is pushed to a registry.
* Container instances are simpler than kubernetes, but do not offerthe same flexibility.
* A resource group is a logical grouping of resources. They can be controlled together.
* Resources within a resource group can have different locations.
* A resource group needs a location, because it stores metadata. This is important when thinking about complience. 
* Not all resources can be moved from one resource group to another. For reference [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources)
* It is possible to make REST calls that can validate if a move of resources would be successful.
* During a move it is not possible to write or delete resources.
* Moving resources between groups does not change their location.
* 



### Web Apps
* We use Azure App Service Web Apps. Code is deployed using Devops pipelines. Web apps can be created using the Portal, CLI, ARM Templates or Terraform.
* Apps can be executed in different runtimes like .NET, Java, Node.js, PHP, Python or Ruby.
* Wep Apps are running inside an App Service Plan. An App Service Plan can be scaled up (more computing power) or out (more instances).
* App Service Plans can scale out. One could use manual scale or custom autoscale. Maual scale: Setting a predined number of instances. Autoscale: Scale based on a metric or scale to a specific instance count at a specific time. Manual scale: Simply set the instance count. 
* Application logging for win/linux, webserver logging win only. Linux only supports applicattion logging and deployment logging. Logs go to file system og blob storage.

### Functions
* During the first call to a function in a consumption plan it can take som time to get a reply. To avoid that use premium plan.
* Functions supported languages: C#, JavaScript, F#, Java, PowerShell, Python, TypeScript.
* Functions can be run by timer trigger. These are set by using NCRONTAB expressions. They have the format {second} {minute} {hour} {day} {month} {day-of-week}. So if you want to run the function every 3 hours it should look like this: 0 0 */3 * * *
* Triggers are what causes a function to run e.g. an event in the system. A function can only have one trigger.
* Bindings: Input --> data that comes in to the function. Output --> data that is send. Function can have multiple input and output bindings.
* Durable function for long running operations that need to maintain state. 
* To work with duarble function the extension in the portal needs to be activated.
* It is possible to check status of a durable function using `statusQueryGetUri`.
