# RAM 鉴权

在使用 RAM 账号调用阿里云 API 前，需要主账号通过创建授权策略对 RAM 账号进行授权。

## 权限策略

链路追踪支持的系统权限策略如下所示。

|权限策略|类型|说明|
|----|--|--|
|AliyunTracingAnalysisFullAccess|系统|链路追踪的完整权限|
|AliyunTracingAnalysisReadOnlyAccess|系统|链路追踪的只读权限|

以上两种权限策略都可直接访问链路追踪的 API 接口。

**相关文档**  


[借助 RAM 用户实现分权](/intl.zh-CN/访问控制/借助 RAM 用户实现分权.md)

