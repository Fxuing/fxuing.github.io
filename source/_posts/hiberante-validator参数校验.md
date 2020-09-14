---
title: hiberante-validator 参数校验
date: 2020年9月14日
comments: true
categories: java
tags:
- 参数校验
---

### hibernate validator

我们经常需要对参数进行校验，很多情况下都是一个个参数校验，这种校验方式虽然也达到了效果，但是看起来就比较别扭。而且如果接收参数为`map`的时候，不太好处理，我在网上找资料也没有找到符合我情况的资料，于是去`hibernate`官网翻了下文档，再次分享下思路。

<!-- more -->

1. 首先编写实体类，添加注解
  
2. 调用`hiberante-validator`校验参数，校验不通过的会放在`Set`里面
  
3. 遍历`Set`的值，有值则抛出异常
  
4. 统一异常处理，针对异常信息处理
  

**具体实现**

```java
/**
 * @Author: code-generate
 * @Date: 2020/09/14 14:15:51
 * @Description:
 */
@Data
public class RoleInfo implements Serializable {

   private static final long serialVersionUID = 1L;
   
   /**  */
   @TableId(type = IdType.AUTO)
   private Long id;
   /** 角色名 */
   @NotNull(message = &quot;角色名称不能为空&quot;)
   private String roleName;
   /** 角色编号 */
   @NotNull(message = &quot;角色编号不能为空&quot;)
   private String roleCode;
   /** 系统编号 */
   @NotNull(message = &quot;系统编号不能为空&quot;)
   private String sysCode;
}

```

**校验工具类**

```java
/**
 * hibernate validator 参数校验
 *
 * @author Hou_fx
 * @date 2020.9.14 17:53
 * @description
 */
public class ValidatorUtil {
    private static Validator validator;

    private static Validator init() {
        if (validator == null) {
            ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
            validator = factory.getValidator();
        }
        return validator;

    }

    public static &lt;T&gt; void validator(T checkObject) {
        init();
        Set&lt;ConstraintViolation&lt;T&gt;&gt; res = validator.validate(checkObject);
        res.forEach(f -&gt; {
            throw new BizException(ResponseStatus.PARAMS_INVALIDE.getCode(), f.getMessage());
        });
    }
}

```

**异常处理**

```java
public class BizException extends RuntimeException {
    private String code;

    public BizException(String code, String message) {
        super(message);
        this.code = code;
    }

    public BizException(ResponseStatus responseStatus) {
        super(responseStatus.getMessage());
        this.code = responseStatus.getCode();
    }
}

```

**全局异常处理**

```java
@ControllerAdvice
public class ExceptionsHandler {

    private static final Logger LOG = LoggerFactory.getLogger(ExceptionsHandler.class);

    @ExceptionHandler(BizException.class)
    @ResponseBody
    public ResultData exceptionCheckData(HttpServletRequest request, BizException ex) {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        ResultData result = new ResultData();
        result.setStatus(ex.getCode());
        result.setMessage(ex.getMessage());
        FastJsonConfig fastJsonConfig = new FastJsonConfig();
        fastJsonConfig.setFeatures(Feature.OrderedField);
        BodyReaderHttpServletRequestWrapper httpReq = (BodyReaderHttpServletRequestWrapper) request;
        Request requestObj = JSON.parseObject(httpReq.getBody(), Request.class, fastJsonConfig.getFeatures());
        stopWatch.stop();
        commonLogService.saveCommonLog(requestObj,result,request.getRequestURI(),stopWatch.getTotalTimeSeconds(),ex);
        return result;
    }
}
```

使用方式也非常简单： `ValidatorUtil.validator(roleInfo);`就可以了，有异常会直接返回信息，也不用再写多余的校验了。

**总结**

这种方式与大部分资料不同的是，现在大部分的处理方式都是在 `controller` 调用的地方使用`@Valid`注解，然后使用`BindingResult`操作，通过全局异常处理，这种方式不需要使用`@Valid`注解和`BindingResult`，相同点是，也是通过全局异常处理，可随地使用，如果是`Map`的话，先转为`java bean`再对`java bean`操作即可