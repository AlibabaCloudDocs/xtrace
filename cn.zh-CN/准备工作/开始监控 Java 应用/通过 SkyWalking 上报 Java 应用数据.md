# 通过 SkyWalking 上报 Java 应用数据 {#task_104185 .task}

在使用链路追踪控制台追踪应用的链路数据之前，需要通过客户端将应用数据上报至链路追踪。本文介绍如何通过 Skywalking 客户端上报 Java 应用数据。

打开 [SkyWalking 下载页面](http://skywalking.apache.org/downloads/#_5-x-releases)，下载 SkyWalking 6.1.0 版本（[Windows](http://www.apache.org/dyn/closer.cgi/incubator/skywalking/5.0.0-GA/apache-skywalking-apm-incubating-5.0.0-GA.zip) / [Linux](http://www.apache.org/dyn/closer.cgi/incubator/skywalking/5.0.0-GA/apache-skywalking-apm-incubating-5.0.0-GA.tar.gz)），并将解压出来的 agent 文件夹放在 Java 进程有访问权限的目录。

**警告：** 日志、插件和配置都在 agent 文件夹中，切勿改变文件夹结构。

**说明：** 插件均放置在 `/plugins` 目录中。在启动阶段将新的插件放进该目录，即可令插件生效。将插件从该目录删除，即可令其失效。另外，日志文件默输出到 `/logs` 目录中。

 

SkyWalking 是一款针对分布式系统的优秀国产 APM（Application Performance Monitoring，应用性能监控）产品，主要针对微服务、Cloud Native 和容器化（Docker、Kubernetes、Mesos）架构。SkyWalking 的核心是一个分布式追踪系统，目前已进入 Apache 孵化器。

要通过 SkyWalking 将 Java 应用数据上报至链路追踪控制台，首先需要完成埋点工作。SkyWalking 既支持自动探针（Dubbo、gRPC、JDBC、OkHttp、Spring、Tomcat、Struts、Jedis 等），也支持手动埋点（OpenTracing）。本文介绍自动埋点方法。

 

## 用 SkyWalking 为 Java 应用自动埋点 {#section_746_yt6_64f .section}

1.  打开 `config/agent.config`，配置接入点和令牌。 

    **说明：** 请将 `<endpoint>` 和 `<auth-token>` 分别替换成控制台概览页面上 SkyWalking 客户端在相应地域的接入点和鉴权令牌。关于获取方法，请参见前提条件中的[获取 SkyWalking 接入点和鉴权令牌](#tab3)。

    ``` {#codeblock_1hq_51m_rgq}
    collector.backend_service=<endpoint>
    agent.authentication=<auth-token>
    ```

2.  采用以下方法之一配置应用名称（Service Name）。 

    **说明：** 请将 `<ServiceName>` 替换为您的应用名称。如果同时采用以下两种方法，则仅第二种方法（在启动命令行中添加参数）生效。

    -   打开 `config/agent.config`，配置应用名称。

        ``` {#codeblock_42o_1re_dle}
        agent.service_name=<ServiceName>
        ```

    -   在应用程序的启动命令行中添加 -Dskywalking.agent.service\_name 参数。

        ``` {#codeblock_pra_a61_jpg}
        java -javaagent:<skywalking-agent-path> -Dskywalking.agent.service_name=<ServiceName> -jar yourApp.jar
        ```

3.  根据应用的运行环境，选择相应的方法来指定 SkyWalking Agent 的路径。 

    **说明：** 请将以下示例代码中的 `<skywalking-agent-path>` 替换为 agent 文件夹中的 skywalking-agent.jar 的绝对路径。

    -   Linux Tomcat 7 / Tomcat 8

        在 `tomcat/bin/catalina.sh` 第一行添加以下内容：

        ``` {#codeblock_tv0_vrv_axh}
        CATALINA_OPTS="$CATALINA_OPTS -javaagent:<skywalking-agent-path>"; export CATALINA_OPTS
        ```

    -   Windows Tomcat 7 / Tomcat 8

        在 `tomcat/bin/catalina.bat` 第一行添加以下内容：

        ``` {#codeblock_6l2_fva_u9i}
        set "CATALINA_OPTS=-javaagent:<skywalking-agent-path>"
        ```

    -   JAR File 或 Spring Boot

        在应用程序的启动命令行中添加 -javaagent 参数。

        **说明：** -javaagent 参数一定要在 -jar 参数之前。

        ``` {#codeblock_366_r5k_pqu}
        java -javaagent:<skywalking-agent-path> -jar yourApp.jar
        ```

    -   Jetty

        在 `{JETTY_HOME}/start.ini` 配置文件中添加以下内容：

        ``` {#codeblock_hwz_9nt_jby}
        --exec    # 去掉前面的井号取消注释
        -javaagent:<skywalking-agent-path>
        ```

4.  重新启动应用。

[SkyWalking 官网](http://skywalking.apache.org/)

[下载 SkyWalking](http://skywalking.apache.org/downloads/)

[部署 SkyWalking Java Agent](https://github.com/apache/incubator-skywalking/blob/v5.0.0-GA/docs/cn/Deploy-skywalking-agent-CN.md)

