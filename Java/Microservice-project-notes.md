# 微服务项目



```
com.mysql.cj.jdbc.Driver
```



日志

lombok的

@Sl4j注解提供log属性



## java发送邮件



sun公司给我们提供了Java发送邮件的解决⽅案，并且提供了jar包

```xml
<dependency>
    <groupId>javax.mail</groupId>
    <artifactId>mail</artifactId>
</dependency>
```



⽽Springboot项⽬则给我们提供了封装了上⾯mail包的接⼝，让我们在Springboot项⽬中能够更⽅便的 实现邮件发送

- 导包

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-mail</artifactId>
  </dependency>
  ```

- 配置

  ```properties
  #邮件服务器
  spring.mail.host=smtp.163.com
  #端⼝
  spring.mail.port=25
  #发送邮件邮箱⽤户名
  spring.mail.username=ciggarnot@163.com
  #发送邮件邮箱密码（这个密码是上⾯的授权码，不是邮箱登录的密码）
  spring.mail.password=SNTGFTIEAJUMRHCT
  #设置邮箱认证打开
  spring.mail.properties.mail.smtp.auth=true
  ```

  

- 代码编写

  ```java
  @Autowired
  private JavaMailSender javaMailSender;
  @Test
  public void sendMail(){
      SimpleMailMessage message = new SimpleMailMessage();
      message.setFrom("ciggarnot@163.com");
      message.setTo("291136733@qq.com");
      message.setSubject("csmall商城激活2");
      message.setText("http://localhost:8080/user/verify?uid=akjsdnh39udklnw");
      javaMailSender.send(message);
  /*
  javaMailSender.send(MimeMessage mimeMessage)发送有附件的邮件
  javaMailSender.send(SimpleMailMessage simpleMailMessage)发送简单的邮件对象
  */
  }
  ```

  

# 能优化的部分



密码散列

通知+注解 比 在各方法体中分别使用散列工具类风格和功能更好。

Md5有各种静态工具类，但是在每个方法分别使用工具类，不如配置 通知+注解 ，这样各个方法体只专注于自己的业务即可。

```
 /**
     * 用户激活
     */
    @Anoymous
    @GetMapping("verify")
    public ResponseData verify(String uid, String username) {
        UserVerifyRequest userVerifyRequest = new UserVerifyRequest();
        userVerifyRequest.setUserName(username);
        userVerifyRequest.setUuid(uid);
        UserVerifyResponse response = iVerifyService.userVerify(userVerifyRequest);

        if (response.getCode().equals(SysRetCodeConstants.SUCCESS.getCode())) {
            return new ResponseUtil<>().setData(null);
        }
        return new ResponseUtil().setErrorMsg(response.getCode());
    }
```

