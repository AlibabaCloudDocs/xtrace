# QueryMetric

Queries a monitoring metric.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=xtrace&api=QueryMetric&type=RPC&version=2019-08-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|QueryMetric|The operation that you want to perform. Set the value to `QueryMetric`. |
|EndTime|Long|Yes|1575622455686|The end of the time range to query. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. |
|IntervalInSec|Integer|Yes|60|The time interval at which the metric was queried. Unit: seconds. |
|Measures.N|RepeatList|Yes|count|The field to be queried. |
|Metric|String|Yes|appstat.incall|The name of the metric. Valid values:

 -   `appstat.incall`: trace statistics
-   `appstat.sql`: SQL statistics |
|StartTime|Long|Yes|1575561600000|The beginning of the time range to query. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. |
|OrderBy|String|No|count|The field used to sort the returned entries. |
|Limit|Integer|No|100|The number of entries returned per page. |
|Filters.N.Key|String|No|http.status\_code|The key of the field used to filter the returned entries. |
|Filters.N.Value|String|No|200|The value of the field used to filter the returned entries. |
|Dimensions.N|RepeatList|No|RT|The field used to group the returned entries. |
|Order|String|No|ASC|The order for sorting. Valid values:

 -   `ASC:`: ascending order.
-   `DESC`: descending order. |
|RegionId|String|No|cn-beijing|The ID of the region. |
|ProxyUserId|String|No|testefgag12|The ID of the proxy user. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Data|String|\{ "RequestId": "E2373982-D8CD-413D-B991-8EB678C01B4F", "Data": "\{\\"data\\":\[\{\\"date\\":1583686800000,\\"count\\":0,\\"rt\\":0,\\"rpc\\":\\"childSpan3\\"\}\}|The statistics. |
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=QueryMetric
&EndTime=1575622455686
&IntervalInSec=60
&Measures.1=count
&Metric=appstat.incall
&StartTime=1575561600000
&<Common request parameters>
```

Sample success responses

`XML` format

```
<QueryMetricResponse> 
    <RequestId>E2373982-D8CD-413D-B991-8EB678C01B4F</RequestId>  
    <Data>{"data":[{"date":1583686800000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583686800000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583686800000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583686800000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583690400000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583690400000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583690400000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583690400000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583694000000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583694000000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583694000000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583694000000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583697600000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583697600000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583697600000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583697600000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583701200000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583701200000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583701200000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583701200000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583704800000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583704800000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583704800000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583704800000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583708400000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583708400000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583708400000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583708400000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583712000000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583712000000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583712000000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583712000000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583715600000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583715600000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583715600000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583715600000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583719200000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583719200000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583719200000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583719200000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583722800000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583722800000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583722800000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583722800000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583726400000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583726400000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583726400000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583726400000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583730000000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583730000000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583730000000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583730000000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583733600000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583733600000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583733600000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583733600000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":0.01,"rpc":"childSpan3"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":305.18,"rpc":"childSpan2"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":305.25,"rpc":"childSpan"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":306.25,"rpc":"parentSpan"}],"actualSizeFromDB":0,"intervalInSec":3600,"resultSize":0,"successful":true}</Data> 
</QueryMetricResponse>
```

`JSON` format

```
{
  "RequestId": "E2373982-D8CD-413D-B991-8EB678C01B4F",
  "Data": "{\"data\":[{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":0.01,\"rpc\":\"childSpan3\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":305.18,\"rpc\":\"childSpan2\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":305.25,\"rpc\":\"childSpan\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":306.25,\"rpc\":\"parentSpan\"}],\"actualSizeFromDB\":0,\"intervalInSec\":3600,\"resultSize\":0,\"successful\":true}"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/xtrace).

