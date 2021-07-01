# SearchTraces

Queries traces by time, application name, IP address, span name, and tag.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=xtrace&api=SearchTraces&type=RPC&version=2019-08-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|SearchTraces|The operation that you want to perform. Set the value to `SearchTraces`. |
|EndTime|Long|Yes|1575622455686|The end of the time range to query. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. |
|RegionId|String|Yes|cn-beijing|The ID of the region. |
|StartTime|Long|Yes|1575561600000|The beginning of the time range to query. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. |
|ServiceName|String|No|service 1|The name of the application. |
|OperationName|String|No|/api|The name of the span. |
|MinDuration|Long|No|1000|The time more than which is used to call the trace. Unit: milliseconds. For example, a value of 100 specifies to return the traces that more than 100 milliseconds are used to call. |
|AppType|String|No|XTRACE|The type of the application. You can set the value to `XTRACE` or leave this parameter unspecified. |
|Tag.N.Key|String|No|http.status\_cod|The key of the tag. |
|Tag.N.Value|String|No|200|The value of the tag. |
|PageNumber|Integer|No|1|The number of the page to return. For example, a value of 5 indicates page 5. |
|PageSize|Integer|No|100|The number of entries to return on each page. |
|Reverse|Boolean|No|false|Specifies whether to sort the query results in chronological order or reverse chronological order. Default value: `false`. Valid values:

-   `true`: reverse chronological order
-   `false`: chronological order |
|ServiceIp|String|No|10.0.0.0|The IP address that corresponds to the span. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|PageBean|Struct| |The information about the returned page. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageSize|Integer|100|The number of entries returned per page. |
|TotalCount|Long|1000|The total number of entries returned. |
|TraceInfos|Array of TraceInfo| |The information about the trace. |
|TraceInfo| | | |
|Duration|Long|100|The time used to call the trace. Unit: milliseconds. |
|OperationName|String|/api|The name of the span. |
|ServiceIp|String|192.163.XXX.XXX|The IP address of the server where the span resides. |
|ServiceName|String|service1|The name of the application. |
|Timestamp|Long|1575561600000000|The time when the span was generated. Unit: microseconds. |
|TraceID|String|1c6881aab84191a4|The ID of the trace. |
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=SearchTraces
&EndTime=1575622455686
&RegionId=cn-beijing
&StartTime=1575561600000
&<Common request parameters>
```

Sample success responses

`XML` format

```
<SearchTracesResponse> 
    <PageBean> 
        <TotalCount>1000</TotalCount>  
        <PageSize>100</PageSize>  
        <PageNumber>1</PageNumber>  
        <TraceInfos> 
            <TraceInfo> 
                <ServiceIp>192.163.XXX.XXX</ServiceIp>  
                <ServiceName>service1</ServiceName>  
                <OperationName>/api</OperationName>  
                <TraceID>1c6881aab84191a4</TraceID>  
                <Duration>100</Duration>  
                <Timestamp>1575561600000000</Timestamp> 
            </TraceInfo> 
        </TraceInfos> 
    </PageBean>  
    <RequestId>1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED</RequestId> 
</SearchTracesResponse>
```

`JSON` format

```
{
    "PageBean": {
        "TotalCount": 1000,
        "PageSize": 100,
        "PageNumber": 1,
        "TraceInfos": {
            "TraceInfo": {
                "ServiceIp": "192.163.XXX.XXX",
                "ServiceName": "service1",
                "OperationName": "/api",
                "TraceID": "1c6881aab84191a4",
                "Duration": 100,
                "Timestamp": 1575561600000000
            }
        }
    },
    "RequestId": "1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/xtrace).

