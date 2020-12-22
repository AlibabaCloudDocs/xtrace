# Use Jaeger to report data of. NET applications

Before using the tracing analysis console to trace tracing data, you need to report the data to the tracing analysis console from the client. This topic describes how to use the Jaeger client to report data to. NET applications. This method also applies to applications that are developed with C\#.



[Jaeger](https://www.jaegertracing.io/) is an open-source distributed tracing system released by Uber. Compatible with OpenTracing API, Jaeger has been widely used at Uber and has joined the [CNCF open-source organization](https://www.cncf.io/blog/2017/09/13/cncf-hosts-jaeger/). This function aggregates real-time monitoring data from multiple heterogeneous systems.

Currently, many components in the OpenTracing community support various. NET frameworks.

-   [ASP.NET Core](https://github.com/opentracing-contrib/csharp-netcore)
-   [Entity Framework Core](https://github.com/opentracing-contrib/csharp-netcore)
-   [.NET Core BCL types \(HttpClient\)](https://github.com/opentracing-contrib/csharp-netcore)
-   [gRPC](https://github.com/opentracing-contrib/csharp-netcore)



## Automatic tracking through netcore components

Follow these steps to track data through the netcore component.

**Note:** Download the [Demo source](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/jaegerDotNetDemo.zip) code, go to the webapi.dotnetcore directory, and follow the instructions in Readme to run the program.

1.  Install the NuGet Package.

    ```
    
            // Add the following components. // OpenTracing.Contrib.NetCore(aspnetcore middleware) // Jaeger(OpenTracing component) // Microsoft.Extensions.Logging.Console (logging component) dotnet add package OpenTracing.Contrib.NetCore dotnet add package Jaeger dotnet add package Microsoft.Extensions.Logging.Console 
          
    ```

2.  Use the Configure method in the Startup.cs of your project to register the middleware.

    ```
    
            // Register OpenTracing component tracking. services.AddOpenTracing(); // filter the messages returned by the Jaeger during httpclient collection. var httpOption = new HttpHandlerDiagnosticOptions(); httpOption.IgnorePatterns.Add(req => req.RequestUri.AbsolutePath.Contains("/api/traces")); services.AddSingleton(Options.Create(httpOption)); 
          
    ```

3.  Initialize and register the Jaeger.

    ```
    
            // Add Jaeger tracers. services.AddSingleton <ITracer> (serviceprovider={stringservicename=serviceProvider.GetRequiredService <IHostingEnvironment> ().ApplicationName; ILoggerFactory loggerFactory = serviceProvider.GetRequiredService <ILoggerFactory> (); Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory) // Obtain the Jaeger Endpoint from the tracing analysis console. .WithEndpoint(" http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces "); // This will log to a default localhost installation of Jaeger. var tracer = new Tracer.Builder(serviceName) .WithSampler(new ConstSampler(true)) .WithReporter(new RemoteReporter.Builder().WithSender(senderConfiguration.GetSender()).Build()) .Build(); // Allows code that can't use DI to also access the tracer. GlobalTracer.Register(tracer); return tracer; }); 
          
    ```


## Automatic tracking through the gRPC component

Follow these steps to track data through the gRPC component.

**Note:** Download the [Demo source](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/jaegerDotNetDemo.zip) code and follow Readme to run the program.

1.  Install the NuGet Package.

    ```
    
            // Add the following components. // OpenTracing.Contrib.Grpc(gRPC middleware) // Jaeger(OpenTracing component) // Microsoft.Extensions.Logging.Console (logging component) dotnet add package OpenTracing.Contrib.grpc dotnet add package Jaeger 
          
    ```

2.  Initialize the ITracer object.

    ```
    
            public static Tracer InitTracer(string serviceName, ILoggerFactory loggerFactory) { Configuration.SamplerConfiguration samplerConfiguration = new Configuration.SamplerConfiguration(loggerFactory) .WithType(ConstSampler.Type) .WithParam(1); Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory) // Obtain the Jaeger Endpoint from the tracing analysis console. .WithEndpoint(" http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces "); Configuration.ReporterConfiguration reporterConfiguration = new Configuration.ReporterConfiguration(loggerFactory) .WithSender(senderConfiguration); return (Tracer)new Configuration(serviceName, loggerFactory) .WithSampler(samplerConfiguration) .WithReporter(reporterConfiguration) .GetTracer();} 
          
    ```

3.  Tracking in the server. ServerTracingInterceptor object for tracking and binding ServerTracingInterceptor to the service.

    ```
    
            ILoggerFactory loggerFactory = new LoggerFactory().AddConsole(); ITracer tracer = TracingHelper.InitTracer("dotnetGrpcServer", loggerFactory); ServerTracingInterceptor tracingInterceptor = new ServerTracingInterceptor(tracer); Server server = new Server { Services = { Greeter.BindService(new GreeterImpl()).Intercept(tracingInterceptor) }, Ports = { new ServerPort("localhost", Port, ServerCredentials.Insecure) } }; 
          
    ```

4.  Tracking at the client. Construct the ClientTracingInterceptor object for tracking and bind the ClientTracingInterceptor to the Channel.

    ```
    
            ILoggerFactory loggerFactory = new LoggerFactory().AddConsole(); ITracer tracer = TracingHelper.InitTracer("dotnetGrpcClient", loggerFactory); ClientTracingInterceptor tracingInterceptor = new ClientTracingInterceptor(tracer); Channel channel = new Channel("127.0.0.1:50051", ChannelCredentials.Insecure); var client = new Greeter.GreeterClient(channel.Intercept(tracingInterceptor)); 
          
    ```


## Manual burial point

To report the data of. NET applications to the tracing console through Jaeger, in addition to using various existing plug-ins to implement tracking, you can also manually track the tracking.

**Note:** Download the [Demo source code](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/jaegerDotNetDemo.zip), go to the ManualDemo directory, and follow the instructions in Readme to run the program.

1.  Install the NuGet Package.

    ```
    
            // Jaeger(OpenTracing component) // Microsoft.Extensions.Logging.Console (logging component) dotnet add package Microsoft.Extensions.Logging.Console dotnet add package Jaeger 
          
    ```

2.  Create an ITracer object. ITracer is an object defined by OpenTracing. We use Jaeger to construct this object \(configuring the gateway, sample rate, and so on\).

    ```
    
            public static ITracer InitTracer(string serviceName, ILoggerFactory loggerFactory) { Configuration.SamplerConfiguration samplerConfiguration = new Configuration.SamplerConfiguration(loggerFactory) .WithType(ConstSampler.Type) .WithParam(1); Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory) // Obtain the Jaeger Endpoint from the tracing analysis console. .WithEndpoint(" http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces "); Configuration.ReporterConfiguration reporterConfiguration = new Configuration.ReporterConfiguration(loggerFactory) .WithSender(senderConfiguration); return (Tracer)new Configuration(serviceName, loggerFactory) .WithSampler(samplerConfiguration) .WithReporter(reporterConfiguration) .GetTracer();} 
          
    ```

3.  Register itreplica to GlobalTracer to facilitate code calling.

    ```
    
            GlobalTracer.Register(InitTracer("dotnetManualDemo", loggerFactory )); 
          
    ```

4.  Record the request data.

    ```
    
            ITracer tracer = GlobalTracer.Instance; ISpan span = tracer.BuildSpan("parentSpan").WithTag("mytag","parentSapn").Start(); tracer.ScopeManager.Activate(span, false); // ...do something .. span.Finish(); 
          
    ```

    **Note:** The preceding code is used to record the root operation of the request. To record the previous and next operations of the request, you need to call the AsChildOf method.

    Example:

    ```
    
            ITracer tracer = GlobalTracer.Instance; ISpan parentSpan = tracer.ActiveSpan; ISpan childSpan =tracer.BuildSpan("childSpan").AsChildOf(parentSpan).WithTag("mytag", "spanSecond").Start(); tracer.ScopeManager.Activate(childSpan, false); // ... do something... childSpan.Finish(); 
          
    ```

5.  \(Optional\) to quickly troubleshoot the problem, you can add custom tags to a record, such as an error or a response to the request.

    ```
    
            tracer.activeSpan().setTag("http.status_code", "200"); 
          
    ```

6.  When an RPC request is sent in a distributed system, Tracing data \(such as TraceId, ParentSpanId, SpanId, and Sampled\) will be attached to the request. When sending an HTTP Request, you can use the Extract/Inject method to pass data through to the HTTP Request Headers. The overall process is as follows:

    ```
    
            Client Span Server Span ┌──────────────────┐ ┌──────────────────┐ │ │ │ │ │ TraceContext │ Http Request Headers │ TraceContext │ │ ┌──────────────┐ │ ┌───────────────────┐ │ ┌──────────────┐ │ │ │ TraceId │ │ │ X-B3-TraceId │ │ │ TraceId │ │ │ │ │ │ │ │ │ │ │ │ │ │ ParentSpanId │ │ Inject │ X-B3-ParentSpanId │Extract │ │ ParentSpanId │ │ │ │ ├─┼─────────>│ ├────────┼>│ │ │ │ │ SpanId │ │ │ X-B3-SpanId │ │ │ SpanId │ │ │ │ │ │ │ │ │ │ │ │ │ │ Sampled │ │ │ X-B3-Sampled │ │ │ Sampled │ │ │ └──────────────┘ │ └───────────────────┘ │ └──────────────┘ │ │ │ │ │ └──────────────────┘ └──────────────────┘ 
          
    ```

    1.  Call the Inject method on the client to pass in Context information.

        ```
        
                  Tracer.Inject(span.Context, BuiltinFormats.HttpHeaders, new HttpHeadersInjectAdapter(request.Headers)); 
                
        ```

    2.  Call the Extract method on the server to parse Context Information.

        ```
        
                  ISpanContext extractedSpanContext = _tracer.Extract(BuiltinFormats.HttpHeaders, new RequestHeadersExtractAdapter(request.Headers)); ISpan childSpan = _tracer.BuildSpan(operationName).AsChildOf(extractedSpanContext); 
                
        ```


## FAQ

Q: Why does the Demo program fail to be executed but no data is reported from the console?

Check whether you have configured the correct Endpoint for the senderConfiguration.

```

     Configuration.SenderConfiguration senderConfiguration = new Configuration.SenderConfiguration(loggerFactory) // Obtain the Jaeger Endpoint from the tracing analysis console. .WithEndpoint(" http://tracing-analysis-dc-sz.aliyuncs.com/adapt_your_token/api/traces "); 
   
```

[Jaeger official website](https://www.jaegertracing.io/)

[jaeger-client-csharp](https://github.com/jaegertracing/jaeger-client-csharp)

[Jage r对asp.net core Component Library](https://github.com/opentracing-contrib/csharp-netcore)

[Geolocation component of the Jaeger to gRPC](https://github.com/opentracing-contrib/csharp-grpc)

