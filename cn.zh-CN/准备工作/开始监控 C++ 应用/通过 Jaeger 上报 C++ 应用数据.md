# 通过 Jaeger 上报 C++ 应用数据 {#task_90502 .task}

在使用链路追踪控制台追踪应用的链路数据之前，需要通过客户端将应用数据上报至链路追踪。本文介绍如何通过 Jaeger 客户端上报 C++ 应用数据。

 

 

## 直接上报数据 {#section_mvt_0yb_0je .section}

1.  运行以下命令，从官方网站获取 [jaeger-client-cpp](https://github.com/jaegertracing/jaeger-client-cpp)。 

    ``` {#codeblock_o27_nbb_dsf}
    wget https://github.com/jaegertracing/jaeger-client-cpp/archive/master.zip && unzip master.zip
    ```

2.  运行以下命令，编译 jaeger-client-cpp。 

    **说明：** 编译依赖 CMake，gcc 版本不低于 4.9.2。

    ``` {#codeblock_vvo_bzm_26w}
    mkdir build
        cd build
        cmake ..
        make
    ```

3.  下载原生 Jaeger Agent [jaeger-agent](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/tools/jaeger-agent)，并用以下参数启动 Agent，以将数据上报至链路追踪 Tracing Analysis。 

    **说明：** 请将 `<endpoint>` 替换成链路追踪控制台概览页面上相应客户端和相应地域的接入点。关于获取接入点信息的方法，请参见前提条件中的[获取接入点信息](#tab2)。

    ``` {#codeblock_u5n_pzf_j9j}
    // reporter.grpc.host-port 用于设置网关，网关因地域而异。 例如：
    $ nohup ./jaeger-agent --reporter.grpc.host-port=tracing-analysis-dc-sz.aliyuncs.com:1883 --jaeger.tags=<endpoint>
    ```

4.  进入 jaeger-client-cpp 的 example 目录，运行测试用例。 

    ``` {#codeblock_v1d_rta_yl7}
    ./app ../examples/config.yml
    ```

5.  登录[链路追踪控制台](https://tracing-analysis.console.aliyun.com/)，查看上报的数据。

## 通过 Agent 上报数据 {#section_7sp_5on_fqq .section}

1.  安装 Jaeger Client（[官方下载地址](https://github.com/jaegertracing/jaeger-client-cpp)）。
2.  创建 Trace。 

    例如，我们可以根据 YAML 配置生成 Trace 对象。

    ``` {#codeblock_pju_m8l_y52}
    void setUpTracer(const char* configFilePath)
    {
        auto configYAML = YAML::LoadFile(configFilePath);
        // 从 YAML 文件中导入配置
        auto config = jaegertracing::Config::parse(configYAML);
        // 设置 Trace 的 serviceName 和日志
        auto tracer = jaegertracing::Tracer::make(
            "example-service", config, jaegertracing::logging::consoleLogger());
        //将 Tracer 设置为全局变量
        opentracing::Tracer::InitGlobal(
            std::static_pointer_cast<opentracing::Tracer>(tracer));
    }
    ```

    YAML 配置参考：

    ``` {#codeblock_6c4_7oz_1yf}
    disabled: false
    reporter:
        logSpans: true
    sampler:
      type: const
      param: 1
    ```

3.  创建 Span。 

    ``` {#codeblock_giv_x1l_8bp}
    // 有 parentSpan 的情况下创建 Span
    void tracedSubroutine(const std::unique_ptr<opentracing::Span>& parentSpan)
    {
        auto span = opentracing::Tracer::Global()->StartSpan(
            "tracedSubroutine", { opentracing::ChildOf(&parentSpan->context()) });
    }
    
    // 无 parentSpan 的情况下创建 Span
    void tracedFunction()
    {
        auto span = opentracing::Tracer::Global()->StartSpan("tracedFunction");
        tracedSubroutine(span);
    }
    ```

4.  下载原生 Jaeger Agent [jaeger-agent](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/tools/jaeger-agent)，并用以下参数启动 Agent，以将数据上报至链路追踪 Tracing Analysis。 

    **说明：** 请将 `<endpoint>` 替换成链路追踪控制台概览页面上相应客户端和相应地域的接入点。关于获取接入点信息的方法，请参见前提条件中的[获取接入点信息](#tab2)。

    ``` {#codeblock_u5n_pzf_j9j}
    // reporter.grpc.host-port 用于设置网关，网关因地域而异。 例如：
    $ nohup ./jaeger-agent --reporter.grpc.host-port=tracing-analysis-dc-sz.aliyuncs.com:1883 --jaeger.tags=<endpoint>
    ```


## 更多信息 {#section_shf_l9f_zu8 .section}

