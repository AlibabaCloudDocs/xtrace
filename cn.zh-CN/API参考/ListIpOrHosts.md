# ListIpOrHosts

调用ListIpOrHosts获取应用的IP地址。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=xtrace&api=ListIpOrHosts&type=RPC&version=2019-08-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListIpOrHosts|系统规定参数，取值为`ListIpOrHosts`。 |
|RegionId|String|是|cn-beijing|地域ID。 |
|ServiceName|String|否|service1|应用名称。若为空则查询该地域下所有应用的IP地址。 |
|StartTime|Long|否|1583683200000|开始时间。 |
|EndTime|Long|否|1583723776974|结束时间。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|IpNames|List|\[ "172.19.XXX.XXX", "10.0.X.X" \]|IP地址列表。 |
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListIpOrHosts
&RegionId=cn-beijing
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<ListIpOrHostsResponse> 
    <RequestId>23D9757C-CB58-41A5-BBE7-2A9A0652B2F4</RequestId>  
    <IpNames> 
        <IpName>172.19.XXX.XXX</IpName>  
        <IpName>10.0.X.X</IpName> 
    </IpNames> 
</ListIpOrHostsResponse>
```

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/xtrace)查看更多错误码。

