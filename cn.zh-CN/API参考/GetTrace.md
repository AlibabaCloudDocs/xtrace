# GetTrace

调用GetTrace接口获取调用链路详情。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=xtrace&api=GetTrace&type=RPC&version=2019-08-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetTrace|系统规定参数，取值为GetTrace。 |
|RegionId|String|是|cn-beijing|地域ID。 |
|TraceID|String|是|1c6881aab84191a4|调用链ID，链路请求的唯一标识。 |
|AppType|String|否|XTRACE|应用类型，取值为`XTRACE`或空。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|请求ID。 |
|Spans|Array of Span| |Span列表。 |
|Span| | | |
|Duration|Long|1000|耗时，单位为微秒（ms）。 |
|HaveStack|Boolean|false|是否有子Span。取值：

 -   `true`：有子Span。
-   `false`：无子Span。 |
|LogEventList|Array of LogEvent| |日志事件列表。 |
|LogEvent| | | |
|TagEntryList|Array of TagEntry| |标签列表。 |
|TagEntry| | | |
|Key|String|logLevel|日志事件的标签键。 |
|Value|String|Warning|日志事件的标签值。 |
|Timestamp|Long|1583683202047000|日志事件的产生时间戳。 |
|OperationName|String|/api|又称为Span名称。 |
|ParentSpanId|String|fec891bb8f8XXX|父Span ID。 |
|ResultCode|String|200|返回码。 |
|RpcId|String|1.1|表示Span之间的父子兄弟关系。 例如1.1是1.1.1的父亲Span， 而1.1.2和1.1.1是兄弟Span。 |
|ServiceIp|String|192.168.XXX.XXX|服务IP地址，即Span所在的机器IP地址。 |
|ServiceName|String|server1|服务名称，又称为应用名称。 |
|SpanId|String|fec891bb8f8XXX|Span ID。 |
|TagEntryList|Array of TagEntry| |标签列表。 |
|TagEntry| | | |
|Key|String|logLevel|Span的标签键。 |
|Value|String|Warning|Span的标签值。 |
|Timestamp|Long|1583683202047000|Span的产生时间戳。 |
|TraceID|String|1c6881aab84191a4|调用链ID，链路请求的唯一标识。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetTrace
&RegionId=cn-beijing
&TraceID=1c6881aab84191a4
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/xtrace)查看更多错误码。

