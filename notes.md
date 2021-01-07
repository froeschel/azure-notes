# Notes

## Compute Solution

### IAAS
* When one wants to publish an image to the ACR it needs to be tagged the following way [reg-name].azurecr.io/[name-of-image].

### Web Apps
* We use Azure App Service Web Apps. Code is deployed using Devops pipelines. Web apps can be created using the Portal, CLI, ARM Templates or Terraform.
* Apps can be executed in different runtimes like .NET, Java, Node.js, PHP, Python or Ruby.
* Wep Apps are running inside an App Service Plan. An App Service Plan can be scaled up (more computing power) or out (more instances).
* App Service Plans can scale out. One could use manual scale or custom autoscale. Maual scale: Setting a predined number of instances. Autoscale: Scale based on a metric or scale to a specific instance count at a specific time. Manual scale: Simply set the instance count. 

### Functions
* Functions can be run by timer trigger. These are set by using NCRONTAB expressions. They have the format {second} {minute} {hour} {day} {month} {day-of-week}. So if you want to run the function every 3 hours it should look like this: 0 0 */3 * * *

## Storage

### Cosmos
* Azure Cosmos DB is a fully managed NoSQL database for modern app development. Single-digit millisecond response times, and automatic and instant scalability, guarantee speed at any scale.
* Pricing can be a bit complicated. Price is calculated per Request Unit (RU). RU is a 'performance currency abstracting the system resources such as CPU, IOPS, and memory'. So database operations can have a variable amount of RUs, depending on how much system resources it takes. You pay for the throughput in RU that you provision. One can have specific pricing per container or share RUs for the whole database.
* Cosmos DB provides five different database APIs: SQL, Cassandra, Mongo, Gramlin and Table API.
* A cosmos container should have a partition key. With a partition key,documents will be grouped into logical partitions. It is important to set a partition key so that all documents are evenly distributed. To find the right key canbe tricky when starting a new project.

### Blob Storage

## Security

### Authentication and Authorization

### Secure Cloud Solutions
* Azure Key Vault API: Azure Key Vault can manage secrets, keys and certificates. When using C# one can access keys by using the `KeyClient` class.


## Monitoring and Optimization

### Caching

### Logging
* Enable diagnostics logging for App Servive: On the menu 'App Service logs' switch on 'Application Logging' and 'Web server logging'. Select a quota and a retention period.
  Logs can be downloaded from this url: `https://<app-name>.scm.azurewebsites.net/api/dump`
* View Log Stream: The log stream can be viewed on the menu item 'Log stream'.
* Logging with Application Insights: When configuring Application Insights in a ASP.NET core app the ILogger sends warnings and above to AI sink. Read more [here](https://docs.microsoft.com/en-us/azure/azure-monitor/app/ilogger).

## Third Party Services

### Logic App
* Logic Apps can have following control actions [Scope, For Each, Condition, Switch, Until].

### API Management
* An API Management instance enables client applications to use OAuth 2.0 authentication when using an AAD tenant.

### Event processing
* Event Grid
* Notification Hubs
* Azure Event Hub


### Messaging
*  Azure Queue Storage queues, provide cloud messaging between components. This can be helpful when designing systems that have components which need to scale independently. To get messages from queue you can use the following code: 
