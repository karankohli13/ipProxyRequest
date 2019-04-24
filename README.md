# ipProxyRequest

A module that can make requests through a free dynamic Ip proxy, suitable for crawlers.

The idea of ​​obtaining ip is the reference to this repository . On this basis, the code is optimized and expanded, and the request logic is encapsulated and can be used directly.

Use method, please refer to /examples/index.js:

Install: npm install ip-proxy-request --save or yarn add ip-proxy-request

```javascript
const ipProxyRequest = require('ip-proxy-request')
// async
const { response, data } = await ipProxyRequest.send({ url: 'xxxx'})

// promise
ipProxyRequest.send({ url: 'xxxx'})
.then(({ response, data }) => {
 // xxx
}).catch(err => {
 // xxx
})
```

The module is to crawl https://proxy.l337.tech/txt this page to get free proxy ip, so strongly rely on this site.

Due to the instability of the proxy ip, some ips are also valid for a short period of time. When making multiple requests, each time you will first pull the ip list and try it one by one. The Ip that can return the request normally succuessProxywill continue to be used for the next request until the ip cannot be accessed correctly, then use the ip list. One will try again. If the ip list is tried, it will climb the ip list and loop through the operation.

The result returned by the proxy ip access is not necessarily the correct result. If the returned page is still wrong, such as some garbled page for the blocked Ip, you need to make your own judgment, then ipProxyRequest.refreshSuccuessProxy()set the succuesProxy to null, so the next request Will use the new ip.

ipProxyRequest.sendMethod, the parameter is requestthe standard input format of the module. This method returns an promiseobject. For each invocation request, if an attempt to ip does not return correctly, it will be replaced by an ip until the correct return resolve({ response, data }). If the time of this entire repeated attempt exceeds 40s, it will reject(new Error('timeout'))prevent the error from being urlcaused by the always wrong request.

It is mainly used for crawlers, so if it is not passed 'User-Agent', it will be given a random one by default 'User-Agent'. Due to the acquisition of the ip process and the proxy speed of ip itself, the request will be slower, and the timeout I gave is also higher, which is not suitable for scenarios with high response speed.

Because the module of request depends on tunnel-agentsome request error, it will throw an error that cannot be caught by the assert module, causing the program to quit unexpectedly reading the issue of the warehouse, and found that many people have encountered this problem, and The module seems to have no one to maintain... so I forked a request to my repository, tunnel-agentpointing the download address of the module to another repository where the developer solved the problem tunnel-agent.



