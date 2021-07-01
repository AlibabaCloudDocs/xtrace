# GetTagKey

Queries tag keys.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=xtrace&api=GetTagKey&type=RPC&version=2019-08-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetTagKey|The operation that you want to perform. Set the value to GetTagKey. |
|RegionId|String|Yes|cn-beijing|The ID of the region. |
|ServiceName|String|No|appTest|The name of the application. |
|SpanName|String|No|createOrder|The name of the span. |
|StartTime|Long|No|1575561600000|The beginning of the time range to query. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. |
|EndTime|Long|No|1575622455686|The end of the time range to query. Specify the time in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|The ID of the request. |
|TagKeys|List|\{"TagKey":\["date","resultCount","aTid"\]\}|The tag keys. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=GetTagKey
&RegionId=cn-beijing
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/xtrace).

