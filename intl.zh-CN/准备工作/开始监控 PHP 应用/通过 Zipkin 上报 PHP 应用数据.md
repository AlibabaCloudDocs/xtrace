# 通过 Zipkin 上报 PHP 应用数据 {#task_96187 .task}

Zipkin 是一款开源的分布式实时数据追踪系统（Distributed Tracking System），由 Twitter 公司开发和贡献。其主要功能是聚合来自各个异构系统的实时监控数据。在链路追踪 Tracing Analysis 中，您可以通过 Zipkin 上报 PHP 应用数据。

 

 

## 代码埋点 {#section_7ni_a37_sy7 .section}

要通过 Zipkin 将 PHP 应用数据上报至链路追踪控制台，首先需要完成埋点工作。

1.  安装 Zipkin 的 PHP 插件。 

    ``` {#codeblock_zb9_lqg_c43}
    composer require openzipkin/zipkin
    ```

2.  创建 Tracer。Tracer 对象可以用来创建 Span 对象（记录分布式操作时间）。Tracer 对象还配置了上报数据的网关地址、本机 IP、采样频率等数据，您可以通过调整采样率来减少因上报数据产生的开销。 

    ``` {#codeblock_tby_wcl_r0e}
    function create_tracing($endpointName, $ipv4)
    {
        $endpoint = Endpoint::create($endpointName, $ipv4, null, 2555);
        /* Do not copy this logger into production.
         * Read https://github.com/Seldaek/monolog/blob/master/doc/01-usage.md#log-levels
         */
        $logger = new \Monolog\Logger('log');
        $logger->pushHandler(new \Monolog\Handler\ErrorLogHandler());
        $reporter = new Zipkin\Reporters\Http(\Zipkin\Reporters\Http\CurlFactory::create());
        $sampler = BinarySampler::createAsAlwaysSample();
        $tracing = TracingBuilder::create()
            ->havingLocalEndpoint($endpoint)
            ->havingSampler($sampler)
            ->havingReporter($reporter)
            ->build();
        return $tracing;
    }   
    ```

3.  记录请求数据。 

    ``` {#codeblock_htd_xg9_4qi}
    $rootSpan = $tracer->newTrace();
    $rootSpan->setName('encode')
    $rootSpan->start();
    
    try {
      doSomethingExpensive();
    } finally {
      $rootSpan->finish();
    }
    ```

    **说明：** 

    以上代码用于记录请求的根操作，如果需要记录请求的上一步和下一步操作，则需要传入上下文。示例：

    ``` {#codeblock_3zw_kht_81n}
    $span = $tracer->newChild($parentSpan->getContext());
    $span->setName('encode');
    $span->start();
    try {
      doSomethingExpensive();
    } finally {
      $span->finish();
    }
    ```

4.  为了快速排查问题，您可以为某个记录添加一些自定义标签，例如记录是否发生错误、请求的返回值等。 

    ``` {#codeblock_ksl_0w1_4wx}
    $span->tag('http.status_code', $resultCode);
    ```

5.  请求上的采样率是由根操作决定的，一般在创建 Tracer 对象时会创建采样策略，但是您也可以在局部进行替换。例如： 

    ``` {#codeblock_qg2_lyx_k71}
    private function newTrace(Request $request) {
      $flags = SamplingFlags::createAsEmpty();
      if (strpos($request->getUri(), '/experimental') === 0) {
        $flags = DefaultSamplingFlags::createAsSampled();
      } else if (strpos($request->getUri(), '/static') === 0) {
        $flags = DefaultSamplingFlags::createAsSampled();
      }
      return $tracer->newTrace($flags);
    }
    ```

6.  在分布式系统中发送 RPC 请求时会带上 Tracing 数据，包括 TraceId、ParentSpanId、SpanId、Sampled 等。您可以在 HTTP 请求中使用 Extract/Inject 方法在 HTTP Request Headers 上透传数据。总体流程如下： 

    ``` {#codeblock_lfe_j75_nej}
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

        ``` {#codeblock_ebe_qc3_45x}
        // configure a function that injects a trace context into a request
        $injector = $tracing->getPropagation()->getInjector(new RequestHeaders);
        
        // before a request is sent, add the current span's context to it
        $injector->inject($span->getContext(), $request);
        ```

    2.  Extract 方法解析 Context 信息。 

        ``` {#codeblock_x3e_5k0_xw0}
        // configure a function that extracts the trace context from a request
        $extracted = $tracing->getPropagation()->extractor(new RequestHeaders);
        
        $span = $tracer->newChild($extracted)
        $span->setKind(Kind\SERVER);
        ```


## 快速入门 {#section_8x7_6om_r6i .section}

接下来以 Zipkin 的官方示例演示如何通过 Zipkin 上报 PHP 应用数据。

1.  下载[示例文件](https://github.com/openzipkin/zipkin-php-example/archive/master.zip)。
2.  在 functions.php 文件中修改上报数据的网关地址，将`$reporter = new Zipkin\Reporters\Http(\Zipkin\Reporters\Http\CurlFactory::create());` 替换为以下内容： 

    **说明：** 请将 `<endpoint>` 替换成链路追踪控制台概览页面上相应客户端和相应地域的接入点。关于获取接入点信息的方法，请参见前提条件中的[获取接入点信息](#tab2)。

    ``` {#codeblock_9kd_39k_mwr}
    reporter = new Zipkin\Reporters\Http(\Zipkin\Reporters\Http\CurlFactory::create(), 
    ['endpoint_url' => '<endpoint>
    ',]);
    ```

3.  安装依赖包。 

    ``` {#codeblock_ljk_nve_yqw}
    // As of zipkin php is still in 1.0.0-betaX
    rm composer.lock && composer install
    ```

4.  启动服务。 

    ``` {#codeblock_070_ne3_xl3}
    # 在终端 1 中执行
    composer run-frontend
    
    # 在终端 2 中执行
    composer run-backend
    ```

5.  访问前端页面上报数据，并在[链路追踪控制台](https://tracing-analysis.console.aliyun.com/)查看上报的数据。 

    ``` {#codeblock_fx7_da6_6yz}
    curl http://localhost:8081
    ```


## 常见问题 {#section_ey2_3uy_uuh .section}

问：为什么按照快速入门的步骤操作没有上报数据？

答：请检查 endpoint\_url 的配置是否正确。

## 更多信息 {#section_mrz_40b_j9s .section}

[Zipkin 官网](https://zipkin.io/)

[Zipkin-php 源码](https://github.com/openzipkin/zipkin-php)

[zipkin-php-example](https://github.com/openzipkin/zipkin-php-example)

