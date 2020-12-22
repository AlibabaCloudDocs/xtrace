# Use Jaeger to report .NET application data

Before using Tracing Analysis, you must report application data to Tracing Analysis through the client. This topic explains how to report .NET application data with Jaeger client.



[Jaeger](https://www.jaegertracing.io/) is an open-source distributed tracing system from Uber. It is compatible with OpenTracing APIs and has been used on a large scale by Uber.[It has officially joined CNCF](https://www.cncf.io/blog/2017/09/13/cncf-hosts-jaeger/). The major function is aggregating real-time monitoring data from various heterogeneous systems.

Currently, the OpenTracing community has many components that support various .NET frameworks, for example:

-   [ASP.NET Core](https://github.com/opentracing-contrib/csharp-netcore)
-   [Entity Framework Core](https://github.com/opentracing-contrib/csharp-netcore)
-   [. NET Core BCL types \(HttpClient\)](https://github.com/opentracing-contrib/csharp-netcore)
-   [gRPC](https://github.com/opentracing-contrib/csharp-netcore)



## Use the netcore component for automatic instrumentation

Follow these steps to use the netcore component for instrumentation.

**Note:** [Download the Demo source code](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/jaegerDotNetDemo.zip), and go to the webapi.dotnetcore directory to run the program according to the Readme instructions.

1.  Install the NuGet Package.

    ```
    // Add the following components.
    // Opentring.contact.net Core (aspnetcore middleware)
    // Jaeger (implementation component of OpenTracing)
    // Microsoft. Logging. Logging. Console (log component)
    
    dotnet add package OpenTracing.Contrib.NetCore
    dotnet add package Jaeger
    dotnet add package Microsoft.Extensions.Logging.Console
    ```

2.  Use the Configure method in Startup.cs to register the middleware.

    ```
    // Register the OpenTracing component tracking
    services.AddOpenTracing();
    // Filter the Jaeger data reported by the httpclient.
    var httpOption = new HttpHandlerDiagnosticOptions();
    httpOption.IgnorePatterns.Add(req => req.RequestUri.AbsolutePath.Contains("/api/traces"));
    services.AddSingleton(Options.Create(httpOption));
    ```

3.  Initialize and register Jaeger.

    ```
    // Add Jaeger Tracer
    services.AddSingleton<ITracer>(serviceProvider =>
    {
        string serviceName = serviceProvider.GetRequiredService<IHostingEnvironment>().ApplicationName;
        ILoggerFactory loggerFactory = serviceProvider.GetRequiredService<ILoggerFactory>();
         Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory)
         // Obtain the Jaeger Endpoint in the Trace Analysis console.
              .WithEndpoint("http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces");
    
          // This will log to a default localhost installation of Jaeger.
          var tracer = new Tracer.Builder(serviceName)
              .WithSampler(new ConstSampler(true))
              .WithReporter(new RemoteReporter.Builder().WithSender(senderConfiguration.GetSender()).Build())
               .Build();
    
          // Allows code that can't use DI to also access the tracer.
          GlobalTracer.Register(tracer);
    
          return tracer;
    });
    ```


## Use the gRPC component for automatic instrumentation

Following These steps to use the gRPC component for instrumentation.

**Note:** [Download the Demo source code](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/jaegerDotNetDemo.zip), and run the program according to the Readme instructions.

1.  Install the NuGet Package.

    ```
    // Add the following components.
    // OpenTracing.Contrib.Grpc (gRPC middleware)
    // Jaeger (implementation component of OpenTracing)
    // Microsoft.Extensions.Logging.Console (log component)
    
    dotnet add package OpenTracing.Contrib.grpc
    dotnet add package Jaeger
    ```

2.  Initialize the ITracer object.

    ```
    public static Tracer InitTracer(string serviceName, ILoggerFactory loggerFactory)
            {
                Configuration.SamplerConfiguration samplerConfiguration = new Configuration.SamplerConfiguration(loggerFactory)
                    .WithType(ConstSampler.Type)
                    .WithParam(1);
                Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory)
                        // Obtain the Jaeger Endpoint in the Tracing Analysis console.
                       .WithEndpoint("http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces");
                Configuration.ReporterConfiguration reporterConfiguration = new Configuration.ReporterConfiguration(loggerFactory)
                    .WithSender(senderConfiguration);
    
                return (Tracer)new Configuration(serviceName, loggerFactory)
                    .WithSampler(samplerConfiguration)
                    .WithReporter(reporterConfiguration)
                    .GetTracer();
            }
    ```

3.  Instrument the server. Create a ServerTracingInterceptor object for tracking and bind ServerTracingInterceptor to the service.

    ```
    ILoggerFactory loggerFactory = new LoggerFactory().AddConsole();
    ITracer tracer = TracingHelper.InitTracer("dotnetGrpcServer", loggerFactory);
    ServerTracingInterceptor tracingInterceptor = new ServerTracingInterceptor(tracer);
    Server server = new Server
     {
        Services = { Greeter.BindService(new GreeterImpl()).Intercept(tracingInterceptor) },
         Ports = { new ServerPort("localhost", Port, ServerCredentials.Insecure) }
     };
    ```

4.  Instrument the client. Create a ClientTracingInterceptor object for tracking and bind ClientTracingInterceptor to the Channel.

    ```
    ILoggerFactory loggerFactory = new LoggerFactory().AddConsole();
    ITracer tracer = TracingHelper.InitTracer("dotnetGrpcClient", loggerFactory);
    ClientTracingInterceptor tracingInterceptor = new ClientTracingInterceptor(tracer);
    Channel channel = new Channel("127.0.0.1:50051", ChannelCredentials.Insecure);
    
    var client = new Greeter.GreeterClient(channel.Intercept(tracingInterceptor));
    ```


## Manual instrumentation

When reporting .Net application data to the Tracking Analysis console with Jaeger, other than multiple existing plug-ins that can fulfill the purpose of instrumentation, you also can instrument manually.

**Note:** [Download the Demo source code](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/jaegerDotNetDemo.zip), go to the terminaldemo directory, and run the program according to the Readme instructions.

1.  Install the NuGet Package.

    ```
    // Jaeger (implementation component of OpenTracing)
    // Microsoft.Extensions.Logging.Console (log component)
    
    dotnet add package Microsoft.Extensions.Logging.Console
    dotnet add package Jaeger
    ```

2.  Construct an ITracer object. ITracer is an object defined by OpenTracing. We use Jaeger to build this object \(configure the gateway, sampling rate, and so on\).

    ```
    public static ITracer InitTracer(string serviceName, ILoggerFactory loggerFactory)
     {
        Configuration.SamplerConfiguration samplerConfiguration = new Configuration.SamplerConfiguration(loggerFactory)
              .WithType(ConstSampler.Type)
              .WithParam(1);
        Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory)
               // Obtain the Jaeger Endpoint in the Tracing Analysis console.
              .WithEndpoint("http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces");
    
       Configuration.ReporterConfiguration reporterConfiguration = new Configuration.ReporterConfiguration(loggerFactory)
              .WithSender(senderConfiguration);
    
       return (Tracer)new Configuration(serviceName, loggerFactory)
              .WithSampler(samplerConfiguration)
              .WithReporter(reporterConfiguration)
            .GetTracer();
    }
    ```

3.  Register ITreacer to GlobalTracer in order to facilitate code calling.

    ```
    GlobalTracer.Register(InitTracer("dotnetManualDemo", loggerFactory ));
    ```

4.  Record request data.

    ```
    ITracer tracer = GlobalTracer.Instance;
    ISpan span = tracer.BuildSpan("parentSpan").WithTag("mytag","parentSapn").Start();
    tracer.ScopeManager.Activate(span, false);
    // ...do something
    ..
    span.Finish();
    ```

    **Note:** The preceding code is used to record the root operation of the request. To record the previous and next operations of the request, you must call the AsChildOf method. Here is an example:

    ```
    ITracer tracer = GlobalTracer.Instance;
    ISpan parentSpan = tracer.ActiveSpan;
    ISpan childSpan =tracer.BuildSpan("childSpan").AsChildOf(parentSpan).WithTag("mytag", "spanSecond").Start();
    tracer.ScopeManager.Activate(childSpan, false);
    // ... do something
    ...
    childSpan.Finish();
    ```

5.  \(Optional\) To do troubleshooting efficiently, you can add custom tags to a record, such as records for errors and returned values of requests.

    ```
    tracer.activeSpan().setTag("http.status_code", "200");
    ```

6.  When a distributed system sends an RPC request, it carries Tracing data, including TraceId, ParentSpanId, SpanId, Sampled, etc. You can use the Extract/Inject method in an HTTP Request to transparently transmit data to HTTP Request Headers. The overall process is as follows:

    ```
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

    1.  Call the Inject method on the client to pass in Context information.

        ```
        Tracer.Inject(span.Context, BuiltinFormats.HttpHeaders, new HttpHeadersInjectAdapter(request.Headers));
        ```

    2.  Call the Extract method on the server to parse the Context information.

        ```
        ISpanContext extractedSpanContext = _tracer.Extract(BuiltinFormats.HttpHeaders, new RequestHeadersExtractAdapter(request.Headers));
        ISpan childSpan = _tracer.BuildSpan(operationName).AsChildOf(extractedSpanContext);
        ```


## FAQ

Q: Why there is no data reported on the console after the Demo program is successfully executed?

A: Please check whether the Endpoint in the senderConfiguration configuration is correct.

```
Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory)
           // Obtain the Jaeger Endpoint in the Tracing Analysis console.
          .WithEndpoint("http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces");
```

**Related topics**  


[Jaeger official website](https://www.jaegertracing.io/)

[jaeger-client-csharp](https://github.com/jaegertracing/jaeger-client-csharp)

[Jaeger&\#39;s asp.net core Component Library](https://github.com/opentracing-contrib/csharp-netcore)

[Jaeger tracking component for gRPC](https://github.com/opentracing-contrib/csharp-grpc)

