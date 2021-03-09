# GetTagKey

调用GetTagKey接口获取标签键。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=xtrace&api=GetTagKey&type=RPC&version=2019-08-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetTagKey|系统规定参数，取值为GetTagKey。 |
|RegionId|String|是|cn-beijing|地域ID。 |
|ServiceName|String|否|appTest|服务名称，又称为应用名称。 |
|SpanName|String|否|createOrder|Span名称，又称为Operation名称。 |
|StartTime|Long|否|1575561600000|开始时间，精确到毫秒（ms）。 |
|EndTime|Long|否|1575622455686|结束时间，精确到毫秒（ms）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|请求ID。 |
|TagKeys|List|\{"TagKey":\["date","resultCount","aTid"\]\}|标签键列表。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetTagKey
&RegionId=cn-beijing
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<GetTagKeyResponse> 
    <TagKeys> 
        <TagKey>date</TagKey>  
        <TagKey>resultCount</TagKey>  
        <TagKey>aTid</TagKey>  
    </TagKeys>  
    <RequestId>7D6519C1-A92A-4F07-AC83-706D48204242</RequestId> 
</GetTagKeyResponse>
```

`JSON`格式

```
{
	"TagKeys": {
		"TagKey": [
			"date",
			"resultCount",
			"aTid"
		]
	},
	"RequestId": "7D6519C1-A92A-4F07-AC83-706D48204242"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/xtrace)查看更多错误码。

