# Notes

## Compute Solution

### IAAS
* VM price can be reduced if you shut it down. You still need to pay for storage.
* Spot instance: Machine at a reduced price for a limited amoount of time.
* Disk is stored as VHD (virtual hard disk). Three option: `Premium SSD`, `Standard SSD`, `Standard HDD`.
* Disk is encrypted at rest with platform encryption key.
* Data disks can be atached and detached to a VM.
* Vnet for VM needs to be in the same region. 
* When using RDP to connect remember to open ports in NSG.
* Use gen1 VMs unless you have specific requirements.
* When creating a VM it needs a name, location, group, vnet, subnet, nsg.
* 1 VM is made of `Machine`,`Network Interface`, `Data Disk`, `Public IP`, `NSG`, `VNET`, `Data Disk`.
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
*



### Web Apps
* We use Azure App Service Web Apps. Code is deployed using Devops pipelines. Web apps can be created using the Portal, CLI, ARM Templates or Terraform.
* Apps can be executed in different runtimes like .NET, Java, Node.js, PHP, Python or Ruby.
* Wep Apps are running inside an App Service Plan. An App Service Plan can be scaled up (more computing power) or out (more instances).
* App Service Plans can scale out. One could use manual scale or custom autoscale. Maual scale: Setting a predined number of instances. Autoscale: Scale based on a metric or scale to a specific instance count at a specific time. Manual scale: Simply set the instance count. 

### Functions
* During the first call to a function in a consumption plan it can take som time to get a reply. To avoid that use premium plan.
* Functions supported languages: C#, JavaScript, F#, Java, PowerShell, Python, TypeScript.
* Functions can be run by timer trigger. These are set by using NCRONTAB expressions. They have the format {second} {minute} {hour} {day} {month} {day-of-week}. So if you want to run the function every 3 hours it should look like this: 0 0 */3 * * *
* Triggers are what causes a function to run e.g. an event in the system. A function can only have one trigger.
* Bindings: Input --> data that comes in to the functuin. Output --> data that is send. Function can have multiple input and output bindings.
* Durable function for long running operations that need to maintain state. 
* To work with duarble function the extension in the portal needs to be activated.
* It is possible to check status of a durable function using `statusQueryGetUri`.

## Storage

* Data can be categorized into three different types: structured, semi structered and unstructured.
* Structured use SQL, semi structured use Cosmos, unstructured use Blob storage.
* Azure storage has four data services: `Blobs, Files, Queues, Tables`.
* Unmanged storage accounts must have a unique name across Azure due to REST endpoint.
* Storage aacount standard SKU is ok. For new accounts use GPv2 [GPv1 only for legacy].
* `LRS`: 3 copies in one AZ. `ZRS`: 3 copies one per AZ. `RA-GRS`: 6 copies --> 3 per region + additional Read access for distribution. `GZRS`: 6 copies --> two regions --> primary and secondary read access.
* Not all replication options are available in every region.
* `Soft delete` gives the possibility to restore deleted files within a specified retention period.


### Cosmos
* Azure Cosmos DB is a fully managed NoSQL database for modern app development. Single-digit millisecond response times, and automatic and instant scalability, guarantee speed at any scale.
* Pricing can be a bit complicated. Price is calculated per Request Unit (RU). RU is a 'performance currency abstracting the system resources such as CPU, IOPS, and memory'. So database operations can have a variable amount of RUs, depending on how much system resources it takes. You pay for the throughput in RU that you provision. One can have specific pricing per container or share RUs for the whole database.
* Cosmos DB provides five different database APIs: SQL, Cassandra, Mongo, Gramlin and Table API.
* A cosmos container should have a partition key. With a partition key,documents will be grouped into logical partitions. It is important to set a partition key so that all documents are evenly distributed. To find the right key canbe tricky when starting a new project.
* When creating a Cosmos database it can be set to 5 different consistency levels [Strong, Bounded staleness, Session, Consistent prefix, Eventual]. [Strong] A client will never read a uncomitted or partial write, [Bounded staleness] A read might lack behind a write, [Consistent Prefix] reads never see out-of-order writes, [Eventual] a client may read the values that are older than the ones it had read before.
* Stored precedures: Stored procedures are written using JavaScript, they can create, update, read, query, and delete items inside an Azure Cosmos container.
* Triggers: Pre-trigger are executed before modifying an item. Post-trigger after modifying an item.
* Trigger and stored procedures must be registered and called through the SDK.
* Triggers, stored procedures and UDF only works with SQL API.
* Changefeed notification:

### Blob Storage
* Blob storage offers 3 different access tiers: Hot [frequently accessed], cool [infrequently accessed] and archived [for long term backup].
* Files can be moved by CLI, AzCopy or by using client library.
* A file in blob storage has metadata and properties. Metadata is user generated (docType, docClas) and properties are system generated (LastModified, BlobType).
* One can set metadata on a blob with following command `PUT https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata`.
* Blob storage is accesible by using Azure Storage REST API, Azure PowerShell, Azure CLI or client libraries in .NET, Java, Node.js, Python, Go, PHP or Ruby.
* Rules can be set so that blobs automatically can move to another access tier based ona property e.g. LastModified.
* Blob storage account URL format: `https://{accountName}blob.core.windows.net/{container}/{blobName}`.
* Blob leasing = prevent override or deletion.
* Create a read onlu snapshot: `https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=snapshot`. 
* Get a snapshot of a blob using query string: `https://myaccount.blob.core.windows.net/mycontainer/myblob?snapshot=<DateTime>`.

## Security

### Authentication and Authorization
* OpenID Connect for authentication and OAuth2 for authentication.
* Shared Access Signatures (SAS) grant limited access to resources. Three types [User, Service and Account].
* SAS keys can be genereted in PowerShell, CLI or .NET.
* RBAC is used to control access to resources.
* Role assignment: attaching a role to a user, group, service principal, or managed identity at a particular scope for the purpose of granting access.
* There are some build in roles e.g.: Owner, Contributor, Reader, User Access Administrator.
* Role assignments can be made in the portal using the IAM blade.

### Secure Cloud Solutions
* Azure Key Vault API: Azure Key Vault can manage secrets, keys and certificates. When using C# one can access keys by using the `KeyClient` class.
* Key Vault is secured by Azure AD. It is possible to assign permissions to a Key Vault. Permission are managed in 'Access policies'.
* In Functions you can reference the Key Vault by using an environment variable in format `@Microsoft.KeyVault(SecretUri=copied identifier for the secret)`. 
* Managed identities: Used for giving access to to resources (e.g. KeyVault) without having to handle credentials.
* Managed identities are free to use.
* System assigned: Enable a managed identity directly on a service instance.
* User assigned: Identity created by the user and assined to different resources. 


## Monitoring and Optimization

### Caching
* Cache expiration policies for FrontDoor, CDNs, or Redis caches store.
* One CDN profile has one or more endpoints.
* When creating a CDN endpoint you add the endpoint url and the origin hostname url. 
* CDN endpoints can have custom domain.
* When origin content changes, purge CDN.
* Azure CDN caching rules: Bypass cache [do not cache], Override [override origin cache], Set if missing [use own if origin missing].
* Query string caching: Ignore query strings [deault, cache in POP node], Bypass caching for query strings [do not cache query strings at all], Cache every unique URL [every url set as an unique asset].
* Get information from Redis: var info = `database.Execute("INFO");`.
* Some Redis commands: `SET` sets a value in a key, `SETEX` sets value and a key and a expiration time, `EXPIRE` sets a key to expire, `EXISTS` check if a key exists in Redis.  


### Logging
* Enable diagnostics logging for App Servive: On the menu 'App Service logs' switch on 'Application Logging' and 'Web server logging'. Select a quota and a retention period.
  Logs can be downloaded from this url: `https://<app-name>.scm.azurewebsites.net/api/dump`
* View Log Stream: The log stream can be viewed on the menu item 'Log stream'.
* Logging with Application Insights: When configuring Application Insights in a ASP.NET core app the ILogger sends warnings and above to AI sink. Read more [here](https://docs.microsoft.com/en-us/azure/azure-monitor/app/ilogger).
* Use `SqlAzureExecutionStrategy` to handle transient faults. Can by used manually `executionStrategy.Execute(() =>` or be set in the DBConfiguration class.

## Third Party Services

### Logic App
* Logic Apps are cloud services that automate a business process.
* A trigger activates when a condition is met. An action executes a task. Control actions are special actions that provides these control constructs:
* Logic Apps can have following control actions [For Each, Condition, Switch, Until].
* Logic Apps are creted in the graphical designer.
* A custom connector can be created by importing an API sepcification.


### API Management
* An API Management instance enables client applications to use OAuth 2.0 authentication when using an AAD tenant.
* API verision: A new version needs to be published and can introduce breaking changes. Address a new version via version header.
* API revision: A revision does not need to be published. A revision can be accessed by using a query string on the same endpoint. 
* Use the `rate-limit-by-key` policy to protect the API againts client usage spikes. When a client triggers the policy it gets a 429. Policy definition:
```xml
<rate-limit-by-key calls="number"
                   renewal-period="seconds"
                   increment-condition="condition"
                   counter-key="key value" />
```
* Use `quota-by-key` to define max calls and or bandwith that can be used in a specific period.
```xml
<quota-by-key calls="number"
              bandwidth="kilobytes"
              renewal-period="seconds"
              increment-condition="condition"
              counter-key="key value" />
```

### Event processing
* Event Grid: For reacting to status changes in event driven architecture.
* With Event Grid you can create a a subscription for an event so that you get notified when changes happen.
* Register event grid as resource provider: `az provider register --namespace Microsoft.EventGrid`. 
* Azure Event Hub: For streaming and telemetry of distributed data. E.g. IoT devices.
* Event Hub connection string format: `Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>`
* Event Hub send events: Use the `EventHubClient` class. It has the method `SendAsync(EventData event)` where EventData takes a byte array. 
* Event Hub process events: Use the  `IEventProcessor` interface. It has a method called  `ProcessEventAsync`. 
* Notification hub: Push engine to send notifications to different platforms (iOS, Android, Windows).
* Notification Hubs: Two resource levels --> hubs and namespaces. Hub is a push resource for one app. A namespace is a collection of hubs in one region.


### Messaging
* Queues consists of orederd timestamped messages. Integrate with queues using the `QueueClient`.
* Use `client.SendAsync(new Message(Encoding.UTF8.GetBytes(message)))` to send messages to the Service Bus.
*  Azure Queue Storage queues, provide cloud messaging between components. This can be helpful when designing systems that have components which need to scale independently. To get messages from queue you can use the following code: 
* The max size for a message in Azure Queue Storage is 64 KB.
* Azure Service Bus: Used to send messages between system components. These messages are consumed elsewhere in the system.
* Service Bus has more features than storage queue.
* Service Bus lives in a namespace.
* Max message size in service bus 256 KB
* Basic Plan has only queues. Standard and above has queues and topics.
* Service Bus has a SLA of 99,9%.
* A namespace is a container for all messaging components. Multiple queues and topics can be in a single namespace.
* Topics can have multiple subscribers not just point-to-point like queues. For integrating with topics use `TopicClient`.

