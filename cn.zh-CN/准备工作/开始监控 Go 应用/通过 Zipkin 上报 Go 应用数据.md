# 通过 Zipkin 上报 Go 应用数据 {#task_96334 .task}

Zipkin 是一款开源的分布式实时数据追踪系统（Distributed Tracking System），由 Twitter 公司开发和贡献。其主要功能是聚合来自各个异构系统的实时监控数据。在链路追踪 Tracing Analysis 中，您可以通过 Zipkin 上报 Go 应用数据。

 

 

## 代码埋点 {#section_o15_in2_zrb .section}

要通过 Zipkin 将 Go 应用数据上报至链路追踪控制台，首先需要完成埋点工作。

1.  添加组件依赖。 

    ``` {#codeblock_ljh_hut_i3e}
    [[constraint]]
      name = "github.com/openzipkin/zipkin-go"
      version = "0.1.1"
    
    [[constraint]]
      name = "github.com/gorilla/mux"
      version = "1.6.2"
    ```

2.  创建 Tracer。Tracer 对象可以用来创建 Span 对象（记录分布式操作时间）。Tracer 对象还配置了上报数据的网关地址、本机 IP、采样频率等数据，您可以通过调整采样率来减少因上报数据产生的开销。 

    ``` {#codeblock_zl4_g0e_od4}
    func getTracer(serviceName string, ip string) *zipkin.Tracer {
      // create a reporter to be used by the tracer
      reporter := httpreporter.NewReporter("http://tracing-analysis-dc-hz.aliyuncs.com/adapt_aokcdqnxyz@123456ff_abcdef123@abcdef123/api/v2/spans")
      // set-up the local endpoint for our service
      endpoint, _ := zipkin.NewEndpoint(serviceName, ip)
      // set-up our sampling strategy
      sampler := zipkin.NewModuloSampler(1)
      // initialize the tracer
      tracer, _ := zipkin.NewTracer(
        reporter,
        zipkin.WithLocalEndpoint(endpoint),
        zipkin.WithSampler(sampler),
      )
      return tracer;
    }
    ```

3.  记录请求数据。 

    ``` {#codeblock_fi7_mzz_rkd}
    // tracer can now be used to create spans.
    span := tracer.StartSpan("some_operation")
    // ... do some work ...
    span.Finish()
    // Output:
    ```

    **说明：** 

    以上代码用于记录请求的根操作，如果需要记录请求的上一步和下一步操作，则需要传入上下文。示例：

    ``` {#codeblock_aku_mob_t08}
    childSpan := tracer.StartSpan("some_operation2", zipkin.Parent(span.Context()))
        // ... do some work ...
    childSpan.Finish()
    ```

4.  为了快速排查问题，您可以为某个记录添加一些自定义标签，例如记录是否发生错误、请求的返回值等。 

    ``` {#codeblock_kas_d8u_uk3}
    childSpan.Tag("http.status_code", statusCode)
    ```

5.  在分布式系统中发送 RPC 请求时会带上 Tracing 数据，包括 TraceId、ParentSpanId、SpanId、Sampled 等。您可以在 HTTP 请求中使用 Extract/Inject 方法在 HTTP Request Headers 上透传数据。总体流程如下： 

    ``` {#codeblock_3gw_lxu_et1}
       Client Span                                                Server Span
    ┌──────────────────┐                                       ┌──────────────────┐
    │                  │                                       │                  │
    │   TraceContext   │           Http Request Headers        │   TraceContext   │
    │ ┌──────────────┐ │          ┌───────────────────┐        │ ┌──────────────┐ │
    │ │ TraceId      │ │          │ X-B3-TraceId      │        │ │ TraceId      │ │
    │ │              │ │          │                   │        │ │              │ │
    │ │ ParentSpanId │ │ Inject   │ X-B3-ParentSpanId │Extract │ │ ParentSpanId │ │
    │ │              ├─┼─────────>│                   ├────────┼>│              │ │
    │ │ SpanId       │ │          │ X-B3-SpanId       │        │ │ SpanId       │ │
    │ │              │ │          │                   │        │ │              │ │
    │ │ Sampled      │ │          │ X-B3-Sampled      │        │ │ Sampled      │ │
    │ └──────────────┘ │          └───────────────────┘        │ └──────────────┘ │
    │                  │                                       │                  │
    └──────────────────┘                                       └──────────────────┘
    ```

    **说明：** 目前 Zipkin 已有组件支持以 HTTP、gRPC 这两种 RPC 协议透传 Context 信息。

    1.  在客户端调用 Inject 方法传入 Context 信息。 

        ``` {#codeblock_o8k_9cc_ys9}
        req, _ := http.NewRequest("GET", "/", nil)
        // configure a function that injects a trace context into a reques
        injector := b3.InjectHTTP(req)
        injector(sp.Context())
        ```

    2.  在服务端调用 Extract 方法解析 Context 信息。 

        ``` {#codeblock_voe_o7p_fop}
        req, _ := http.NewRequest("GET", "/", nil)
                b3.InjectHTTP(req)(sp.Context())
        
                b.ResetTimer()
                _ = b3.ExtractHTTP(copyRequest(req))
        ```


## 快速入门 {#section_ibe_l1b_si5 .section}

接下来以一个示例演示如何通过 Zipkin 上报 Go 应用数据。

1.  下载[示例文件](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/zipkinTracingGoTest.zip)。
2.  在 utils.go 文件中修改上报数据的网关地址（endpointURL）。 

    **说明：** 请将 `<endpoint>` 替换成链路追踪控制台概览页面上相应客户端和相应地域的接入点。关于获取接入点信息的方法，请参见前提条件中的[获取接入点信息](#tab2)。

3.  安装依赖包。 

    ``` {#codeblock_u2x_u48_9ue}
    dep ensure
    ```

4.  运行测试程序。 

    ``` {#codeblock_bi6_bcx_e4n}
    go run main.go
    ```

5.  在[链路追踪控制台](https://tracing-analysis.console.aliyun.com/)查看上报的数据。

## 常见问题 {#section_cpl_zsy_nec .section}

问：为什么按照快速入门的步骤操作没有上报数据？

答：请检查运行过程中是否有提示，并检查 endpoint\_url 的配置是否正确。例如，错误 `failed the request with status code 403` 表明用户名或密码不正确。

[Zipkin 官网](https://zipkin.io/)

[Zipkin-go 源码](https://github.com/openzipkin/zipkin-go)

