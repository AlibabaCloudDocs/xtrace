# Instrument Node. js applications

This topic explains how to instrument Node. js applications for Tracing Analysis.

Configure the dependency on the Jaeger Client in the Package of the Node project.

```
"dependencies": {
    "jaeger-client": "^3.12.0"
  }
```





## Procedure

1.  Complete the initial configuration.

    **Note:** Please replace `<endpoint>` with corresponding endpoints of clients and regions on the Overview page of Tracing Analysis console. For more information about how to obtain endpoint information, please see[Obtain access point information](#tab2).

    ```
    const initTracer = require("jaeger-client").initTracer;
    
    const config = {
        serviceName: 'node-service',
        sampler: {
            type: "const",
            param: 1
        },
        reporter: {
            collectorEndpoint: "<endpoint>"
        },
    };
    ```

2.  Create a Tracer instance object.

    ```
    const tracer = initTracer(config);
    ```

3.  Create a Span instance object.

    ```
    const span = tracer.startSpan("say-hello");
    
    // Set the tag (optional. Multiple tags are supported).
    span.setTag("tagKey-01", "tagValue-01");
    
    // Configure events (optional. Multiple events are supported).
    span.log({event: "timestamp", value: Date.now()});
    
    // Mark the end of the Span
    span.finish();
    ```

4.  Log on to the Tracing Analysis console and view traces.


## Complete example based on Express

```
const express = require("express");
const initTracer = require("jaeger-client").initTracer;
const app = express();

const config = {
    serviceName: 'node-service',
    sampler: {
        type: "const",
        param: 1
    },
    reporter: {
        collectorEndpoint: "<endpoint>"
    },
};
const tracer = initTracer(config);

app.all('*', function (req, res, next) {
    req.span = tracer.startSpan("say-hello");
    next();
});

app.get("/api", function (req, res) {
    const span = req.span;
    span.log({event: "timestamp", value: Date.now()});
    req.span.finish();
    res.send({code: 200, msg: "success"});
});

app.listen(3000, '127.0.0.1', function () {
    console.log('start');
});
```

**Related topics**  


[jaeger-client-node](https://github.com/jaegertracing/jaeger-client-node)

[opentracing-javascript](https://github.com/opentracing/opentracing-javascript)

