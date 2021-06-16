

# JWT notes

依赖

```xml
<!--jwt相关依赖-->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.8.1</version>
</dependency>
<dependency>
    <groupId>joda-time</groupId>
    <artifactId>joda-time</artifactId>
    <version>2.10.3</version>
</dependency>

<!--日志相关-->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.30</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.30</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

<!--lombok-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

JwtTokenUtils.class

```java
import com.auth0.jwt.JWT;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.interfaces.DecodedJWT;
import com.auth0.jwt.interfaces.JWTVerifier;
import lombok.Builder;
import lombok.Setter;
import lombok.extern.slf4j.Slf4j;
import org.joda.time.DateTime;

@Slf4j
@Builder//lombok注解建造者模式
public class JwtTokenUtils {
    /**
     * 传输信息，必须是json格式
     */
    private String msg;
    /**
     * 所验证的jwt
     */
    @Setter
    private String token;

    private final String secret="324iu23094u598ndsofhsiufhaf_+0wq-42q421jiosadiusadiasd";

    public String creatJwtToken () throws Exception {
        msg = new AESUtil(msg).encrypt();//先对信息进行aes加密(防止被破解） AES 对称加密
        String token = null;
        try {
            token = JWT.create()
                    .withIssuer("zs")
                    .withExpiresAt(DateTime.now().plusDays(1).toDate())
                    .withClaim("user", msg)
                    .sign(Algorithm.HMAC256(secret));
        } catch (Exception e) {
              throw e;
        }
        return token;
    }
//测试
    public static void main(String[] args) throws Exception {

        String data = "hello,jwt";

        // 将该数据，放入jwt
//        JwtTokenUtils tokenUtils = JwtTokenUtils.builder().msg(data).build();
//        String token = tokenUtils.creatJwtToken();
//        System.out.println(token);

        String tokenStr
                = "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJ6cyIsImV4cCI6MTYyMzkyMzY3NSwidXNlciI6IkJBQ0VDOUQxNDRBMkZGNUI5ODA0RTY0QTBGMkZCMTgwIn0.vqLiFQ83mX4FFjtwmB39NbGlquCrNpR-nilxiWjZq54";


        // 从jwt中，取出放入的数据
        JwtTokenUtils freeJwtTokenUtil = JwtTokenUtils.builder().token(tokenStr).build();
        String s = freeJwtTokenUtil.freeJwt();
        System.out.println(s);

    }


    /**
     * 解密jwt并验证是否正确
     */
    public String freeJwt () {
        DecodedJWT decodedJWT = null;
        try {
            //使用hmac256加密算法
            JWTVerifier verifier = JWT.require(Algorithm.HMAC256(secret))
                    .withIssuer("zs")
                    .build();
            decodedJWT = verifier.verify(token);
            log.info("签名人：" + decodedJWT.getIssuer() + " 加密方式：" + decodedJWT.getAlgorithm() + " 携带信息：" + decodedJWT.getClaim("user").asString());
        } catch (Exception e) {
            log.info("jwt解密出现错误，jwt或私钥或签证人不正确");
            //throw new ValidateException(SysRetCodeConstants.TOKEN_VALID_FAILED.getCode(),SysRetCodeConstants.TOKEN_VALID_FAILED.getMessage());
        }
        //获得token的头部，载荷和签名，只对比头部和载荷
        String [] headPayload = token.split("\\.");
        //获得jwt解密后头部
        String header = decodedJWT.getHeader();
        //获得jwt解密后载荷
        String payload = decodedJWT.getPayload();
        if(!header.equals(headPayload[0]) && !payload.equals(headPayload[1])){
            //throw new ValidateException(SysRetCodeConstants.TOKEN_VALID_FAILED.getCode(),SysRetCodeConstants.TOKEN_VALID_FAILED.getMessage());
        }
        return new AESUtil(decodedJWT.getClaim("user").asString()).decrypt();
    }

}

```



AESUtil

```java
import lombok.Setter;
import lombok.extern.slf4j.Slf4j;

import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import java.security.Key;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;

@Slf4j
public class AESUtil {
    //加密或解密内容
    @Setter
    private String content;
    //加密密钥
    private String secret;

    public AESUtil(String content) {
        this.content = content;
        this.secret = "iwofnoadnsa922342mnjaolkdsao9423242niosadopa_a02402sad";
    }

    /**
     * 加密
     * @return 加密后内容
     */
    public String encrypt () {
        Key key = getKey();
        byte[] result = null;
        try{
            //创建密码器
            Cipher cipher = Cipher.getInstance("AES");
            //初始化为加密模式
            cipher.init(Cipher.ENCRYPT_MODE,key);
            //加密
            result = cipher.doFinal(content.getBytes("UTF-8"));
        } catch (Exception e) {
            log.info("aes加密出错:"+e);
        }
        //将二进制转换成16进制
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < result.length; i++) {
            String hex = Integer.toHexString(result[i] & 0xFF);
            if (hex.length() == 1) {
                hex = '0' + hex;
            }
            sb.append(hex.toUpperCase());
        }
        return  sb.toString();
    }

    /**
     * 解密
     * @return 解密后内容
     */
    public String decrypt () {
        //将16进制转为二进制
        if (content.length() < 1)
            return null;
        byte[] result = new byte[content.length()/2];
        for (int i = 0;i< content.length()/2; i++) {
            int high = Integer.parseInt(content.substring(i*2, i*2+1), 16);
            int low = Integer.parseInt(content.substring(i*2+1, i*2+2), 16);
            result[i] = (byte) (high * 16 + low);
        }

        Key key = getKey();
        byte[] decrypt = null;
        try{
            //创建密码器
            Cipher cipher = Cipher.getInstance("AES");
            //初始化为解密模式
            cipher.init(Cipher.DECRYPT_MODE,key);
            //解密
            decrypt = cipher.doFinal(result);
        } catch (Exception e) {
            log.info("aes解密出错："+e);
        }
        assert decrypt != null;
        return new String(decrypt);
    }

    /**
     * 根据私钥内容获得私钥
     */
    private Key getKey () {
        SecretKey key = null;
        try {
            //创建密钥生成器
            KeyGenerator generator = KeyGenerator.getInstance("AES");
            //初始化密钥
            SecureRandom random = SecureRandom.getInstance("SHA1PRNG");
            random.setSeed(secret.getBytes());
            generator.init(128,random);
            //生成密钥
            key = generator.generateKey();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }
        return key;
    }

    public static void main(String[] args) {
        AESUtil aesUtil=new AESUtil("Hello");
        String ec=aesUtil.encrypt();
        System.out.println(ec);
        System.out.println(new AESUtil(ec).decrypt());
    }
}

```

