# 使用Zipkin对Nginx进行链路追踪

Nginx是一款自由的、开源的、高性能的HTTP服务器和反向代理服务器，对其进行跟踪可以帮助我们更好的了解应用服务的运行状况。本文将详细介绍Nginx的链路跟踪的安装过程。

## 教程概述

当Nginx代理的微服务出现假死现象时，因为采集不到任何数据，所以无法评估造成的影响。借助链路追踪，我们追踪微服务的上游Nginx，并快速统计出假死现象影响的访问量。

## 准备工作

1.  登录[链路追踪Tracing Analysis控制台](https://tracing-sg.console.aliyun.com/)。

2.  在左侧导航栏单击**集群配置**。

3.  在集群配置页面上单击**接入点信息**页签，在集群信息区域打开**显示Token**开关。

4.  在客户端采集工具区域单击**Zipkin**。

5.  在下方表格的相关信息列中，单击接入点信息末尾的复制图标。

    ![接入点信息-zipkin](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8470437061/p188458.png)

    **说明：** 如果应用部署于阿里云生产环境，则选择私网接入点，否则选择公网接入点。


## 操作步骤

1.  从Registry中拉取镜像。

    ```
    sudo docker pull registry.cn-hangzhou.aliyuncs.com/public-community/nginx-zipkin:0.1
    ```

2.  运行Nignx Docker。

    ```
    docker run --rm  -p 80:80 -e "COLLECTOR_HOST=${ZIPKIN_ENDPOINT}?" -d registry.cn-hangzhou.aliyuncs.com/public-community/nginx-zipkin:0.1
    ```

    `${ZIPKIN_ENDPOINT}`是[准备工作](#section_hst_9gl_yew)中保存的v1版本的Agent接入点信息。不包括“http://”部分，且以英文问号（?）结尾。

    例如：

    ```
    docker run --rm  -p 80:80 -e "COLLECTOR_HOST=tracing-analysis-dc-hz.aliyuncs.com/adapt_******_******/api/v1/spans?" -d registry.cn-hangzhou.aliyuncs.com/public-community/nginx-zipkin:0.1
    ```

3.  访问Nginx页面。

    在浏览器上访问localhost/nginx.conf或者curl "localhost/nginx.conf"。

4.  查看Nginx链路数据。

    登录[链路追踪Tracing Analysis控制台](https://tracing-sg.console.aliyun.com/)可以查看应用nginx-jaeger的链路数据。


## 在Docker上部署和跟踪Nginx

1.  下载Dockerfile并编译部署。

    ```
    wget https://arms-apm.oss-cn-hangzhou.aliyuncs.com/demo/nginx-zipkin-docker.tgz
    tar -xzvf nginx-zipkin-docker.tgz
    cd nginx-zipkin
    // 编译docker
    docker build --rm --tag nginx-jaeger:0.1 .
    ```

2.  运行Docker。

    ```
    docker run --rm  -p 80:80 -e "COLLECTOR_HOST=${ZIPKIN_ENDPOINT}?" -d nginx-zipkin:0.1
    ```

    `${ZIPKIN_ENDPOINT}`是[准备工作](#section_hst_9gl_yew)中保存的v1版本的Agent接入点信息。不包括“http://”部分，且以英文问号（?）结尾。

    例如：

    ```
    docker run --rm  -p 80:80 -e "COLLECTOR_HOST=tracing-analysis-dc-hz.aliyuncs.com/adapt_123@abc_456@efg/api/v1/spans?" -d nginx-zipkin:0.1
    ```


## 在ECS上部署和跟踪Nginx

1.  安装Nginx。

    1.  下载并解压Nginx源码。

        ```
        wget http://nginx.org/download/nginx-1.14.2.tar.gz
        tar -xzvf nginx-1.14.2.tar.gz
        ```

    2.  编译Nginx源码。

        ```
        cd nginx-1.14.2
        ./configure --with-compat
        make
        sudo make install
        ```

2.  安装OpenTracing插件。

    1.  下载OpenTracing插件并解压。

        ```
        wget https://github.com/opentracing-contrib/nginx-opentracing/releases/download/v0.7.0/linux-amd64-nginx-1.14.0-ngx_http_module.so.tgz
        tar -xzvf linux-amd64-nginx-1.14.0-ngx_http_module.so.tgz
        ```

    2.  拷贝.so文件至Nginx的modules目录。如果不存在该目录则需要先创建。

        ```
        sudo mkdir /usr/local/nginx/modules
        sudo cp ngx_http_opentracing_module.so /usr/local/nginx/modules/ngx_http_opentracing_module.so
        ```

3.  使用Zipkin进行链路追踪。

    1.  下载Zipkin插件并将其拷贝至任意工作目录。

        ```
        wget  https://github.com/rnburn/zipkin-cpp-opentracing/releases/download/v0.5.2/linux-amd64-libzipkin_opentracing_plugin.so.gz
        gunzip linux-amd64-libzipkin_opentracing_plugin.so.gz
        sudo cp linux-amd64-libzipkin_opentracing_plugin.so /usr/local/lib/libzipkin_opentracing_plugin.so 
        ```

    2.  配置/usr/local/nginx/conf/nginx.conf文件。

        ```
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
              # 跳转到代理的服务，用户根据需要替换。
              proxy_pass http://127.0.0.1:8081;
              opentracing_propagate_context;
            }
          }
        }
        ```

        **说明：** 详细配置说明，请参见[opentracing-contrib配置](https://github.com/opentracing-contrib/nginx-opentracing/blob/ea9994d7135be5ad2e3009d0f270e063b1fb3b21/doc/Reference.md)。

    3.  在/etc/zipkin-config.json文件中配置Zipkin参数。

        ```
        {
          "service_name": "nginx",
          "collector_host": "zipkin"
        }
        ```

        若使用阿里云的Zipkin托管服务，则将collector\_host配置为Zipkin接口。

        **说明：** Zipkin接口的值为[准备工作](#section_hst_9gl_yew)中保存的v1版本的Agent接入点信息，不包括“http://”部分，且以英文问号（?）结尾。

        ```
        "collector_host": "tracing-analysis-dc-hz.aliyuncs.com/adapt_abc123@abc456_abc123@abc356/api/v1/spans?"
        ```

    4.  通过sample\_rate配置采样比例。

        ```
        // 10%的采样比例
        "sample_rate":0.1
        ```

    5.  运行Nginx并访问Nginx服务。

        ```
        sudo /usr/local/nginx/sbin/nginx
        curl "http://localhost"
        ```


## 查看结果

稍等片刻后到链路追踪控制台查看，如果有监控数据则表示追踪成功。如有关于Nginx监控的疑问，欢迎通过钉钉账号osfriend交流。

[nginx-opentracing项目](https://github.com/opentracing-contrib/nginx-opentracing)

[jaeger-client-cpp项目](https://github.com/jaegertracing/jaeger-client-cpp)

[zipkin-cpp-opentracing](https://github.com/rnburn/zipkin-cpp-opentracing)

[Jaeger的Nginx Example](https://github.com/opentracing-contrib/nginx-opentracing/tree/master/example/trivial/jaeger)

[Zipkin的Nginx Example](https://github.com/opentracing-contrib/nginx-opentracing/tree/master/example/trivial/zipkin)

