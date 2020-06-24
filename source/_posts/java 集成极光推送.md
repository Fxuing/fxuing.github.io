---
title: java 集成极光推送
date: 2020年6月24日
comments: true
categories: java
tags:
- 极光推送
---

## java 集成极光推送

前段时间公司引入了极光推送，做个简单的记录，大致开发步骤如下：

1. 到极光官网注册账号
2. 创建应用
3. 完善应用信息（Android、iOS）
4. 各端集成
5. 联调

<!--more-->

---

前面步骤不细讲了，可自行翻阅 -> [极光推送官方文档](https://docs.jiguang.cn//jpush/guideline/intro/)
我是直接下载官方sdk进行修改的，话不多说，直接上代码吧。

###### maven引入极光推送包，我们用的是3.3.11：

```java
<!-- 极光推送 -->
<dependency>
    <groupId>cn.jpush.api</groupId>
    <artifactId>jpush-client</artifactId>
    <version>3.3.11</version>
</dependency>
```

###### 主要业务代码如下（代码经处理过，大家需要根据实际业务场景修改）：

```java
@Service
public interface JpushService {
    boolean push(WeChatPushType type, String alias, String urlParam, String... inputParam);

    boolean jobPush(String title, String content, String url) throws APIConnectionException, APIRequestException;
}



/**
 * @Author: Hou_fx
 * @Date: 2019/8/1 11:40
 * @Description:
 */
@Service
public class JpushServiceImpl implements JpushService {
    @Value("${jPush.appKey}")
    private String appKey;
    @Value("${jPush.masterSecret}")
    private String masterSecret;
    @Value("${jPush.devMode}")
    private Boolean devMode;
    @Value("${jPush.url}")
    private String url;
    private static final Logger LOG = LoggerFactory.getLogger(WeChatPushServiceImpl.class);

    @Override
    public boolean push(WeChatPushType type, String alias,String urlParam, String ... inputParam) {
        try {
            return this.pushAlias(type, alias,urlParam,inputParam);

        } catch (APIConnectionException e) {
            // Connection error, should retry later
            LOG.error("Connection error, should retry later", e);
            return false;
        } catch (APIRequestException e) {
            // Should review the error, and fix the request
            LOG.error("Should review the error, and fix the request", e);
            LOG.info("HTTP Status: " + e.getStatus());
            LOG.info("Error Code: " + e.getErrorCode());
            LOG.info("Error Message: " + e.getErrorMessage());
            return false;
        }
    }

    @Override
    public boolean jobPush(String title, String content, String url) throws APIConnectionException, APIRequestException  {
        JPushClient jpushClient = new JPushClient(masterSecret, appKey, null, ClientConfig.getInstance());
        Map param = new HashMap() {{
            put("url", url);
        }};
        PushPayload payload = PushPayload.newBuilder()
                .setPlatform(Platform.android_ios())
                .setAudience(Audience.all())
                .setNotification(Notification.newBuilder()
                        .setAlert(content)
                        .addPlatformNotification(AndroidNotification.newBuilder()
                                .setTitle(title)
                                .addExtras(param).build())
                        .addPlatformNotification(IosNotification.newBuilder()
                                .incrBadge(1)
                                .addExtras(param).build())
                        .build())
                .setOptions(this.setOptions())
                .build();
        LOG.info("payload param - {}", JSONObject.toJSONString(payload));
        PushResult result = jpushClient.sendPush(payload);
        LOG.info("Got result - " + result);
        return false;
    }

    private void jPush(String title, String content, WeChatPushType type, String alias,String url) throws APIConnectionException, APIRequestException {
        JPushClient jpushClient = new JPushClient(masterSecret, appKey, null, ClientConfig.getInstance());
        Map param = new HashMap() {{
            put("type", type.name());
            put("url", url);
        }};
        PushPayload payload = PushPayload.newBuilder()
                .setPlatform(Platform.android_ios())
                .setAudience(Audience.alias(alias))
                .setNotification(Notification.newBuilder()
                        .setAlert(content)
                        .addPlatformNotification(AndroidNotification.newBuilder()
                                .setTitle(title)
                                .addExtras(param).build())
                        .addPlatformNotification(IosNotification.newBuilder()
                                .incrBadge(1)
                                .addExtras(param).build())
                        .build())
                .setOptions(this.setOptions())
                .build();
        LOG.info("payload param - {}", JSONObject.toJSONString(payload));
        PushResult result = jpushClient.sendPush(payload);
        LOG.info("Got result - " + result);
    }

    private Options setOptions(){
        if(devMode){
            return Options.newBuilder()
                    .setApnsProduction(false).build();
        }else{
            return Options.newBuilder()
                    .setApnsProduction(true).build();
        }
    }
    private boolean pushAlias(WeChatPushType type, String alias,String urlParam, String... inputParam) throws APIConnectionException, APIRequestException {
        String title,content,url="";
        WeChatPushType pushType;
        String[] params = urlParam.split(",");
        switch (type) {
            // 认证
            case W005:
                title = "认证成功";
                content = "恭喜，您的企业账号已认证成功，现可免费体验一键智能匹配服务，点击查看";
                pushType = W005;
                break;
            // 受理
            case W011:
                title = "对接进度变更";
                content = String.format("您的（%s）融资申请已受理，点击查看需求进度", inputParam);
                pushType = W011;
                url = String.format(this.url + "/aaaaaa/#/ssssss?cccc=%s&id=%s", params);
                break;
            // 审核
            case W016:
                title = "对接进度变更";
                content = String.format("您的（%s）融资申请已审批通过，点击查看需求进度", inputParam);
                pushType = W016;
                url = String.format(this.url + "/aaaaaa/#/ssssss?cccc=%s&id=%s", params);
                break;
            // 放款
            case W018:
                title = "对接进度变更";
                content = String.format("您的（%s）融资申请已成功放款，到账可能需要时间， 请耐心等待", inputParam);
                pushType = W018;
                url = String.format(this.url + "/aaaaaa/#/ssssss?cccc=%s&id=%s", params);
                break;
            // 审核失败
            case W007:
                title = "对接进度变更";
                content = String.format("很抱歉，您的（%s）融资申请审核失败，原因为:%s，如有疑问，可联系在线客服咨询", inputParam);
                pushType = W007;
                url = String.format(this.url + "/aaaaaa/#/ssssss?cccc=%s&id=%s", params);
                break;
            // 放款失败
            case W017:
                title = "对接进度变更";
                content = String.format("很抱歉，您的（%s）融资申请放款失败，原因为:%s，如有疑问，可联系在线客服咨询", inputParam);
                pushType = W017;
                url = String.format(this.url + "/aaaaaa/#/ssssss?cccc=%s&id=%s", params);
                break;
            // 匹配更新
            case W010:
                title = "融资方案";
                content = "已为您匹配到企业专属融资方案，点击查看";
                pushType = W010;
                url = String.format(this.url + "/aaaaaa/#/bbb?matchType=%s", params);
                break;

            default:
                LOG.error("type not exist");
                return false;
        }
        if (StringUtils.isNotEmpty(title)) {
            this.jPush(title, content, pushType, alias,url);
        }

        return true;
    }
}
```

###### 业务场景调用代码

我们是通过别名注册的，可查阅官方文档，这里以Android为例：[安卓别名注册](https://docs.jiguang.cn//jpush/client/Android/android_api/#api_3)

```java
jpushService.push(WeChatPushType.W005, "ENT&" + userid,"");
```

###### 测试效果图如下

![测试效果图](https://img-blog.csdnimg.cn/20191104190937959.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTQ5Mjc1,size_16,color_FFFFFF,t_70)
---

如有理解不到位之处，敬请斧正
END
