# B端用户中心

前端工程地址：https://wiki.maoyan.com/pages/viewpage.action?pageId=42626415

cat接入： 
  https://docs.sankuai.com/dp/arch/cat/master/#_1
  https://wiki.maoyan.com/pages/viewpage.action?pageId=41045411&code=eAENw0tOg0AAANA0brp06dKlaYLBgczAku9Qamml0gqbBqZYfgMDIyI9iocwHsWV7oxH8Aj6kjc9O__-fJ1cTC5_P37evwQwvZElWRGR1F1t7gVFgbIsKQI2Vn5jqOaDB-pizngCHqHAecOfr_On09sk1ZwidPC4vc1OSNhXADVxYYULxivqcorHNGR5QF23oVj_PwZVzErK2jhibh7iPC53KkReWPtHpcvEpE9KiBotU8nOGWazVLPsIbT0wdQsfWls1z1mxDeBA22JUHSway-Yt1F0XL4A2BGwWAceaP2DsXHVvqWBLRJv3BNzFRM_vfsDaKdSIg**eAEFwQcBwDAMAzBKbl49ODkNfwiTgJBuzsi42QoUtZ8rcU5l4HLCXunjeKcW-2rnZEpQsPkDL8gRxw&t=1569644347755

### 线上故障
https://wiki.maoyan.com/pages/viewpage.action?pageId=77058460

「重试」。比如：  

下游系统返回「请求超时」、「被限流中」等临时状态的时候，我们可以考虑重试  
而如果是返回“余额不足”、“无权限”等明确无法继续的业务性错误的时候就不需要重试了  
一些中间件或者 rpc 框架中返回 Http503、404 等没有何时恢复的预期的时候，也不需要重试  
如果确定要进行「重试」，我们还需要选定一个合适的「重试策略」。主流的「重试策略」主要是以下几种。  

策略 1. 立即重试。有时故障是候暂时性，可能是因网络数据包冲突或硬件组件流量高峰等事件造成的。在此情况下，适合立即重试操作。不过，立即重试次数不应超过一次，如果立即重试失败，应改用其它的策略。  

策略 2. 固定间隔。应用程序每次尝试的间隔时间相同。 这个好理解，例如，固定每 3 秒重试操作。（以下所有示例代码中的具体的数字仅供参考。）  

`报警`接入
https://wiki.maoyan.com/pages/editpage.action?pageId=77061114



### 猫眼通keeper改造
`问题`
```javascript
1、运营端需要不能通过token，是什么方式？
2、商家端的调用逻辑？
3、登录问题？猫眼通加`下拉框`？
```




https://wiki.maoyan.com/pages/viewpage.action?pageId=74484976




