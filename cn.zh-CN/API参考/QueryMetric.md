# QueryMetric

调用QueryMetric查询相关监控指标。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=xtrace&api=QueryMetric&type=RPC&version=2019-08-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|QueryMetric|系统规定参数，取值为`QueryMetric`。 |
|EndTime|Long|是|1575622455686|结束时间，精确到毫秒（ms）。 |
|IntervalInSec|Integer|是|60|时间间隔，单位为秒（s）。 |
|Measures.N|RepeatList|是|count|查询字段。 |
|Metric|String|是|appstat.incall|指标名称，取值：

 -   `appstat.incall`：链路统计。
-   `appstat.sql`：SQL统计。 |
|StartTime|Long|是|1575561600000|开始时间，精确到毫秒（ms）。 |
|OrderBy|String|否|count|排序字段，根据查询返回字段中的某个字段排序。 |
|Limit|Integer|否|100|返回数据条数。 |
|Filters.N.Key|String|否|http.status\_code|过滤字段的Key。 |
|Filters.N.Value|String|否|200|过滤字段的Value。 |
|Dimensions.N|RepeatList|否|RT|维度，即GroupBy分组。 |
|Order|String|否|ASC|排序，取值：

 -   `ASC`：升序。
-   `DESC`：降序。 |
|RegionId|String|否|cn-beijing|地域ID。 |
|ProxyUserId|String|否|testefgag12|代理用户ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|String|\{ "RequestId": "E2373982-D8CD-413D-B991-8EB678C01B4F", "Data": "\{\\"data\\":\[\{\\"date\\":1583686800000,\\"count\\":0,\\"rt\\":0,\\"rpc\\":\\"childSpan3\\"\}\}|返回统计信息。 |
|RequestId|String|1E2B6A4C-6B83-4062-8B6F-AEEC1FC47DED|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=QueryMetric
&EndTime=1575622455686
&IntervalInSec=60
&Measures.1=count
&Metric=appstat.incall
&StartTime=1575561600000
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<QueryMetricResponse> 
    <RequestId>E2373982-D8CD-413D-B991-8EB678C01B4F</RequestId>  
    <Data>{"data":[{"date":1583686800000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583686800000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583686800000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583686800000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583690400000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583690400000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583690400000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583690400000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583694000000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583694000000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583694000000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583694000000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583697600000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583697600000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583697600000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583697600000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583701200000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583701200000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583701200000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583701200000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583704800000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583704800000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583704800000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583704800000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583708400000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583708400000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583708400000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583708400000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583712000000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583712000000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583712000000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583712000000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583715600000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583715600000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583715600000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583715600000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583719200000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583719200000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583719200000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583719200000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583722800000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583722800000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583722800000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583722800000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583726400000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583726400000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583726400000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583726400000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583730000000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583730000000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583730000000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583730000000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583733600000,"count":0,"rt":0,"rpc":"childSpan3"},{"date":1583733600000,"count":0,"rt":0,"rpc":"childSpan2"},{"date":1583733600000,"count":0,"rt":0,"rpc":"childSpan"},{"date":1583733600000,"count":0,"rt":0,"rpc":"parentSpan"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":0.01,"rpc":"childSpan3"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":305.18,"rpc":"childSpan2"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":305.25,"rpc":"childSpan"},{"date":1583737200000,"count":100,"__time__":1583683200,"rt":306.25,"rpc":"parentSpan"}],"actualSizeFromDB":0,"intervalInSec":3600,"resultSize":0,"successful":true}</Data> 
</QueryMetricResponse>
```

`JSON`格式

```
{
  "RequestId": "E2373982-D8CD-413D-B991-8EB678C01B4F",
  "Data": "{\"data\":[{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583686800000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583690400000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583694000000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583697600000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583701200000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583704800000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583708400000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583712000000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583715600000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583719200000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583722800000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583726400000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583730000000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan3\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan2\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"childSpan\"},{\"date\":1583733600000,\"count\":0,\"rt\":0,\"rpc\":\"parentSpan\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":0.01,\"rpc\":\"childSpan3\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":305.18,\"rpc\":\"childSpan2\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":305.25,\"rpc\":\"childSpan\"},{\"date\":1583737200000,\"count\":100,\"__time__\":1583683200,\"rt\":306.25,\"rpc\":\"parentSpan\"}],\"actualSizeFromDB\":0,\"intervalInSec\":3600,\"resultSize\":0,\"successful\":true}"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/xtrace)查看更多错误码。

