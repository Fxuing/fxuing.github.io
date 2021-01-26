---
title: spring mvc 动态添加 RequestMapping
date: 2021年1月26日
comments: true
categories: java
tags:
- springmvc
---


# spring mvc 动态添加 RequestMapping

>  *想法总是非常独特，有时候我们想在运行期间动态添加接口，那么就用到了动态添加 `RequestMapping`了*

<!--more-->

首先我们得搞清楚 `springmvc` 请求原理，客户端发起请求，会先去 `RequestMappingHandlerMapping` 里面去查找，如果找不到，就 404 了。



明白了请求原理，那么动态添加就容易了。只需要往`RequestMappingHandlerMapping`里面添加一个请求映射就可以了。



`AbstractHandlerMethodMapping` 提供了 `registerMapping` 和 `unregisterMapping` 方法

继承关系：

 `RequestMappingHandlerMapping` -> `RequestMappingInfoHandlerMapping` ->  `AbstractHandlerMethodMapping`



具体代码：

```java
@Autowired
RequestMappingHandlerMapping requestMappingHandlerMapping;

public ResultData hello(){
    return ResultData.success("hello");
}

@PostMapping("get")
@ResponseBody
public ResultData addRequestMapper() throws IllegalAccessException, InstantiationException {

    Class<?> entry = this.getClass();

    // hello 是方法名，ReflectionUtils 是 org.springframework.util 包
    Method methodName = ReflectionUtils.findMethod(entry, "hello");

    PatternsRequestCondition patterns = new PatternsRequestCondition("op/api/test/hello");

    RequestMethodsRequestCondition method = new RequestMethodsRequestCondition(RequestMethod.POST);
    RequestMappingInfo mappingInfo = new RequestMappingInfo(patterns, method, null, null, null, null, null);

    requestMappingHandlerMapping.registerMapping(mappingInfo, entry.newInstance(), methodName);
    return ResultData.success();
}
```
