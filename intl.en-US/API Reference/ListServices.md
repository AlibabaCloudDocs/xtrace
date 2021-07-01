# ListServices

Queries applications.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=xtrace&api=ListServices&type=RPC&version=2019-08-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ListServices|The operation that you want to perform. Set the value to `ListServices`. |
|RegionId|String|Yes|cn-beijing|The ID of the region. |
|AppType|String|No|XTRACE|The type of the application. You can set the value to `XTRACE` or leave this parameter unspecified. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|The ID of the request. |
|Services|Array of Service| |The applications. |
|Service| | | |
|Pid|String|XXXqn3ly@741623b4e915df8|The ID of the application. |
|RegionId|String|cn-hangzhou|The ID of the region. |
|ServiceName|String|a3|The name of the application. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=ListServices
&RegionId=cn-beijing
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ListServicesResponse> 
    <Services> 
        <Service> 
            <ServiceName>a3</ServiceName>  
            <Pid>XXXqn3ly@741623b4e915df8</Pid>  
            <RegionId>cn-hangzhou</RegionId> 
        </Service> 
    </Services> 
</ListServicesResponse>
```

`JSON` format

```
{
  "Services": {
    "Service": [
      {
        "ServiceName": "a3",
        "Pid": "XXXqn3ly@741623b4e915df8",
        "RegionId": "cn-hangzhou"
      }
      ]
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/xtrace).

