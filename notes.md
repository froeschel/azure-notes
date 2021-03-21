# Notes

## Storage

* Data can be categorized into three different types: structured, semi structered and unstructured.
* Structured use SQL, semi structured use Cosmos, unstructured use Blob storage.
* Azure storage has four data services: `Blobs, Files, Queues, Tables`.
* Unmanged storage accounts must have a unique name across Azure due to REST endpoint.
* Storage aacount standard SKU is ok. For new accounts use GPv2 [GPv1 only for legacy].
* `LRS`: 3 copies in one AZ. `ZRS`: 3 copies one per AZ. `RA-GRS`: 6 copies --> 3 per region + additional Read access for distribution. `GZRS`: 6 copies --> two regions --> primary and secondary read access.
* Not all replication options are available in every region.
* `Soft delete` gives the possibility to restore deleted files within a specified retention period.
* All data written to Azure Storage is automatically encrypted.
* Always use HTTPS to encrypt data in transit.
* Storage Analytics service audits the access to the storage account. 


### Cosmos
* Azure Cosmos DB is a fully managed NoSQL database for modern app development. Single-digit millisecond response times, and automatic and instant scalability, guarantee speed at any scale.
* Pricing can be a bit complicated. Price is calculated per Request Unit (RU). RU is a 'performance currency abstracting the system resources such as CPU, IOPS, and memory'. So database operations can have a variable amount of RUs, depending on how much system resources it takes. You pay for the throughput in RU that you provision. One can have specific pricing per container or share RUs for the whole database.
* Cosmos DB provides five different database APIs: SQL, Cassandra, Mongo, Gremlin and Table API.
* A cosmos container should have a partition key. With a partition key,documents will be grouped into logical partitions. It is important to set a partition key so that all documents are evenly distributed. To find the right key canbe tricky when starting a new project.
* When creating a Cosmos database it can be set to 5 different consistency levels [Strong, Bounded staleness, Session, Consistent prefix, Eventual]. [Strong] A client will never read a uncomitted or partial write, [Bounded staleness] A read might lack behind a write, [Consistent Prefix] reads never see out-of-order writes, [Eventual] a client may read the values that are older than the ones it had read before.
* Strong consistency is important for apps where is is important that all replicated date is available at the same time and in the same order. An example could be financial transactions.
* Eventual consistency is the weakest level. It gives low latency and high troughput, but no order is guaranteed. 
* Stored precedures: Stored procedures are written using JavaScript, they can create, update, read, query, and delete items inside an Azure Cosmos container.
* Triggers: Pre-trigger are executed before modifying an item. Post-trigger after modifying an item.
* Trigger and stored procedures must be registered and called through the SDK.
* Triggers, stored procedures and UDF only works with SQL API.
* Cosmos supports two types of provisioned throughput. Manual and autoscale.
* When using autoscale you provision a max throughput.

### Blob Storage
* Three different type of blobs `Block Blob` (optimized for uploading large amounts of data), `Append Blob` (optimized for appending data e.g. logs) and `Page Blob` (optimized for VHDs.
* Blob storage offers 3 different access tiers: Hot [frequently accessed], cool [infrequently accessed] and archived [for long term backup].
* Access tiers are only available in `General Purpose V2` storage account types.
* Hot is cheap in access and more expensive in storage, where archived is expensive in access and cheap in storage. 
* Files can be moved by CLI, AzCopy or by using client library.
* A file in blob storage has metadata and properties. Metadata is user generated (docType, docClas) and properties are system generated (LastModified, BlobType).
* One can set metadata on a blob with following command `PUT https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata`.
* Blob storage is accesible by using Azure Storage REST API, Azure PowerShell, Azure CLI or client libraries in .NET, Java, Node.js, Python, Go, PHP or Ruby.
* Rules can be set so that blobs automatically can move to another access tier based ona property e.g. LastModified.
* Blob storage account URL format: `https://{accountName}blob.core.windows.net/{container}/{blobName}`.
* A storage container can only store blobs. But you can name blobs with a hierarchy structure. You then get 'virtual' folders.
* Blob leasing = prevent override or deletion.
* Create a read onlu snapshot: `https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=snapshot`. 
* Get a snapshot of a blob using query string: `https://myaccount.blob.core.windows.net/mycontainer/myblob?snapshot=<DateTime>`.
* A `Lease` will set a lock on a specific blob.

## Monitoring and Optimization

### Caching
* Cache expiration policies for FrontDoor, CDNs, or Redis caches store.
* CDNs can be created by using offers from Microsoft, Verizon or Akamai. Verizon also offers a premium version.
* The premium version allows to use mobile optimization.
* Different types of optimization (based on provider)
* `Genaral web delivery` used for genearal web apps. Can also be used ofr video streaming.
* `Dynamic Site Acceleration` used for dynamic content. 
*  Microsoft delivers `Dynamic Site Acceleration` via the `Front Door Service`.
* `General Media Streaming` for live streaming and on demand video.
* `Video-on-demand media streaming` for on demand streaming.
* `Large file download` for files larger than 10 MB.
* One CDN profile has one or more endpoints.
* When creating a CDN endpoint you add the endpoint url and the origin hostname url. 
* CDN endpoints can have custom domain.
* When origin content changes, purge CDN.
* Azure CDN caching rules: Bypass cache [do not cache], Override [override origin cache], Set if missing [use own if origin missing].
* Query string caching: Ignore query strings [deault, cache in POP node], Bypass caching for query strings [do not cache query strings at all], Cache every unique URL [every url set as an unique asset].
* Get information from Redis: var info = `database.Execute("INFO");`.
* Some Redis commands: `SET` sets a value in a key, `SETEX` sets value and a key and a expiration time, `EXPIRE` sets a key to expire, `EXISTS` check if a key exists in Redis.
* Redis connection string can be found in the portal.  


### Logging
* Enable diagnostics logging for App Servive: On the menu 'App Service logs' switch on 'Application Logging' and 'Web server logging'. Select a quota and a retention period.
  Logs can be downloaded from this url: `https://<app-name>.scm.azurewebsites.net/api/dump`
* View Log Stream: The log stream can be viewed on the menu item 'Log stream'.
* Logging with Application Insights: When configuring Application Insights in a ASP.NET core app the ILogger sends warnings and above to AI sink. Read more [here](https://docs.microsoft.com/en-us/azure/azure-monitor/app/ilogger).
* In Azure Monitor one can get can overview of all the system metrics and AppInsights logs that have been collected.
* It is possible to query the data with the Kusto query language.
* Transient fault is a temporary error that can occur in your cloud set up (e.g. network connection lost).
* Use `SqlAzureExecutionStrategy` to handle transient faults. Can by used manually `executionStrategy.Execute(() =>` or be set in the DBConfiguration class.
* Another idea would be to use queues to decouple the components from eachother.

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
* In APIM there are versions and revisions.
* Updating to a new version when breaking changes are introduced for the consumer of the API.
* A revision makes sure the API is still usable. You do not need to publish changes. With that you can test new functions in the API without breaking the current implementation.
* A version can have multiple revisions.
* When a revision has been tested you can create a version out of it.

### Event processing
* Event Grid: For reacting to status changes in event driven architecture.
* With Event Grid you can create a a subscription for an event so that you get notified when changes happen.
* Register event grid as resource provider: `az provider register --namespace Microsoft.EventGrid`. 
* Azure Event Hub: For streaming and telemetry of distributed data. 
* There are three pricing tiers for Azure Event Hubs: Basic, Standard, and Dedicated. The tiers differ in terms of supported connections, the number of available Consumer groups, and throughput [from docs site].
* Namespaces need a unique name. The have the url pattern `namespace.servicebus.windows.net`.
* EventHub partition count needs to be equal to number of consumers.
* CLI command for eventhubs: `az eventhubs`.
* Event Hub connection string format: `Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>`
* Event Hub send events: Use the `EventHubClient` class. It has the method `SendAsync(EventData event)` where EventData takes a byte array. 
* Event Hub process events: Use the  `IEventProcessor` interface. It has a method called  `ProcessEventAsync`. 
* The class `EventProcessorHost ` can also be used for event processing.
* Event hub max event size is 1 MB.
* Notification hub: Push engine to send notifications to different platforms (iOS, Android, Windows).
* Notification Hubs: Two resource levels --> hubs and namespaces. Hub is a push resource for one app. A namespace is a collection of hubs in one region.


### Messaging
* Queues consists of orederd timestamped messages. Integrate with queues using the `QueueClient`.
* Use `client.SendAsync(new Message(Encoding.UTF8.GetBytes(message)))` to send messages to the Service Bus.
*  Azure Queue Storage queues, provide cloud messaging between components. This can be helpful when designing systems that have components which need to scale independently. To get messages from queue you can use the following code: 
* The max size for a message in Azure Queue Storage is 64 KB.
* Storage queue can handle 2000 messages/second.
* Max size of a queue is 500 TB.
* Queues can respond to high demand instantaneously.
* Access to queues can be authourized by Azure AD, an acoount key or shared access signitures.
* Azure Service Bus: Used to send messages between system components. These messages are consumed elsewhere in the system.
* Service Bus has more features than storage queue.
* Azure Service Bus can exchange messages in three different ways: queues, topics, and relays.
* Service Bus lives in a namespace.
* Max message size in service bus 256 KB. In the premium SKU 1 MB.
* Basic Plan has only queues. Standard and above has queues and topics.
* Service Bus has a SLA of 99,9%.
* A namespace is a container for all messaging components. Multiple queues and topics can be in a single namespace.
* Topics can have multiple subscribers not just point-to-point like queues. For integrating with topics use `TopicClient`.

