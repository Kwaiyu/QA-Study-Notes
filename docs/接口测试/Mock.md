- 客户端mock：mockjs
- 服务端mock：mockserver、moco

mockjs
http://mockjs.com/

mockserver
https://github.com/mock-server/mockserver

moca
https://github.com/dreamhead/moco

## mockserver使用

### MockServer运行方式：

- . 通过java程序（我们的演示程序使用这种方式）
- . JUnit环境
- . 命令行（缺点是不能像moco那样带配置文件参数，这个下面将讲到）
- . maven plugin
- . Node.js
- . war包

### 配置项

Request-Matcher 匹配的参数:



- \1. method - 有GET\POST\PUT\DELETE...
- \2. path - 就是请求路径，比如\mypath\{id}
- \3. path parameters - 路径参数
- \4. query string parameters - 直接的QueryString请求参数
- \5. headers - key-values matchers
- \6. cookies - key-values matchers
- \7. body - body matchers
- \8. secure - boolean value, true for HTTPS

response返回以下任意：

- \1. status code
- \2. reason phrase
- \3. body
- \4. headers
- \5. cookies
- \6. delay

### 1.简单get请求

```java
private static void mockGet() {        
        mockServer.when(
                request()
                    .withMethod("GET")
                    .withPath("/cart")
                    //设置两次后失效
                    //,Times.exactly(2)
                    //设置5秒内只能访问一次
                    //,Times.once(),
                    //TimeToLive.exactly(TimeUnit.SECONDS, 5L)
                    //正则，starts with "/some"
                    //.withPath("/some.*")//.withPath(not("/some.*"))
            )
            .respond(
                response()
                    .withBody("some_response_body")
            );
        
        mockServer.when(
            request()
                .withMethod("GET")
                .withPath("/cart1")
                .withQueryStringParameters(
                        new Parameter("cartId", "055CA455-1DF7-45BB-8535-4F83E7266092")
                )
        )
        .respond(
            response()
                .withBody("some_response_body_withQueryStringParameters")
        );
        
    } 
```

### 2.参数正则替换

```java
private static void mockGetPathParameter() {
        
        mockServer.when(
                request()
                    .withMethod("GET")
                    .withPath(BASEPATH_VIEW+"/cart/{cartId}")
                    .withPathParameters(
                        new Parameter("cartId", "[A-Z0-9\\-]+")
                    )
                    .withQueryStringParameters(
                            new    Parameter("year", "2019"),
                            new    Parameter("month", "10"),
                            new    Parameter("userid", "[A-Z0-9\\\\-]+"))
            )
            .respond(
                response()
                    .withBody("some_response_body")
            );    

    }
```

### 3.POST request with Body-json

```java
public static void mockPost() {

        mockServer.when(
            request()
                .withMethod("POST")
                .withPath(BASEPATH_VIEW)
                .withBody("{username: 'user', password: 'mypassword'}")
        )
            .respond(
                response()
                .withStatusCode(200)
                .withCookie(
                        "sessionId", "2By8LOhBmaW5nZXJwcmludCIlMDAzMW"
                        )
                .withHeaders(
                        new Header("Content-Type", "application/json; charset=utf-8"),
                        new Header("Cache-Control", "public, max-age=86400")
                        )
                .withBody("{ \"apply_id\": \"000001\", \"overdued\": \"Y\" }")
        );
    }
```

### 4.json占位符

```java
        private static void mockBodyPlaceholder() {
            mockServer.when(
                request()
                .withBody(
                        new JsonBody("{" + System.lineSeparator() +
                                "    \"id\": 1," + System.lineSeparator() +
                                "    \"name\": \"A_${json-unit.any-string}\"," + System.lineSeparator() +
                                "    \"price\": \"${json-unit.any-number}\"," + System.lineSeparator() +
                                "    \"price2\": \"${json-unit.ignore-element}\"," + System.lineSeparator() +
                                "    \"enabled\": \"${json-unit.any-boolean}\"," + System.lineSeparator() +
                                "    \"tags\": [\"home\", \"green\"]" + System.lineSeparator() +
                                "}",
                            MatchType.ONLY_MATCHING_FIELDS
                        )
                )
                )
            .respond(
                response()
                    .withBody("some_response_body")
        );
    }
```

### 5.Responese with body-json

```java
private static void mockResponese() {
        mockServer.when(
                request()
                    .withMethod("GET")
                    .withPath(BASEPATH_GET)
            )
        .respond(
                response()
                .withStatusCode(200)
                //.withHeader("Content-Type", "plain/text")
                .withCookie("Session", "97d43b1e-fe03-4855-926a-f448eddac32f")
                .withBody(new JsonBody("{" + System.lineSeparator() +
                        "    \"id\": 1," + System.lineSeparator() +
                        "    \"name\": \"姓名\"," + System.lineSeparator() +
                        "    \"price\": \"123\"," + System.lineSeparator() +
                        "    \"price2\": \"121\"," + System.lineSeparator() +
                        "    \"enabled\": \"true\"," + System.lineSeparator() +
                        "    \"tags\": [\"home\", \"green\"]" + System.lineSeparator() +
                        "}"
                    ))
            );
    }
```

### 6.一个稍微灵活些的扩展思路：通过把各种配置项写进数据表，注入到mock服务，实现无代码mock

```java
static ClientAndServer mockServer;

    public static void main(String[] args) {
        
        mockServer = startClientAndServer(1080);
        List<Instance> mockInstances = getDataFromDb();
        mockInstances(mockInstances);
    }

    private static List<Instance> getDataFromDb() {
        List<Instance> mockInstances = new ArrayList<Instance>();
        
        //数据行1
        Instance inst = new Instance();        
        inst.setMethod("GET"); 
        inst.setPath("/mypath/{id}/{type}");

        Map<String,String> nullMap = new HashMap<String,String>();
        Parameter[] paths = {new Parameter("id", "[A-Z0-9\\\\-]+"),new Parameter("type", "[A-Z0-9\\\\-]+")};
        //inst.setQuery_string_parameters(new Parameter("year", "2019")); 
        inst.setPath_parameters(paths);
        inst.setReq_cookie(nullMap);//--留空
        inst.setReq_headers(nullMap);//--留空
        inst.setReq_jsonbody("{}");//--空json
        inst.setResp_body("{\r\n" + 
                "  \"id\" : 1,\r\n" + 
                "  \"name\" : \"姓名\",\r\n" + 
                "  \"price\" : \"123\",\r\n" + 
                "  \"price2\" : \"121\",\r\n" + 
                "  \"enabled\" : \"true\",\r\n" + 
                "  \"tags\" : [ \"tag1\", \"tag2数组项\" ]\r\n" + 
                "}");
        
        Map<String,String> cookieMap = new HashMap<String,String>();        
        cookieMap.put("Session", "97d43b1e-fe03-4855-926a-f448eddac32f");
        inst.setResp_cookie(cookieMap);
        inst.setResp_headers(nullMap);
        inst.setResp_statuscode(200);
        
        //数据行2
        Instance inst2 = new Instance();
        inst2.setMethod("GET"); 
        inst2.setPath("/mypath2");
        Parameter[] queryParams = {new Parameter("month", "10"),new Parameter("userid", "[A-Z0-9\\\\-]+")};
        inst2.setQuery_string_parameters(queryParams); 
        inst2.setReq_cookie(nullMap);//--留空
        inst2.setReq_headers(nullMap);//--留空
        inst2.setReq_jsonbody("{}");//--空json
        inst2.setResp_body("{\r\n" + 
                "  \"id\" : \"97d43b1e-fe03-4855-926a-f448eddac32f\",\r\n" + 
                "  \"name\" : \"姓名\",\r\n" + 
                "  \"year\" : \"2019\",\r\n" + 
                "  \"month\" : \"10\",\r\n" + 
                "  \"userid\" : \"id\",\r\n" + 
                "  \"tags\" : [ \"tag3\", \"tag4\" ]\r\n" + 
                "}");
        inst2.setResp_cookie(cookieMap);
        inst2.setResp_headers(nullMap);
        inst2.setResp_statuscode(200);
        
        //把数据行注入实例列表，用于后续注入
        mockInstances.add(inst);
        mockInstances.add(inst2);
        return mockInstances;
    }


    private static void mockInstances(List<Instance> mockInstances) {
        
        for (Instance inst : mockInstances) {
            List<Cookie> cookies = new ArrayList<Cookie>();
            List<Header> headers = new ArrayList<Header>();
            List<Cookie> resp_cookies = new ArrayList<Cookie>();            
            List<Header> resp_headers = new ArrayList<Header>();                    
            //注入MOCK
            injectionMock(inst, cookies, headers, resp_cookies, resp_headers);        
        }
    }

    private static void injectionMock(Instance inst, List<Cookie> cookies, List<Header> headers,
            List<Cookie> resp_cookies, List<Header> resp_headers) {
        for (Entry<String, String> entry : inst.getReq_cookie().entrySet()) {
            cookies.add(new Cookie(entry.getKey(),entry.getValue()));
        }            
        for (Entry<String, String> entry : inst.getReq_headers().entrySet()) {
            headers.add(new Header(entry.getKey(),entry.getValue()));
        }
        
        for (Entry<String, String> entry : inst.getResp_cookie().entrySet()) {
            resp_cookies.add(new Cookie(entry.getKey(),entry.getValue()));
        }
        for (Entry<String, String> entry : inst.getResp_headers().entrySet()) {
            resp_headers.add(new Header(entry.getKey(),entry.getValue()));
        }
        
        if (inst.getPath_parameters()==null) {                
            mockQueryParamsNoBody(inst,cookies, headers, resp_cookies, resp_headers);
        }
        else {
            mockPathParamsNoBody(inst,cookies, headers, resp_cookies, resp_headers);
        }
    }

    private static void mockPathParamsNoBody(Instance inst, List<Cookie> cookies, List<Header> headers,
            List<Cookie> resp_cookies, List<Header> resp_headers) {
        
        mockServer.when(
                request()
                    .withMethod(inst.getMethod())
                    .withPath(inst.getPath())
                    .withPathParameters(inst.getPath_parameters())
                    //.withQueryStringParameters(inst.getPath_parameters())
                    .withCookies(cookies)
                    .withHeaders(headers)
                    .withBody(new JsonBody(inst.getReq_jsonbody()))
            )
        .respond(
                response()
                .withStatusCode(inst.getResp_statuscode())
                .withHeaders(resp_headers)
                .withCookies(resp_cookies)
                .withBody(new JsonBody(
                        inst.getResp_body()    
                    ))
            );
    }
    
    private static void mockQueryParamsNoBody(Instance inst, List<Cookie> cookies, List<Header> headers,
            List<Cookie> resp_cookies, List<Header> resp_headers) {
        
        mockServer.when(
                request()
                    .withMethod(inst.getMethod())
                    .withPath(inst.getPath())
                    //.withPathParameters(inst.getPath_parameters())
                    .withQueryStringParameters(inst.getQuery_string_parameters())
                    .withCookies(cookies)
                    .withHeaders(headers)
                    .withBody(new JsonBody(inst.getReq_jsonbody()))
            )
        .respond(
                response()
                .withStatusCode(inst.getResp_statuscode())
                .withHeaders(resp_headers)
                .withCookies(resp_cookies)
                .withBody(new JsonBody(
                        inst.getResp_body()    
                    ))
            );
    }
```

结果：
调用：http://localhost:1080/mypath/22/2234

```java
{
      "id" : 1,
      "name" : "姓名",
      "price" : "123",
      "price2" : "121",
      "enabled" : "true",
      "tags" : [ "tag1", "tag2数组项" ]
     }
```

调用：http://localhost:1080/mypath2?month=10&userid=11

```java
{
"id" : "97d43b1e-fe03-4855-926a-f448eddac32f",
"name" : "姓名",
"year" : "2019",
"month" : "10",
"userid" : "id",
"tags" : [ "tag3", "tag4" ]
}
```

## moco使用

开发者郑晔是ThoughtWorks首席技术专家。moco也是根据一些配置，启动一个HTTP服务。当发起请求满足配置中的一个条件时，它就给回复一个应答。Moco的底层没有依赖于像Servlet这样的重型框架，是基于Netty框架直接编写的HTTP服务，这样一来，绕过了复杂的应用服务器，速度极快。

Moco运行方式：
https://github.com/dreamhead/moco/blob/master/moco-doc/usage.md 

- \1. API通过java程序
- \2. JUnit环境
- \3. 命令行（带配置文件参数，这个好用）
- \4. maven plugin
- \5. Socket
- \6. https

### 最简单用法

### conf.json

```java
conf.json  
    [
          {
            "response" :
                  {
                    "text" : "Hello, Moco"
                  }
              }
     ] 
```

### run:

```java
java -jar moco-runner-1.1.0-standalone.jar http -p 12306 -c conf.json
```

### 例子 

```java
[
      {
        "request" :
        {
          "uri":"/getUser",
          "method":"get",
          "queries":{
            "type":"pp",
            "age":"3"
          }
        },
        "response" :
          {
            "text" : "Hello, Moco"
          }
      },
    
      {
        "description":"带参数的post请求",
        "request":{
          "uri":"/postDemoWithParam",
          "method":"post",
          "forms":{
            "param1":"one",
            "param2":"two"
          }
        },
        "response":{
          "text":"this is post request with param 并且是form格式的"
        }
      },
    
      {
        "description":"带header请求",
        "request": {
          "uri": "/withHeader",
          "method": "post",
          "headers": {
            "content-type": "application/json"
          },
          "json": {
            "name": "xiaoming",
            "age": "18"
          }
        },
        "response": {
          "json": {
            "code": "C0",
            "msg": "查询成功",
            "data": {
              "name": "周**",
              "cid": "22************011",
              "respCode": "0",
              "respDesc": "一致,成功",
              "detail": {
                "name": "周**",
                "cid": "22************011",
                "driveIssueDate": "A",
                "driveValidStartDate": "A",
                "firstIssueDate": "A",
                "validDate": "-B",
                "driveCardStatus": "A",
                "allowDriveCar ": "C小型汽车 ",
                "driveLicenseType ": "A ",
                "gender ": "1 / 男性 "
              }
    
            }
          }
        }
      },
    ]
```

如果response响应json非常长，可以写到文件里

```java
{
        "description":"查全部",
        "request":{
          "uri":"/findAll",
          "forms":{
            "gender": "1"
          }
        },
        "response":{
          "file":"result_file/findAll.json"
        }
      }
```

### 分模块

当配置的路径多了，容易乱，可以分模块，比如首页模块、登陆模块

首页模块：index.json:

```java
 [
            {
            "description": "首页",
            "request": {
            "uri": "/index"
            },
            "response": {
            "text": "hello world"
            }
            }
        ]
```

登录模块：login.json

```java
[
    {
        "description": "登录",
        "request": {
            "uri": "/login"
        },
        "response": {
            "text": "success"
        }
    }
   ] 
```

合并：

```java
[
{"include": "index.json"},
{"include": "login.json"}
]
```

 MockJS客户端mock就不详细介绍了，非常简单，可以看官网说明。

实现实时动态mock的完整代码：

https://gitee.com/475660/databand/tree/master/databand-mock-api