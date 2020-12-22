# How do I distinguish production and test environments?

Tracing Analysis does not distinguish between production and test environments. However, you can use regions or tags to distinguish environments.

## Use regions to distinguish environments

You can select the current region or the nearest region for the production environment, and select a distant region for the test environment to differentiate the environment. For example, if you select Beijing for the production environment, select or Zhangjiakou for the Test environment.

-   Advantages: Resources in different regions are isolated from each other. This allows you to easily isolate environments.
-   Disadvantages: some VPC users of Alibaba Cloud cannot report data across regions through the intranet. In this case, we recommend that you use tags to distinguish environments.

## Use tags to distinguish environments

For applications in the same region, you only need to add a tag after the Token in the access point information \(in the format of`_<label>`\), You can achieve the following effect:

-   An application name is enclosed in brackets.`<label>`.
-   Tracing Analysis automatically creates a`<label>`Tags, which can be used to filter applications.

Assume that the application name is`Demo`, And add tags after tokens.`_ Prod`And`_ Test`, The following results can be achieved after data is reported successfully:

A Token is a red character in access point information displayed on the overview page of the Tracing Analysis console.

Add a tag after the Token`Test`Example:

-   When using the Jaeger client to report data

    -   Before adding a tag

        ```
        http://tracing-analysis-dc-hz.aliyuncs.com/adapt_abcefg123@abcefg123_abcefg456@abcefg456/api/traces
        ```

    -   After adding tags

        ```
        http://tracing-analysis-dc-hz.aliyuncs.com/adapt_abcefg123@abcefg123_abcefg456@abcefg456_test/api/traces
        ```

-   When using a Zipkin client to report data

    -   Before adding a tag

        ```
        http://tracing-analysis-dc-hz.aliyuncs.com/adapt_abcefg123@abcefg123_abcefg456@abcefg456/api/v2/spans
        ```

    -   After adding tags

        ```
        http://tracing-analysis-dc-hz.aliyuncs.com/adapt_abcefg123@abcefg123_abcefg456@abcefg456_test/api/v2/spans
        ```

-   When using the SkyWalking client to report data

    -   Before adding a tag,agent.authenticationField

        ```
        agent.authentication=abcefg123@abcefg123_abcefg456@abcefg456
        ```

    -   After adding tags,agent.authenticationField

        ```
        agent.authentication=abcefg123@abcefg123_abcefg456@abcefg456_test
        ```


[Before you begin](/intl.en-US/Setup/Before you begin.md)

[Manage apps and tags](/intl.en-US/Use the console/Application management/Manage applications and tags.md)

