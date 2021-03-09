# SearchTraces

调用SearchTraces查询调用链列表，可根据时间、应用名称、IP地址、Span名称、Tag等信息对调用链进行过滤查询。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=xtrace&api=SearchTraces&type=RPC&version=2019-08-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|SearchTraces|系统规定参数，取值为`SearchTraces`。 |
|EndTime|Long|是|1575622455686|结束时间，精确到毫秒（ms）。 |
|RegionId|String|是|cn-beijing|地域ID。 |
|StartTime|Long|是|1575561600000|开始时间，精确到毫秒（ms）。 |
|ServiceName|String|否|service 1|微服务名称，又称为应用名称。 |
|OperationName|String|否|/api|Span名称，即某个跟踪点或埋点的名称。 |
|MinDuration|Long|否|1000|大于某个时间跨度范围，单位为毫秒（ms）。例如，100表示大于100毫秒的数据。 |
|AppType|String|否|XTRACE|应用类型，取值为`XTRACE`或空。 |
|Tag.N.Key|String|否|http.status\_cod|标签键。 |
|Tag.N.Value|String|否|200|标签值。 |
|PageNumber|Integer|否|1|页码，例如，5表示第5页。 |
|PageSize|Integer|否|100|每页的查询数据条数。 |
|Reverse|Boolean|否|false|按照时间正序或者倒序排列。默认为`false`。取值：

 -   `true`：倒序。
-   `false` ：顺序。 |
|ServiceIp|String|否|10.0.0.0|Span对应的IP地址。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|PageBean|Struct| |返回数据的位置信息。 |
|PageNumber|Integer|1|页码。 |
|PageSize|Integer|100|每页显示的数据条数。 |
|TotalCount|Long|1000|总条数。 |
|TraceInfos|Array of TraceInfo| |返回的调用链信息。 |
|TraceInfo| | | |
|Duration|Long|100|耗时，单位为毫秒（ms）。 |
|OperationName|String|/api|Span名称。 |
|ServiceIp|String|192.163.XXX.XXX|Span所在的IP地址。 |
|ServiceName|String|service1|微服务名称，又称为应用名称。 |
|Timestamp|Long|1575561600000000|Span产生时间，单位为微秒（μs）。 |
|TraceID|String|1c6881aab84191a4|调用链ID。 |
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=SearchTraces
&EndTime=1575622455686
&RegionId=cn-beijing
&StartTime=1575561600000
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/xtrace)查看更多错误码。

