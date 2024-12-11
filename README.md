

# 首先运行 consul



[下载 consul](https://github.com)


以开发模式运行





```
consul agent -dev
```




# 2\. 调试



1. 用 Visual Studio 2022 IDE 打开项目；
2. 右击解决方案\-选择“属性”
3. 在属性界面下，选择多项目启动, News.Server, Auth.Server, Register.Server, ApiGateway 几个项目的操作方式选择为“启动”；


如下图 ![jimu_demo_debug](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_debug_setting.png)



# 3\. 部署



1. 用 Visual Studio 2022 IDE 打开项目；
2. 右击 News.Server 项目，选择“发布”（Auth.Server,Register.Server,ApiGateway 同理）
3. 选择发布的文件夹；
4. 用命令提示符进入文件夹，启动项目




```
dotnet News.Server.dll

```

 

如下图


![jimu_demo_publish_setting](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_publish_setting.png)


![jimu_demo_register_server](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_news_server.png)


apigateway \& server ![jimu_demo_apigateway](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_apigateway.png)


service ![jimu_demo_apigateway_service](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_apigateway_services.png) ![jimu_demo_apigateway_service_all](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_apigateway_services_all.png) service detial ![jimu_demo_apigateway_serivce_detial](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_apigateway_services_detail.png)



# 4\. postman 调用



调用方不需要知道微服务部署的地址，直接访问网关就可以了。打开网关的服务列表可以看到所有服务的调用路径，在展开服务的明细可看到更多关于该服务的信息，如授权、角色限制等。


![jimu_demo_api](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api.png)


直接复制上面所示的调用地址，下面用 postman 对 api 进行调用:



## 4\.1 Register




### 4\.1\.1 判断用户名 grissom 是否可用



1. 使用 GET 方法请求；
2. Content\-Type 设置为 application/json； ![jimu_demo_api_call](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_call.png)


![jimu_demo_api_call_result](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_call_result.png)



### 4\.1\.2 注册会员 grissom



请求路径： [http://localhost:5000/api/v1/register/register?name\=grissom\&nickname\=Gil\&pwd\=123](https://github.com):[wgetcloud全球加速器服务](https://wgetcloud6.org)



## 4\.2 Auth




### 4\.2\.1 获取 token



上面注册会员都是匿名访问的，但访问受保护的 api 必须先获取 token 请求 token api 需要：


1. 使用 Post 方法请求；
2. Content\-Type 设置为 application/json；
3. Body 用 json 格式， 如 {"username":"grissom","password":"123"}；


![jimu_demo_api_token_setting_header](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_token_setting_header.png)


![jimu_demo_api_token_setting_body](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_token_setting_body.png)


![jimu_demo_api_token_call_result](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_token_call_result.png)


返回结果是一个 json 格式的对象, access\_token 就是生成的 token, expired\_in (数字时间戳从 1970 到现在的秒数)是 token 的失效时间




```
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MzIzNjUzNzUsInVzZXJuYW1lIjoiZ3Jpc3NvbSIsInJvbGVzIjoiYWRtaW4iLCJtZW1iZXIiOiJ7XCJJZFwiOlwiNTgwMDc4MGYtMjMyMy00ZTdjLWFmMDEtOWEyNzY4NDE0N2MyXCIsXCJOYW1lXCI6XCJncmlzc29tXCIsXCJOaWNrTmFtZVwiOlwiR2lsXCIsXCJSb2xlXCI6XCJhZG1pblwifSJ9.Rr1g94btU8oxJ3ci7dg3OY_QEj2sBhxI-YtyFZQONbQ",
    "expired_in": 1532365375
}

```

 


### 4\.2\.2 获取当前会员信息



1. 使用 GET 方法请求；
2. Content\-Type 设置为 application/json；
3. Authorization 设置为上面生成的 token, 格式： Bearer {token} 如




```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MzIzNjUzNzUsInVzZXJuYW1lIjoiZ3Jpc3NvbSIsInJvbGVzIjoiYWRtaW4iLCJtZW1iZXIiOiJ7XCJJZFwiOlwiNTgwMDc4MGYtMjMyMy00ZTdjLWFmMDEtOWEyNzY4NDE0N2MyXCIsXCJOYW1lXCI6XCJncmlzc29tXCIsXCJOaWNrTmFtZVwiOlwiR2lsXCIsXCJSb2xlXCI6XCJhZG1pblwifSJ9.Rr1g94btU8oxJ3ci7dg3OY_QEj2sBhxI-YtyFZQONbQ
```

 

4. 请求路径： [http://localhost:5000/api/v1/member/getcurrentmemberinfo](https://github.com)


![jimu_demo_api_currentmemberinfo_call_result](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_currentmemberinfo_call_result.png)



## 4\.3 News



会员服务： 获取所有新闻、获取指定新闻、发布新闻 都是受保护的 api (EnableAuthorization:true)，所以调用时都需要带上 token



### 4\.3\.1 获取所有新闻



1. 使用 GET 方法请求；
2. Content\-Type 设置为 application/json；
3. Authorization 设置为上面生成的 token, 格式： Bearer {token}
4. 调用路径： api/v1/news/getallnews


![jimu_demo_api_getallnews_call_result](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_getallnews_call_result.png)



### 4\.3\.2 发布新闻



发布新闻的接口接受的是一个对象，所以需要用 POST


1. 使用 POST 方法请求；
2. Content\-Type 设置为 application/json；
3. Authorization 设置为上面生成的 token, 格式： Bearer {token}
4. 调用路径： api/v1/news/postnews


![jimu_demo_api_postnews_call_result](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_getallnews_call_result.png)


根据返回的 id 获取该篇新闻 ![jimu_demo_api_getnews_call_result](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_getnews_call_result.png)



### 4\.3\.3 角色限制



我们看看发布新闻方法的声明 ![jimu_demo_api_role_setting](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_role_setting.png)


指定了 `Roles="admin"`, 即需要具有 admin 角色的用户才能访问， 而上面调用发布新闻的用户是 grissom , 在模拟数据里配置了他属于 admin 角色 ![jimu_demo_api_role](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_role.png)


而用户 Foo 的角色是 guest， 我们换成他去获取 token， 然后再调用发布新闻的 api, 会报错： 未授权 ![jimu_demo_api_postnews_call_result_unauthorized](https://raw.githubusercontent.com/wiki/grissomlau/jimu/images/jimu_demo_api_postnews_call_result_unauthorized.png)


