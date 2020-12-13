# Notes

## Compute Solution

### IAAS

### Web Apps
* We use Azure App Service Web Apps. Code is deployed using Devops pipelines. Web apps can be created using the Portal, CLI, ARM Templates or Terraform.
* Apps can be executed in different runtimes like .NET, Java, Node.js, PHP, Python or Ruby.
* Wep Apps are running inside an App Service Plan. An App Service Plan can be scaled up (more computing power) or out (more instances).

### Functions

## Storage

### Cosmos
* Azure Cosmos DB is a fully managed NoSQL database for modern app development. Single-digit millisecond response times, and automatic and instant scalability, guarantee speed at any scale.
* Cosmos DB provides five different database APIs: SQL, Cassandra, Mongo, Gramlin and Table API.

### Blob Storage

## Security

### Authentication and Authorization

### Secure Cloud Solutions
* Azure Key Vault API: Azure Key Vault can manage secrets, keys and certificates.


## Monitoring and Optimization

### Caching

### Logging
* Enable diagnostics logging for App Servive: On the menu 'App Service logs' switch on 'Application Logging' and 'Web server logging'. Select a quota and a retention period.
  Logs can be downloaded from this url: `https://<app-name>.scm.azurewebsites.net/api/dump`
* View Log Stream: The log stream can be viewed on the menu item 'Log stream'.
* Logging with Application Insights: When configuring Application Insights in a ASP.NET core app the ILogger sends warnings and above to AI sink. Read more [here](https://docs.microsoft.com/en-us/azure/azure-monitor/app/ilogger).

## Third Party Services

### Logic App

### API Management

### Event processing
* Event Grid
* Notification Hubs
* Azure Event Hub


### Messaging
