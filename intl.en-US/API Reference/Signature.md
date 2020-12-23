# Signature

You must sign all API requests to ensure security. Each API request must contain the signature, regardless of whether the request is sent over HTTP or HTTPS.

## Overview

You must add the signature to the PolarDB-X API request in the following format:

```

     https:// 
    
      Endpoint 
    /? 
    
      SignatureVersion 
    =1.0& 
    
      SignatureMethod 
    =HMAC-SHA1&Signature=CT9X0VtwR86fNWSnsc6v8YGOjuE%3D& 
    
      SignatureNonce 
    =3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf 
   
```

In the preceding information:

-   SignatureMethod: the signature method. Currently, HMAC-SHA1 is supported.
-   SignatureVersion: the version of the signature algorithm. Set the value to 1.0.
-   SignatureNonce: a unique, random number used to prevent replay attacks. You must use different random numbers for different requests. We recommend that you use universally unique identifiers \(UUIDs\).
-   Signature: the Signature generated by performing symmetric encryption on the request by using the AccessKey Secret.

## Signature calculation

The signature algorithm complies with the HMAC-SHA1 specifications in RFC 2104. The AccessKey secret is used to calculate the Hash-based Message Authentication Code \(HMAC\) value of an encoded and formatted query string. The HMAC value is then used as the signature. Some parameters in a request are used to calculate the signature. Therefore, the signature of a request varies based on the API request parameters.

```

     Signature = Base64( HMAC-SHA1( AccessSecret, UTF-8-Encoding-Of( StringToSign)) ) 
   
```

To calculate a signature, follow these steps:

1.  Compose and encode a string-to-sign.
    1.  Use the request parameters to construct a canonicalized query string:
        1.  The request parameters are ordered alphabetically by the request all the request parameters \(including the public request parameters and custom parameters for the API, but does not include a common request parameters in Signature parameters\).

            If you use the GET method to submit the request, these parameters are the part located after the question mark \(?\) and connected by the ampersands \(&\) in the request uniform resource identifier \(URI\).

        2.  Encode in UTF-8 the parameters and their values in the request URL. The following table describes the encoding rules.

            |Character|Encoding rule|
            |---------|-------------|
            |Uppercase letters \(A-Z\), lowercase letters \(a-z\), digits \(0-9\), and the following special characters: hyphens \(-\), underscores \(\_\), periods \(.\), and tildes \(~\)|These characters do not need to be encoded|
            |Other characters|Encodes characters into the `else XY` format. `XY` specifies the hexadecimal ASCII code of a character. For example, the double quotation marks \("\) are encoded as `22`.|
            |Extended UTF-8 characters|Encode in `% XY%ZA...` format.|
            |Spaces|Encodes the character into `figure 20` instead of a plus sign \(+\). This encoding method is different from the `application/x-www-form-urlencoded` MIME encoding algorithm, such as the `java.net.URLEncoder` class provided by the Java Standard Library. You can use this encoding method directly by replacing the plus sign \(+\) in the encoded string with `%20`, the asterisk \(\*\) with `%2A`, and `%7E` with a tilde \(~\) to conform to the preceding encoding rules. You can use the following `percentEncode` method to implement this algorithm:

            ```

                  private static final String ENCODING = "UTF-8"; private static String percentEncode(String value) throws UnsupportedEncodingException { return value ! = null ? URLEncoder.encode(value, ENCODING).replace("+", "%20").replace("*", "%2A").replace("%7E", "~") : null; } 
                
            ``` |

        3.  Connect each encoded parameter name with its encoded value by using an equal sign \(=\).
        4.  Sort the parameter name and value pairs in the order specified in Step i. Then connect the pairs with ampersands \(&\) to produce the canonicalized query string.
    2.  Use the canonicalized query string to construct the string for signature calculation in the following way:

        ```
        
                  StringToSign = HTTPMethod + "&" + percentEncode("/") + "&" + percentEncode(CanonicalizedQueryString) 
                
        ```

        In the preceding information:

        -   HTTPMethod: the HTTP method used to submit requests, such as GET.
        -   Percentencode\(\#/\), is the encoded value \(% 2F\) of a forward slash \(/\) according to the URL encoding rules described in step 1.1.
        -   Encoding the canonicalized query string constructed in step 1 according to the URL encoding rules described in step 1.2.
2.  Obtain the signature.
    1.  Calculate the HMAC value of the StringToSign based on RFC 2104.

        **Note:** Use the SHA1 algorithm to calculate the HMAC value of the string-to-sign. Your AccessKey secret followed by an ampersand \(&\) \(ASCII code 38\) is used as the key for HMAC calculation.

    2.  Encode the HMAC value in Base64 to obtain the signature string
    3.  Add the Signature string to the request parameters as the Signature parameter.

        **Note:** When the obtained signature value is submitted as the final request parameters value, the value must be URL-encoded like other parameters according to [RFC3986](https://tools.ietf.org/html/rfc3986) rules.


## Examples

Take DescribeRegions API as an example. The `AccessKeyId` is `testid`, and the `AccessKeySecret` is `testsecret`. The following example shows the request URL to be signed:

```

     http://ecs.aliyuncs.com/?Timestamp=2016-02-23T12:46:24Z&Format=XML&AccessKeyId=testid&Action=DescribeRegions&SignatureMethod=HMAC-SHA1&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&Version=2014-05-26&SignatureVersion=1.0 
   
```

Use `testsecret&` to calculate the signature value. The result is as follows:

```

     OLeaidS1JvxuMvnyHOwuJ+uX5qY= 
   
```

Add the Signature parameter to the request as the Signature parameter. The signed URL is as follows:

```

     http://ecs.aliyuncs.com/?SignatureVersion=1.0&Action=DescribeRegions&Format=XML&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&Version=2014-05-26&AccessKeyId=testid&Signature=OLeaidS1JvxuMvnyHOwuJ+uX5qY=&SignatureMethod=HMAC-SHA1&Timestamp=2016-02-23T12%3A46%3A24Z 
   
```
