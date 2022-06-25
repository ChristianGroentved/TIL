# Bigquery: Restore Deleted View

To restore a deleted view you have to use the Logs Explorer. To get the last time the view was changed you can use the following query:

```shell
resource.type="bigquery_resource"
protoPayload.methodName="tableservice.update"
protoPayload.serviceData.tableUpdateRequest.resource.tableName.tableId="custom_events_attribution_VIEW"
```
`custom_events_attribution_VIEW` is the name of the view to be restored.

In the Select time range settings, set the periode during which changes could have been made to the view.
The it's only a matter of finding the last time the view was changed, using the timestamp.

It is also possible to use the Logs Explorer to find the query which created the view.

To do this you can use the following query:

```shell
resource.type="bigquery_resource"
protoPayload.methodName="tableservice.insert"
protoPayload.serviceData.tableInsertRequest.resource.tableName.tableId="query_name_VIEW"
```
Set the longest possible periode to search the logs for the required entry. If the view was created before the logs began, it won't be possible to restore the view.