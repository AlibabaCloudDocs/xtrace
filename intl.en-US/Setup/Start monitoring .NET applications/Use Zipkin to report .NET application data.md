# Use Zipkin to report .NET application data

Zipkin is an open-source real-time Distributed Tracking System developed and contributed by Twitter. The major functions including aggregating real-time monitoring data from various heterogeneous systems. In Tracing Analysis, you can use Zipkin to report. NET application data.





## Use the ASP. NET Core component for automatic tracking

Follow these steps to use the ASP. NET Core component for tracking.

**Note:** [Download the Demo source code](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/zipkinDotnetNetcore.zip), and run the program according to the Readme instructions.

1.  Install the NuGet Package.

    ```
    // Add the following components.
    // Zipkin4net. middleware. aspnetcore (aspnetcore middleware)
    // Zipkin4net (tracker)
    
    dotnet add package zipkin4net.middleware.aspnetcore
    dotnet add package zipkin4nete
    ```

2.  Register and launch Zipkin.

    ```
    lifetime.ApplicationStarted.Register(() => {
        TraceManager.SamplingRate = 1.0f;
        var logger = new TracingLogger(loggerFactory, &quot;zipkin4net&quot;);
        // Obtain a Zipkin Endpoint in the Tracing Analysis console. Note that the Endpoint does not contain "/api/v2/spans";.
        var httpSender = new HttpZipkinSender(&quot;http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token&quot;, &quot;application/json&quot;);
    
        var tracer = new ZipkinTracer(httpSender, new JSONSpanSerializer());
        TraceManager.RegisterTracer(tracer);
        TraceManager.Start(logger);
     });
    lifetime.ApplicationStopped.Register(() => TraceManager.Stop());
    app.UseTracing(applicationName);
    ```

3.  Add tracinghandler to the HttpClient that sends the Get/Post request.

    ```
    public override void ConfigureServices(IServiceCollection services)
    {
        services.AddHttpClient(&quot;Tracer&quot;).AddHttpMessageHandler(provider =>
            TracingHandler.WithoutInnerHandler(provider.GetService<IConfiguration>()[&quot;applicationName&quot;]));
    }
    ```


## Use the Owin component for automatic tracking

Follow these steps to use the Owin component for tracking.

**Note:** [Download the Demo source code](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/zipkinDotnetOwin.zip), and run the program according to the Readme instructions.

1.  Install the NuGet Package.

    ```
    // Add the following components.
    // Zipkin4net. middleware. aspnetcore (aspnetcore middleware)
    // Zipkin4net (tracker)
    
    dotnet add package zipkin4net.middleware.aspnetcore
    dotnet add package zipkin4nete
    ```

2.  Register and launch Zipkin.

    ```
    // Set Tracing
    TraceManager.SamplingRate = 1.0f;
    var logger = new ConsoleLogger();
    // Obtain a Zipkin Endpoint in the Tracing Analysis console. Note that the Endpoint does not contain "/api/v2/spans".
    var httpSender = new HttpZipkinSender(&quot;http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token&quot;, &quot;application/json&quot;);
    
    var tracer = new ZipkinTracer(httpSender, new JSONSpanSerializer());
    TraceManager.RegisterTracer(tracer);
    TraceManager.Start(logger);
    
    //Stop TraceManager on app dispose
    var properties = new AppProperties(appBuilder.Properties);
    var token = properties.OnAppDisposing;
    
    if (token! = CancellationToken.None)
    {
    token.Register(() =>
    {
    TraceManager.Stop();
    });
    }
    //
    
    // Setup Owin Middleware
    appBuilder.UseZipkinTracer(System.Configuration.ConfigurationManager.AppSettings[&quot;applicationName&quot;]);
    //
    						
    ```

3.  Add tracinghandler to the HttpClient that sends the Get/Post request.

    ```
    using (var httpClient = new HttpClient(new TracingHandler(applicationName)))
        {
           var response = await httpClient.GetAsync(callServiceUrl);
           var content = await response.Content.ReadAsStringAsync();
    
           await context.Response.WriteAsync(content);
        }
    ```


## Manual tracking

When reporting Java application data to the Tracking Analysis console with Zipkin, other than multiples existing plug-ins that can fulfill the purpose of tracking, you also can do tracking manually.

**Note:** [Download the Demo source code](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/zipkinDotnetManual.zip), And run the program according to the Readme instructions.

1.  Install the NuGet Package.

    ```
    // Add zipkin4net (tracker)
    
    dotnet add package zipkin4nete
    ```

2.  Register and launch Zipkin.

    ```
    TraceManager.SamplingRate = 1.0f;
    var logger = new ConsoleLogger();
    // Obtain a Zipkin Endpoint in the Tracing Analysis console. Note that the Endpoint does not contain "/api/v2/spans&quot".
    var httpSender = new HttpZipkinSender(&quot;http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token&quot;, &quot;application/json&quot;);
    
    var tracer = new ZipkinTracer(httpSender, new JSONSpanSerializer());
    TraceManager.RegisterTracer(tracer);
    TraceManager.Start(logger);
    ```

3.  Record request data.

    ```
    var trace = Trace.Create();
    
    Trace.Current = trace;
    trace.Record(Annotations.ClientSend());
    
    trace.Record(Annotations.Rpc(&quot;client&quot;));
    trace.Record(Annotations.Tag(&quot;mytag&quot;, &quot;spanFrist&quot;));
    trace.Record(Annotations.ServiceName(&quot;dotnetManual&quot;));
    //. ..do something
    testCall();
    
    trace.Record(Annotations.ClientRecv());
    ```

    **Note:**

    The preceding code is used to record the root operation of the request. To record the previous and next operations of the request, you must call the ChildOf method. Here is an example:

    ```
    var trace = Trace.Current.Child();
    Trace.Current = trace;
    trace.Record(Annotations.ServerRecv());
    trace.Record(Annotations.Rpc(&quot;server&quot;));
    trace.Record(Annotations.Tag(&quot;mytag&quot;, &quot;spanSecond&quot;));
    trace.Record(Annotations.ServiceName(&quot;dotnetManual&quot;));
    //. ..do something
    trace.Record(Annotations.ServerSend());
    ```

4.  To do troubleshooting efficiently, you can add custom tags to a record, such as records for errors and returned values of requests.

    ```
    tracer.activeSpan().setTag(&quot;http.status_code&quot;, &quot;200&quot;);
    ```

5.  When a distributed system sends an RPC request, it carries Tracing data, including TraceId, ParentSpanId, SpanId, Sampled, etc. You can use the Extract/Inject method in an HTTP Request to transparently transmit data to HTTP Request Headers. The overall process is shown as follows:

    ```
       Client Span Server Span
    ┌ ── ── ── ── ── ┐ ┌ ── ── ── ──
    Number of failed migration tasks
    │ TraceContext │ Http Request Headers │ TraceContext │
    │ ┌ ── ── ── ── ┐ │ ── ── ── ── ── ── ┐ ┌ ── ── ── ┐ │
    │ │ TraceId │ │ │ X-B3-TraceId │ │ │ TraceId │ │
    │ │ │ │ │ │ │ │ │ │ │
    │ │ ParentSpanId │ │ Inject │ X-B3-ParentSpanId │Extract │ │ ParentSpanId │ │
    │ │ ── ── ── ── ──> │ ├ ── ── ┼> │ │ │
    │ │ SpanId │ │ │ X-B3-SpanId │ │ │ SpanId │ │
    │ │ │ │ │ │ │ │ │ │ │
    │ │ Sampled │ │ │ X-B3-Sampled │ │ │ Sampled │ │
    │ └ ── ── ── ── ┘ │ ── ── ── ── ── ── ┘ └ ── ── ── ┘ │
    Number of failed migration tasks
    └ ── ── ── ── ── ┘ └ ── ── ── ──
    ```

    1.  Call the Inject method on the client to pass in Context information.

        ```
        _injector.Inject(clientTrace.Trace.CurrentSpan, request.Headers);
        ```

    2.  Call the Extract method on the server to parse the Context information.

        ```
        Ivar traceContext = traceExtractor.Extract(context.Request.Headers);
        var trace = traceContext == null? Trace.Create(): Trace.CreateFromId(traceContext);
        ```


## FAQ

Q: Why there is no data reported on the console after the Demo program is successfully executed?

A: Please check whether the Endpoint in the senderConfiguration configuration is correct.

```
// Obtain a Zipkin Endpoint in the Tracing Analysis console. Note that the Endpoint does not contain "/api/v2/spans&quot".
var httpSender = new HttpZipkinSender(&quot;http://tracing-analysis-dc-hz.aliyuncs.com/adapt_your_token&quot;, &quot;application/json&quot;);
```

[Zipkin official website](https://zipkin.io/)

[Zipkin&\#39;s dotnet client](https://github.com/openzipkin/zipkin4net)

