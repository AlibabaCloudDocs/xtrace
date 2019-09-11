# 通过 Zipkin 上报 .NET 应用数据 {#task_99881 .task}

Zipkin 是一款开源的分布式实时数据追踪系统（Distributed Tracking System），由 Twitter 公司开发和贡献。其主要功能是聚合来自各个异构系统的实时监控数据。在链路追踪 Tracing Analysis 中，您可以通过 Zipkin 上报 .NET 应用数据。

 

 

## 通过 ASP.NET Core 组件自动埋点 {#section_yjd_atq_p8t .section}

请按照以下步骤通过 ASP.NET Core 组件埋点。

**说明：** [下载 Demo 源码](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/zipkinDotnetNetcore.zip)，并按照 Readme 的说明运行程序。

1.  安装 NuGet 包。 

    ``` {#codeblock_bh4_up7_cf6}
    // 添加以下组件
    // zipkin4net.middleware.aspnetcore（aspnetcore 中间件）
    // zipkin4net（追踪器）
    
    dotnet add  package zipkin4net.middleware.aspnetcore
    dotnet add  package zipkin4nete
    ```

2.  注册和启动 Zipkin。 

    ``` {#codeblock_fha_zjj_iu9}
    lifetime.ApplicationStarted.Register(() => {
        TraceManager.SamplingRate = 1.0f;
        var logger = new TracingLogger(loggerFactory, "zipkin4net");
        // 在链路追踪控制台获取 Zipkin Endpoint,注意 Endpoint 中不包含“/api/v2/spans”。
        var httpSender = new HttpZipkinSender("http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token", "application/json");
    
        var tracer = new ZipkinTracer(httpSender, new JSONSpanSerializer());
        TraceManager.RegisterTracer(tracer);
        TraceManager.Start(logger);
     });
    lifetime.ApplicationStopped.Register(() => TraceManager.Stop());
    app.UseTracing(applicationName);
    ```

3.  在发送 Get/Post 请求的 HttpClient 中添加 tracinghandler（追踪处理者）。 

    ``` {#codeblock_c7x_n6i_4uq}
    public override void ConfigureServices(IServiceCollection services)
    {
        services.AddHttpClient("Tracer").AddHttpMessageHandler(provider =>
            TracingHandler.WithoutInnerHandler(provider.GetService<IConfiguration>()["applicationName"]));
    }
    ```


## 通过 Owin 组件自动埋点 {#section_noi_2ls_omm .section}

请按照以下步骤通过 Owin 组件埋点。

**说明：** [下载 Demo 源码](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/zipkinDotnetOwin.zip)，并按照 Readme 的说明运行程序。

1.  安装 NuGet 包。 

    ``` {#codeblock_q2h_hvw_zla}
    // 添加以下组件
    // zipkin4net.middleware.aspnetcore（aspnetcore 中间件）
    // zipkin4net（追踪器）
    
    dotnet add  package zipkin4net.middleware.aspnetcore
    dotnet add  package zipkin4nete
    ```

2.  注册和启动 Zipkin。 

    ``` {#codeblock_okw_ckh_j1a}
    // 设置 Tracing
    TraceManager.SamplingRate = 1.0f;
    var logger = new ConsoleLogger();
    // 在链路追踪控制台获取 Zipkin Endpoint,注意 Endpoint 中不包含“/api/v2/spans”。
    var httpSender = new HttpZipkinSender("http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token", "application/json");
    
    var tracer = new ZipkinTracer(httpSender, new JSONSpanSerializer());
    TraceManager.RegisterTracer(tracer);
    TraceManager.Start(logger);
    
    //Stop TraceManager on app dispose
    var properties = new AppProperties(appBuilder.Properties);
    var token = properties.OnAppDisposing;
    
    if (token != CancellationToken.None)
    {
    token.Register(() =>
    {
    TraceManager.Stop();
    });
    }
    //
    
    // Setup Owin Middleware
    appBuilder.UseZipkinTracer(System.Configuration.ConfigurationManager.AppSettings["applicationName"]);
    //
    						
    ```

3.  在发送 Get/Post 请求的 HttpClient 中添加 tracinghandler（追踪处理者）。 

    ``` {#codeblock_abc_den_yuw}
    using (var httpClient = new HttpClient(new TracingHandler(applicationName)))
        {
           var response = await httpClient.GetAsync(callServiceUrl);
           var content = await response.Content.ReadAsStringAsync();
    
           await context.Response.WriteAsync(content);
        }
    ```


## 手动埋点 {#section_msc_x19_3uz .section}

要通过 Zipkin 将 Java 应用数据上报至链路追踪控制台，除了利用各种现有插件实现埋点的目的，也可以手动埋点。

**说明：** [下载 Demo 源码](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/zipkinDotnetManual.zip)，并按照 Readme 的说明运行程序。

1.  安装 NuGet 包。 

    ``` {#codeblock_wuc_3zw_fml}
    // 添加 zipkin4net（追踪器）
    
    dotnet add  package zipkin4nete
    ```

2.  注册和启动 Zipkin。 

    ``` {#codeblock_yb6_fy2_94v}
    TraceManager.SamplingRate = 1.0f;
    var logger = new ConsoleLogger();
    // 在链路追踪控制台获取 Zipkin Endpoint,注意 Endpoint 中不包含“/api/v2/spans”。
    var httpSender = new HttpZipkinSender("http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token", "application/json");
    
    var tracer = new ZipkinTracer(httpSender, new JSONSpanSerializer());
    TraceManager.RegisterTracer(tracer);
    TraceManager.Start(logger);
    ```

3.  记录请求数据。 

    ``` {#codeblock_3ph_gz2_jxq}
    var trace = Trace.Create();
    
    Trace.Current = trace;
    trace.Record(Annotations.ClientSend());
    
    trace.Record(Annotations.Rpc("client"));
    trace.Record(Annotations.Tag("mytag", "spanFrist"));
    trace.Record(Annotations.ServiceName("dotnetManual"));
    // ...do something
    testCall();
    
    trace.Record(Annotations.ClientRecv());
    ```

    **说明：** 

    以上代码用于记录请求的根操作，如果需要记录请求的上一步和下一步操作，则需要调用 ChildOf 方法。示例：

    ``` {#codeblock_fw7_bye_vzf}
    var trace = Trace.Current.Child();
    Trace.Current = trace;
    trace.Record(Annotations.ServerRecv());
    trace.Record(Annotations.Rpc("server"));
    trace.Record(Annotations.Tag("mytag", "spanSecond"));
    trace.Record(Annotations.ServiceName("dotnetManual"));
    // ...do something
    trace.Record(Annotations.ServerSend());
    ```

4.  为了快速排查问题，您可以为某个记录添加一些自定义标签，例如记录是否发生错误、请求的返回值等。 

    ``` {#codeblock_jci_1vp_4z1}
    tracer.activeSpan().setTag("http.status_code", "200");
    ```

5.  在分布式系统中发送 RPC 请求时会带上 Tracing 数据，包括 TraceId、ParentSpanId、SpanId、Sampled 等。您可以在 HTTP 请求中使用 Extract/Inject 方法在 HTTP Request Headers 上透传数据。总体流程如下： 

    ``` {#codeblock_v3j_ey2_vm3}
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

    1.  在客户端调用 Inject 方法传入 Context 信息。 

        ``` {#codeblock_kxh_tkx_jnt}
        _injector.Inject(clientTrace.Trace.CurrentSpan, request.Headers);
        ```

    2.  在服务端调用 Extract 方法解析 Context 信息。 

        ``` {#codeblock_ni3_8p7_tn3}
        Ivar traceContext = traceExtractor.Extract(context.Request.Headers);
        var trace = traceContext == null ? Trace.Create() : Trace.CreateFromId(traceContext);
        ```


## 常见问题 {#section_l63_6ok_n4v .section}

问：Demo 程序执行成功，为什么控制台上没有上报数据？

答：请检查 senderConfiguration 配置中的 Endpoint 是否填写正确。

``` {#codeblock_eop_sen_zgx}
// 在链路追踪控制台获取 Zipkin Endpoint,注意 Endpoint 中不包含“/api/v2/spans”。
var httpSender = new HttpZipkinSender("http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token", "application/json");
```

## 更多信息 {#section_mrz_40b_j9s .section}

[Zipkin 官网](https://zipkin.io/)

[Zipkin 的 dotnet 客户端](https://github.com/openzipkin/zipkin4net)

