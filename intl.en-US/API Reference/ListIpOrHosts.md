# ListIpOrHosts

Queries the IP addresses of an application.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=xtrace&api=ListIpOrHosts&type=RPC&version=2019-08-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ListIpOrHosts|The operation that you want to perform. Set the value to `ListIpOrHosts`. |
|RegionId|String|Yes|cn-beijing|The ID of the region. |
|ServiceName|String|No|service1|The name of the application. If you do not set this parameter, the IP addresses of all applications in the specified region are returned. |
|StartTime|Long|No|1583683200000|The beginning of the time range to query. |
|EndTime|Long|No|1583723776974|The end of the time range to query. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|IpNames|List|\[ "172.19.XXX.XXX", "10.0.X.X" \]|The IP addresses. |
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=ListIpOrHosts
&RegionId=cn-beijing
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ListIpOrHostsResponse> 
    <RequestId>23D9757C-CB58-41A5-BBE7-2A9A0652B2F4</RequestId>  
    <IpNames> 
        <IpName>172.19.XXX.XXX</IpName>  
        <IpName>10.0.X.X</IpName> 
    </IpNames> 
</ListIpOrHostsResponse>
```

`JSON` format

```
{
  "RequestId": "23D9757C-CB58-41A5-BBE7-2A9A0652B2F4",
  "IpNames": {
    "IpName": [
      "172.19.XXX.XXX",
      "10.0.X.X"
    ]
  }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/xtrace).

