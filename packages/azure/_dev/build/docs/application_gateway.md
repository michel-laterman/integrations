# Azure Application Gateway Logs

The Azure Logs integration retrieves different types of log data from Azure.

There are several requirements before using the integration since the logs will actually be read from azure event hubs.

- The logs have to be [exported first to the event hub](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create-kafka-enabled).
- To export activity logs to event hubs users can follow the steps [here](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-export).
- To export audit and sign-in logs to event hubs users can follow the steps [here](https://docs.microsoft.com/en-us/azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub).

Supported log categories:

| Log Category         | Description                                                                                                                          |
|:-------------------:|:-------------------------------------------------------------------------------------------------------------------------------------:|
| [Access log](https://learn.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics#access-log)             |  This log can be used to view Application Gateway access patterns and analyze important information. This includes the caller's IP, requested URL, response latency, return code, and bytes in and out. An access log is collected every 60 seconds. This log contains one record per instance of Application Gateway. The Application Gateway instance is identified by the instanceId property.     
| [Firewall log](https://learn.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics#firewall-log)                | This log can be used to view the requests that are logged through either detection or prevention mode of an application gateway that is configured with the web application firewall. Firewall logs are collected every 60 seconds.                                                                                                     |

## Settings

`eventhub` :
  _string_
An Event Hub is a fully managed, real-time data ingestion service. Elastic recommends using only letters, numbers, and the hyphen (-) character for Event Hub names to maximize compatibility. You can use existing Event Hubs having underscores (_) in the Event Hub name; in this case, the integration will replace underscores with hyphens (-) when it uses the Event Hub name to create dependent Azure resources behind the scenes (e.g., the storage account container to store Event Hub consumer offsets). Elastic also recommends using a separate event hub for each log type as the field mappings of each log type differ.
Default value `insights-operational-logs`.

`consumer_group` :
_string_
 The publish/subscribe mechanism of Event Hubs is enabled through consumer groups. A consumer group is a view (state, position, or offset) of an entire event hub. Consumer groups enable multiple consuming applications to each have a separate view of the event stream, and to read the stream independently at their own pace and with their own offsets.
Default value: `$Default`

`connection_string` :
_string_
The connection string is required to communicate with Event Hubs, see steps [here](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string).

A Blob Storage account is required in order to store/retrieve/update the offset or state of the eventhub messages. This means that after stopping the Azure logs package it can start back up at the spot that it stopped processing messages.

`storage_account` :
_string_
The name of the storage account where the state/offsets will be stored and updated.

`storage_account_key` :
_string_
The storage account key, this key will be used to authorize access to data in your storage account.

`resource_manager_endpoint` :
_string_
Optional, by default we are using the azure public environment, to override, users can provide a specific resource manager endpoint in order to use a different azure environment.

Resource manager endpoints:

```text
# Azure ChinaCloud
https://management.chinacloudapi.cn/

# Azure GermanCloud
https://management.microsoftazure.de/

# Azure PublicCloud 
https://management.azure.com/

# Azure USGovernmentCloud
https://management.usgovcloudapi.net/
```

## Logs

### application_gateway 

The `application_gateway` data stream of the Azure Logs package will collect any Application Gateway log events that have been streamed through an Azure event hub.

{{event "application_gateway"}}

{{fields "application_gateway"}}