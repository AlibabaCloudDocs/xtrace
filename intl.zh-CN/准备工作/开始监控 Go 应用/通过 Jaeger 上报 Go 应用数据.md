# 通过 Jaeger 上报 Go 应用数据 {#task_1425878 .task}

在使用链路追踪控制台追踪应用的链路数据之前，需要通过客户端将应用数据上报至链路追踪。本文介绍如何通过 Jaeger 客户端上报 Go 应用数据。

 

 

## 快速开始 {#section_ej1_7jy_3ub .section}

1.  运行以下命令，在 GOPATH/src 目录下载 [Demo 文件](http://arms-apm.oss-cn-hangzhou.aliyuncs.com/tools/tracingtest.zip)。 

    ``` {#codeblock_aob_aw7_mv8}
    wget http://arms-apm.oss-cn-hangzhou.aliyuncs.com/tools/tracingtest.zip && unzip tracingtest.zip
    ```

2.  修改配置。 

    **说明：** 请将 `<endpoint>` 替换成链路追踪控制台概览页面上相应客户端和相应地域的接入点。关于获取接入点信息的方法，请参见前提条件中的[获取接入点信息](#tab2)。

    ``` {#codeblock_hte_4ir_cep}
    sender := transport.NewHTTPTransport(
            // 设置网关，网关因地域而异。
            "<endpoint>",
            )
    ```

3.  运行以下命令上传数据。 

    ``` {#codeblock_shd_6b7_ril}
    go run main.go
    http done
    grpc done
    ```

    **说明：** 

    如果出现以下错误，说明用户名和密码不正确，请更正并重试。

    ``` {#codeblock_8gf_22m_k6y}
    go run main.go
    http done
    2018/09/17 21:11:54 ERROR: error when flushing the buffer: error from collector: 403
    2018/09/17 21:11:54 ERROR: error when flushing the buffer: error from collector: 403
    ```

4.  登录[链路追踪控制台](https://tracing-analysis.console.aliyun.com/)。执行上一步骤后等待 30 秒，即可查看上报的数据。

## 直接上报数据 {#section_885_398_ab0 .section}

1.  引入 jaeger-client-go。 

    -   包路径：github.com/uber/jaeger-client-go
    -   版本号：\>=2.11.0
    以 glide 为例，您需要在 glide.yaml 中加入以下配置：

    ``` {#codeblock_lwt_v9w_eh2}
    package: github.com/uber/jaeger-client-go
    version: ^2.11.0
    subpackages:
    transport
    ```

2.  创建 Trace 对象。 

    **说明：** 请将 `<endpoint>` 替换成链路追踪控制台概览页面上相应客户端和相应地域的接入点。关于获取接入点信息的方法，请参见前提条件中的[获取接入点信息](#tab2)。

    ``` {#codeblock_ryq_eun_lr2}
    func NewJaegerTracer(service string) (opentracing.Tracer, io.Closer) {
    
        sender := transport.NewHTTPTransport(
           // 设置网关，网关因地域而异。
            "<endpoint>",
        )
       tracer, closer:= jaeger.NewTracer(service,
            jaeger.NewConstSampler(true),
       jaeger.NewRemoteReporter(sender))
       return tracer, closer
    }
    ```

3.  创建 span 实例对象和数据透传。 
    -   如果没有 parentSpan

        ``` {#codeblock_htb_jgr_i5v}
        // 创建 Span
        span := tracer.StartSpan("myspan")
        // 设置 Tag
        clientSpan.SetTag("mytag", "123")
        // 透传 traceId
        tracer.Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))
        ...
        defer  span.Finish()
        ```

    -   如果有 parentSpan

        ``` {#codeblock_bam_0r9_4o5}
        // 从 HTTP/RPC 对象解析出 spanCtx
        spanCtx, _ := tracer.Extract(opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(r.Header))
        span := tracer.StartSpan("myspan", opentracing.ChildOf(spanCtx))
        ...
        defer  span.Finish()
        ```


## 通过 Agent 上报数据 {#section_hlk_ee2_mwm .section}

1.  引入 jaeger-client-go。 

    -   包路径：github.com/uber/jaeger-client-go
    -   版本号：\>=2.11.0
    以 glide 为例，您需要在 glide.yaml 中加入以下配置：

    ``` {#codeblock_tng_g5n_xa4}
    package: github.com/uber/jaeger-client-go
    version: ^2.11.0
    subpackages:
    transport
    ```

2.  创建 Trace 对象。 

    ``` {#codeblock_om6_3yr_kg3}
    func NewJaegerTracer(serviceName string) (opentracing.Tracer, io.Closer) {
       sender, _ := jaeger.NewUDPTransport("",0)
       tracer, closer:= jaeger.NewTracer(serviceName,
            jaeger.NewConstSampler(true),
            jaeger.NewRemoteReporter(sender))
        return tracer, closer
    }
    ```

3.  下载原生 Jaeger Agent [jaeger-agent](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/tools/jaeger-agent)，并用以下参数启动 Agent，以将数据上报至链路追踪 Tracing Analysis。 

    **说明：** 请将 `<endpoint>` 替换成链路追踪控制台概览页面上相应客户端和相应地域的接入点。关于获取接入点信息的方法，请参见前提条件中的[获取接入点信息](#tab2)。

    ``` {#codeblock_y2b_ucp_enr}
    // reporter.grpc.host-port 用于设置网关，网关因地域而异。 例如：
    $ nohup ./jaeger-agent --reporter.grpc.host-port=tracing-analysis-dc-sz.aliyuncs.com:1883 --jaeger.tags=<endpoint>
    ```


## 更多信息 { .section}

