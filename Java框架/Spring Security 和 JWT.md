## Spring Security

>SpringSecurity是一个强大的可高度定制的认证和授权框架，对于Spring应用来说它是一套Web安全标准。
>
>SpringSecurity注重为Java应用提供认证和授权功能，像所有的Spring项目一样，它对于自定义需求具有强大的扩展性。

## JWT

>JWT是JSON WEB TOKEN的缩写，它是基于 RFC 7519 标准定义的一种可以安全传输的的JSON对象，由于使用了数字签名，所以是可信任和安全的。

### JWT的组成

- JWT token的格式：header.payload.signature

- header中用于存放签名的生成算法

  {"alg": "HS512"}

payload中用于存放用户名、token的生成时间和过期时间

```json
{"sub":"admin","created":1489079981393,"exp":1489684781}
```

signature为以header和payload生成的签名，一旦header和payload被篡改，验证将失败

```java
//secret为加密算法的密钥
String signature = HMACSHA512(base64UrlEncode(header) + "." +base64UrlEncode(payload),secret)
```

#### [JWT实现认证和授权的原理](http://www.macrozheng.com/#/architect/mall_arch_04?id=jwt实现认证和授权的原理)

- 用户调用登录接口，登录成功后获取到JWT的token；
- 之后用户每次调用接口都在http的header中添加一个叫Authorization的头，值为JWT的token；
- 后台程序通过对Authorization头中信息的解码及数字签名校验来获取其中的用户信息，从而实现认证和授权。































