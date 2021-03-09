# ListServices

调用ListServices获取应用列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=xtrace&api=ListServices&type=RPC&version=2019-08-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListServices|系统规定参数，取值为`ListServices`。 |
|RegionId|String|是|cn-beijing|地域ID。 |
|AppType|String|否|XTRACE|应用类型，取值为`XTRACE`或空。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|请求ID。 |
|Services|Array of Service| |应用列表。 |
|Service| | | |
|Pid|String|XXXqn3ly@741623b4e915df8|应用ID。 |
|RegionId|String|cn-hangzhou|地域ID。 |
|ServiceName|String|a3|应用名称。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListServices
&RegionId=cn-beijing
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/xtrace)查看更多错误码。

