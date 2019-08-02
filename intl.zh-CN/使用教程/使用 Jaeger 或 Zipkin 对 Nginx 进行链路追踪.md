# 使用 Jaeger 或 Zipkin 对 Nginx 进行链路追踪 {#task_1443756 .task}

Nginx 是一款自由、开源的高性能 HTTP 服务器和反向代理服务器，对其进行链路追踪可以帮助我们更好地了解应用服务的运行状况。最近遇到一个问题，Nginx 代理的微服务出现了假死现象， 采集不到任何数据。我们没法评估这次事件造成的影响，通过在微服务的上游nginx进行跟踪，可以快速统计出影响多少访问量。本教程详细介绍了从安装 Nginx、安装 OpenTracing 插件到使用 Jaeger 或 Zipkin 对 Nginx 进行链路追踪的步骤。

## 背景信息 {#section_gnx_5mv_hpk .section}

 

## 安装 Nginx {#section_wf8_oo8_0lx .section}

请按照以下步骤安装 Nginx。

1.  下载 Nginx 源码。 

    ``` {#codeblock_has_zf4_9yp}
    wget http://nginx.org/download/nginx-1.14.2.tar.gz
    ```

2.  解压 Nginx 源码。 

    ``` {#codeblock_xsk_493_6su}
    tar -xzvf nginx-1.14.2.tar.gz
    cd nginx-1.14.2
    ```

3.  编译 Nginx 源码。 

    ``` {#codeblock_k01_bt6_2tb}
    ./configure --with-compat
    make
    sudo make install
    ```


## 安装 OpenTracing 插件 {#section_nda_j10_6dw .section}

请按照以下步骤安装 OpenTracing 插件。

1.  下载 OpenTracing 插件并解压。 

    ``` {#codeblock_v1e_o6z_dlf}
    wget https://github.com/opentracing-contrib/nginx-opentracing/releases/download/v0.7.0/linux-amd64-nginx-1.14.0-ngx_http_module.so.tgz
    tar -xzvf linux-amd64-nginx-1.14.0-ngx_http_module.so.tgz
    ```

2.  拷贝 .so 文件至 Nginx 的 modules 目录。如果不存在该目录则需要先创建。 

    ``` {#codeblock_knj_ppv_peg}
    sudo mkdir /usr/local/nginx/modules
    sudo cp ngx_http_opentracing_module.so /usr/local/nginx/modules/ngx_http_opentracing_module.so
    ```


## 方案一：用 Jaeger 进行链路追踪 {#section_g9a_8ei_5u7 .section}

您可以选择使用 Jaeger 进行链路追踪。

1.  下载 Jaeger 插件并将其拷贝至某一目录。 

    ``` {#codeblock_tnk_3xq_0iu}
    wget https://github.com/jaegertracing/jaeger-client-cpp/releases/download/v0.4.2/libjaegertracing_plugin.linux_amd64.so
    sudo cp libjaegertracing_plugin.linux_amd64_042.so /usr/local/lib/libjaegertracing.so
    ```

2.  配置 /usr/local/nginx/conf/nginx.conf 文件。 

    ``` {#codeblock_xip_12q_2tr}
    load_module modules/ngx_http_opentracing_module.so;
    
    events {}
    
    http {
      opentracing on;
    
      opentracing_load_tracer /usr/local/lib/libjaegertracing_plugin.so /etc/jaeger-config.json;
    
      server {
    
        error_log /var/log/nginx/debug.log debug;
        listen 80;
    
        location  ~ {
          opentracing_operation_name $uri;
          opentracing_trace_locations off;
          # 跳转到代理的服务，请根据需要替换
          proxy_pass http://127.0.0.1:8081;
          opentracing_propagate_context;
        }
      }
    }
    ```

    详细配置说明可参照[opentracing-contrib 配置](https://github.com/opentracing-contrib/nginx-opentracing/blob/ea9994d7135be5ad2e3009d0f270e063b1fb3b21/doc/Reference.md)。

3.  在 /etc/jaeger-config.json 文件中配置 Jaeger 参数。 

    ``` {#codeblock_yi4_v55_1es}
    {
      "service_name": "nginx",
      // 配置采样
      "sampler": {
        "type": "const",
        "param": 1
      },
      "reporter": {
        "localAgentHostPort": "localhost:6831"
      }
    }
    ```

4.  采用以下方法之一配置 Jaeger Agent。 
    -   用于自建的jaeger服务。 使用原生 Jaeger Agent。可以快速下载 [jaeger-agent](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/tools/jaeger-agent)，此时需要配置 collector.host-port。

        ``` {#codeblock_g1j_txo_fkb}
        nohup ./jaeger-agent  --collector.host-port=10.100.7.46:14267   1>1.log 2>2.log &
        ```

    -   用于阿里云的 Jaeger 托管服务。使用原生 Jaeger Agent，可以快速下载 [jaeger-agent](https://arms-apm.oss-cn-hangzhou.aliyuncs.com/tools/jaeger-agent)，并用以下参数启动 Agent，以将数据上报至链路追踪 Tracing Analysis。

        **说明：** 具体启动参数的值可以在链路追踪的概览-》查看配置-》“jaeger agent 上报“ 中查看。。

        ``` {#codeblock_tpt_buq_usf}
        // reporter.grpc.host-port 用于设置网关，网关因地域而异。 例如：
          $  nohup ./jaeger-agent --reporter.grpc.host-port=tracing-analysis-dc-sz.aliyuncs.com:1883 --jaeger.tags=Authentication=abc123@789abc_abc123f@789efg
        ```

5.  运行 Nginx 并访问 Nginx 服务。 

    ``` {#codeblock_dui_bk6_15x}
    sudo /usr/local/nginx/sbin/nginx
    curl "http://localhost"
    ```


## 方案二：用 Zipkin 进行链路追踪 {#section_cy7_2ou_88i .section}

您也可以选择使用 Zipkin 进行链路追踪。

1.  下载 Zipkin 插件并将其拷贝至某一目录。 

    ``` {#codeblock_klx_5nc_kla}
    wget https://github.com/rnburn/zipkin-cpp-opentracing/releases/download/v0.5.2/linux-amd64-libzipkin_opentracing_plugin.so.gz
    $ gunzip linux-amd64-libzipkin_opentracing_plugin.so.gz
    $ sudo cp linux-amd64-libzipkin_opentracing_plugin.so /usr/local/lib/libzipkin_opentracing_plugin.so
    ```

2.  配置 /usr/local/nginx/conf/nginx.conf 文件。 

    ``` {#codeblock_rtw_fg9_ja4}
    load_module modules/ngx_http_opentracing_module.so;
    
    events {}
    
    http {  
        opentracing on;  
        opentracing_load_tracer /usr/local/lib/libzipkin_opentracing.so /etc/zipkin-config.json;  
        server {    
              error_log /var/log/nginx/debug.log debug; 
              listen 80;    
              location  ~ {      
                     opentracing_operation_name $uri;      
                     opentracing_trace_locations off;     
                     # 跳转到代理的服务，用户根据需要替换      
                     proxy_pass http://127.0.0.1:8081;      
                     opentracing_propagate_context;    
            }
      }
    }
    ```

    详细的配置说明可参考[opentracing-contrib 配置](https://github.com/opentracing-contrib/nginx-opentracing/blob/ea9994d7135be5ad2e3009d0f270e063b1fb3b21/doc/Reference.md) 

3.  在 /etc/zipkin-config.json 文件中配置 Zipkin 参数。 

    ``` {#codeblock_n9t_lha_ze8}
    {
      "service_name": "nginx",
      "collector_host": "zipkin"
    }
    ```

4.  collector\_host配置说明。 
    -   使用自建的zipkin服务。collector\_host 可以配置成zipkin的ip即可。
    -   使用阿里云的 zipkin 托管服务

        **说明：** 请将 `collector_host` 替换成控制台概览页面的 Zipkin v1 接口，不包含 `http://` 部分。

        ``` {#codeblock_aoo_5ld_2yv}
        "collector_host": "tracing-analysis-dc-hz.aliyuncs.com/adapt_abc123@abc456_abc123@abc356/api/v1/spans?"
        ```

5.  （可选）通过 sample\_rate 配置采样比例。 

    ``` {#codeblock_c8z_pdp_n8m}
    // 10%的采样比例
    "sample_rate":0.1
    ```

6.  运行 Nginx 并访问 Nginx 服务。 

    ``` {#codeblock_ihu_vre_8nw}
    sudo /usr/local/nginx/sbin/nginx
    curl "http://localhost"
    ```


## 更多信息 {#section_jja_dow_yvs .section}

如有关于 Nginx 监控的疑问，欢迎通过钉钉账号 osfriend 交流。

[nginx-opentracing 项目](https://github.com/opentracing-contrib/nginx-opentracing)

[jaeger-client-cpp 项目](https://github.com/jaegertracing/jaeger-client-cpp)

[zipkin-cpp-opentracing](https://github.com/rnburn/zipkin-cpp-opentracing)

[Jaeger 的 Nginx Example](https://github.com/opentracing-contrib/nginx-opentracing/tree/master/example/trivial/jaeger)

[Zipkin 的 Nginx Example](https://github.com/opentracing-contrib/nginx-opentracing/tree/master/example/trivial/zipkin)

