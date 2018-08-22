# 1. 概述

提供SipRS工作流引擎的相关接口，可以对流程，和任务进行启动，查询，重试等操作。

### 访问鉴权

Oauth2鉴权之client_credentials。首先，要向SipRS申请客户端访问信息，如下：

- clientId
- client_secret

用户通过获取的client信息，获取token，通过携带token，访问API。

### 认证服务器

#### get_token

```
http://authServer/ssov3/oauth/token?grant_type=client_credentials&scope=${scope}&client_id=${clientId}&client_secret=${secret}
```

 返回信息：

```
{
    "access_token": "bG9jYWw6NGM4ZDA2YmUtNGEwYS00ZDg4LTlkMjQtM2I4MTM3ZjVkYTZh",
    "token_type": "Bearer",
    "expires_in": 36000,
    "refresh_token": "bG9jYWw6NDhkYzU5MjktNGExMS00N2NlLWIxOWEtNzNmYmM5ZjE4YzUw",
    "eCode": "local"
}
```

#### check_token

```
http://authServer/ssov3/oauth2/introspect?access_token=bG9jYWw6MmRmM2U5MmMtZDlmMy00M2U0LW&client_id=${clientId}&client_secret=${secret}
```

返回信息：

```
{
    "error": "invalid_token",
    "error_description": "无效的token"
}

{
    "user_id": null,
    "expires_in": 35933,
    "active": true,
    "exp": 1534957749,
    "scope": "siprsall",
    "username": null,
    "user_name": null,
    "token_type": "Bearer",
    "client_id": "deploys",
    "sub": null,
    "eCode": "local",
    "resource": "siprs_engine",
    "client": {
        "id": "deploys",
        "eCode": "local"
    }
}
```

#### token认证地址

资源服务检查token合法性的API。

```
String checkEndPoint = "http://authServer/ssov3/oauth/token?grant_type={grant_type}&scope={scope}&client_id={client_id}&client_secret={client_secret}";
       
```

### JavaSDk

开发者可以自行通过以上API获取Token，然后调用API。也可以使用引擎提供的SDK进行调用。

此SDK基于Spring RestTemplate进行封装，实现了自动获取token，并在token过期时自动更新token的功能。具体调用方式，可参考RestTemplate的相关文档。

具体调用信息如下：

#### maven依赖

```
<dependency>
     <groupId>bsip</groupId>
     <artifactId>bsip-rest-client</artifactId>
     <version>dev-SNAPSHOT</version>
</dependency>
```

#### 调用样例

```
// checkEndPoint 为认证服务器地址 ： 
String checkEndPoint = "http://authServer/ssov3/oauth/token?grant_type={grant_type}&scope={scope}&client_id={client_id}&client_secret={client_secret}";

RestTemplateFactory factory = new RestTemplateFactory("clientA","clientA_secret","clientScope",checkEndPoint);

protected JsonNode getExchangeBody(ResponseEntity<String> exchange) throws Exception {
    JsonNode jsonNode = MqUtils.transStrToJsonNode(exchange.getBody());
    if (!exchange.getStatusCode().equals("200"))
        throw new Exception(jsonNode.findValue("message").textValue());
    return jsonNode;
}

public JsonNode getProcessLog(String processInstanceId) throws Exception {
    RestTemplate restTemplate = factory.getRestTemplate();
    HttpEntity entity = new HttpEntity(restTemplateFactory.getHttpHeaders());
    ResponseEntity<String> exchange = restTemplate.exchange("http://backend:8306/process/{processInstanceId}"
            ,HttpMethod.GET,entity,String.class,ImmutableMap.of("processInstanceId",processInstanceId));
    JsonNode jsonNode = getExchangeBody(exchange);
    return jsonNode;
}
```



