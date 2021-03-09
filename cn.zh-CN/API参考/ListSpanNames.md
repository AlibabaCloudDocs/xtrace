# ListSpanNames

调用ListSpanNames接口查询Span名称列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=xtrace&api=ListSpanNames&type=RPC&version=2019-08-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListSpanNames|系统规定参数，取值为ListSpanNames。 |
|RegionId|String|是|cn-beijing|地域ID。 |
|ServiceName|String|否|service 1|服务名称，又称为应用名称。 |
|StartTime|Long|否|1575561600000|开始时间，精确到毫秒（ms）。 |
|EndTime|Long|否|1575622455686|结束时间，精确到毫秒（ms）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|请求ID。 |
|SpanNames|List|\{"SpanName":\["rpc0","rpc1.1","rpc1.1.1"\]\}|Span名称列表。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListSpanNames
&RegionId=cn-beijing
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<ListSpanNamesResponse> 
    <SpanNames> 
        <SpanName>rpc0</SpanName>  
        <SpanName>rpc1.1</SpanName>  
        <SpanName>rpc1.1.1</SpanName> 
    </SpanNames>  
    <RequestId>79C84C64-9951-477E-96F3-7FA44128C601</RequestId> 
</ListSpanNamesResponse>
```

`JSON`格式

```
{
  "SpanNames": {
    "SpanName": [
      "rpc0",
      "rpc1.1",
      "rpc1.1.1"]
      },
  "RequestId": "79C84C64-9951-477E-96F3-7FA44128C601"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/xtrace)查看更多错误码。

