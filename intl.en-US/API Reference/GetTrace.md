# GetTrace

Queries the details of a trace.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=xtrace&api=GetTrace&type=RPC&version=2019-08-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetTrace|The operation that you want to perform. Set the value to GetTrace. |
|RegionId|String|Yes|cn-beijing|The ID of the region. |
|TraceID|String|Yes|1c6881aab84191a4|The unique ID of the trace. |
|AppType|String|No|XTRACE|The type of the application. You can set the value to `XTRACE` or leave this parameter unspecified. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|The ID of the request. |
|Spans|Array of Span| |The spans that the trace contains. |
|Span| | | |
|Duration|Long|1000|The time used to call the trace. Unit: microseconds. |
|HaveStack|Boolean|false|Indicates whether the span has child spans. Valid values:

 -   `true`: The span has child spans.
-   `false`: The span has no child spans. |
|LogEventList|Array of LogEvent| |The log events. |
|LogEvent| | | |
|TagEntryList|Array of TagEntry| |The tags in the log event. |
|TagEntry| | | |
|Key|String|logLevel|The tag key in the log event. |
|Value|String|Warning|The tag value in the log event. |
|Timestamp|Long|1583683202047000|The timestamp when the log event was generated. |
|OperationName|String|/api|The name of the span. |
|ParentSpanId|String|fec891bb8f8XXX|The ID of the parent span. |
|ResultCode|String|200|The status code. |
|RpcId|String|1.1|The parent-child and sibling relationship between spans. For example, span 1.1 is the parent of span 1.1.1, and span 1.1.2 and span 1.1.1 are siblings. |
|ServiceIp|String|192.168.XXX.XXX|The IP address of the server where the span resides. |
|ServiceName|String|server1|The name of the application. |
|SpanId|String|fec891bb8f8XXX|The ID of the span. |
|TagEntryList|Array of TagEntry| |The tags in the span. |
|TagEntry| | | |
|Key|String|logLevel|The tag key in the span. |
|Value|String|Warning|The tag value in the span. |
|Timestamp|Long|1583683202047000|The timestamp when the span was generated. |
|TraceID|String|1c6881aab84191a4|The unique ID of the trace. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=GetTrace
&RegionId=cn-beijing
&TraceID=1c6881aab84191a4
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetTraceResponse> 
    <RequestId>1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED</RequestId>  
    <Spans> 
        <Span> 
            <HaveStack>false</HaveStack>  
            <ParentSpanId>fec891bb8f8XXX</ParentSpanId>  
            <ServiceIp>192.168.XXX.XXX</ServiceIp>  
            <ServiceName>server1</ServiceName>  
            <OperationName>/api</OperationName>  
            <RpcId>1.1</RpcId>  
            <TraceID>1c6881aab84191a4</TraceID>  
            <Duration>1000</Duration>  
            <Timestamp>1583683202047000</Timestamp>  
            <ResultCode>200</ResultCode>  
            <SpanId>fec891bb8f8XXX</SpanId>  
            <TagEntryList> 
                <TagEntry> 
                    <Value>Warning</Value>  
                    <Key>logLevel</Key> 
                </TagEntry> 
            </TagEntryList>  
            <LogEventList> 
                <LogEvent> 
                    <Timestamp>1583683202047000</Timestamp>  
                    <TagEntryList> 
                        <TagEntry> 
                            <Value>Warning</Value>  
                            <Key>logLevel</Key> 
                        </TagEntry> 
                    </TagEntryList> 
                </LogEvent> 
            </LogEventList> 
        </Span> 
    </Spans> 
</GetTraceResponse>
```

`JSON` format

```
{
    "RequestId":"1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED",
    "Spans":{
        "Span":[
            {
                "HaveStack":"false",
                "ParentSpanId":"fec891bb8f8XXX",
                "ServiceIp":"192.168.XXX.XXX",
                "ServiceName":"server1",
                "OperationName":"/api",
                "RpcId":"1.1",
                "TraceID":"1c6881aab84191a4",
                "Duration":"1000",
                "Timestamp":"1583683202047000",
                "ResultCode":"200",
                "SpanId":"fec891bb8f8XXX",
                "TagEntryList":{
                    "TagEntry":[
                        {
                            "Value":"Warning",
                            "Key":"logLevel"
                        }
                    ]
                },
                "LogEventList":{
                    "LogEvent":[
                        {
                            "Timestamp":"1583683202047000",
                            "TagEntryList":{
                                "TagEntry":[
                                    {
                                        "Value":"Warning",
                                        "Key":"logLevel"
                                    }
                                ]
                            }
                        }
                    ]
                }
            }
        ]
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/xtrace).

