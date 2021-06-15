# 权限管理

 

# 1  引言

回顾我们之前做的权限管理

更加符合实践需求的精细的用户管理

粒度角色/操作

# 2  权限管理的基础知识

## 2.1   概念

权限管理要实现对用户访问系统的控制，按照安全规则或者[安全策略](http://baike.baidu.com/view/160028.htm)控制用户可以访问而且只能访问自己被授权的资源。

只要有用户参与的系统一般都要有权限管理。

权限管理包括用户**认证**和**授权**两部分

## 2.1   用户认证(en)

### 2.1.1        什么是用户认证 en authen

通过一些用户信息去识别用户是否合法，用户是谁，用户的身份是哪个。

常用的用户身份验证的方法：用户名密码方式、指纹、声纹

### 2.1.2        用户认证的流程

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image002.jpg)

### 2.1.3        关键对象

subject：原意是主题，主体。 可以理解为用户。当用户, 程序要去访问系统的资源，系统需要对subject进行身份认证。

 

principal：原意是当事人。可以理解为用户身份信息，通常是唯一的，一个主体还有多个身份信息，但是都有一个主身份信息（primary principal）

 

credential：凭证信息。 可以是密码 、证书、指纹，声纹。

 

**总结：主体在进行身份认证时需要提供身份信息或凭证信息。**

 

 

## 2.2   用户授权(or)author

 

### 2.2.1        概念

用户授权，简单理解为访问控制，在用户认证通过后，系统对用户访问资源进行控制，用户只能访问那些具有资源的访问权限的资源。

### 2.2.1        授权流程

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image004.jpg)

 

### 2.2.2        关键对象

授权的过程理解为：who对what(which)进行how操作。

 

who：principal主体即subject，subject在认证通过后系统进行访问控制。

 

what(which)：资源(**Resource**)，subject必须具备资源的访问权限才可访问该 资源。资源比如：系统某个页面、某个页面的按钮，某个删除或者修改菜单、或者更细的商品id为001的商品信息。

资源分为**资源类型和资源实例**：

系统的用户信息就是资源类型，相当于java类。

系统中id为001的用户就是资源实例，相当于new的java对象。

 

how：什么操作。用户添加、用户修改、商品删除。这些东西可以表示为具体的权限。

可以进行用户删除操作，就是具备用户删除的权限。

归纳一下，权限/许可(**permission**) ，又称针对资源的权限或许可，subject具有permission访问资源，如何访问/操作需要定义permission， 

 

## 2.3   权限管理模型

 

### 2.3.1        基本模型

我们可以把上面的分析抽象成一套通用的权限管理模型

 

用户身份信息（用户名和密码）

资源 （资源的名称 主键 ）

权限（这是一个how） （权限的名称，主键）

三者之间的关系是

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image006.jpg)

 

用户 **对某个资源的** **具体权限**，这种case表示起来不太方便。

通常企业开发中将资源和权限表合并为一张权限表，如下：

资源（资源名称、访问地址）

权限（权限名称、资源id）

合并为：

权限（权限名称、资源名称、资源访问地址）

比如用户查询：user:query, user, /user/query.action

这样模型可以简化为：

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image008.jpg)

 

### 2.3.2        通用模型（role角色）

分析上面的模型。

不足之处：

需要给每个具体的用户增加其对应的权限。如果用户很多，有100个，那么很麻烦。

所以我们可以给用户分类。

比如说有90个都是普通用户，具备的权限是一样的。

有9个都是管理员，具备的权限都是一样的。

有1个是超级管理员，具备的权限是一样的。

 

所以我们把模型优化成如下的case：

角色就是普通用户，管理员，超级管理员。

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image010.png)

 

上图常被称为权限管理的通用模型，不过企业在开发中根据系统自身的特点还会对上图进行修改，但是用户、角色、权限、用户角色关系、角色权限关系是需要去理解的。

 

通常给用户分配资源权限需要将权限信息持久化，比如存储在关系数据库中。

把用户信息、权限管理、用户分配的权限信息写到数据库（权限数据模型）

 

 

## 2.4   建立在通用模型下的权限管理

用户首先需要**认证**。认证之后访问就可以**获取用户的角色**，从而**获取到用户具备的权限**。这个流程成为授权。

 

 

当用户访问某个资源或者对某个资源进行操作时，需要分配相应的权限才可以访问或操作相应的资源。权限是对于资源的操作许可。

根据授权的不同依据，可以分为如下两种

### 2.4.1        基于角色的授权

(role based access control)，基于角色的访问控制。

比如：

系统角色包括 ：

超级管理员，管理员，普通用户。

或者

 部门经理、总经理。

当用户进行某个操作的时候，需要判断用户的角色。

比如总经理，可以查看所有人的薪资。

总经理可以查看所有员工的基本信息。

部门经理只能查询自己部门的员工基本信息。

 

讨论优缺点

问题：

比如：需要变更为部门经理和总经理都可以进行用户基本信息查看

//如果该user是总经理则可以访问if中的代码

if(user.hasRole('总经理')){

​     //系统资源内容

​     //用户报表查看

}

变更后

if(user.hasRole('部门经理') || user.hasRole('总经理') ){

​     //系统资源内容

​     //用户报表查看

}

 

 

角色针对人划分的，人作为用户在系统中属于活动内容，如果该 角色可以访问的资源出现变更，需要修改你的代码了，

 

 

所以基于角色的访问控制是不利于系统维护(可扩展性不强)。

 

### 2.4.2        基于资源的授权

(Resource based access control)，基于资源的访问控制。

资源在系统中是不变的，比如资源有：类中的方法，页面中的按钮。

如果某个角色对某个资源的访问权限发生变化了。

比如刚刚的case

if(user.hasPermission ('用户报表查看（权限标识符）')){

​     //系统资源内容

​     //用户报表查看

}

变更后。数据库里增加该用户的权限即可，代码无需修改。

所以通常建议使用基于资源的访问控制实现权限管理。

 

# 3  Shiro（声明式的）

## 3.1   引入

### 3.1.1        简介

shiro是apache的一个开源框架，是一个权限管理的框架，实现 用户认证、用户授权。

 

spring中有spring security (原名Acegi)，是一个权限框架，它和spring依赖过于紧密，没有shiro使用简单。

shiro不依赖于spring，shiro不仅可以实现 web应用的权限管理，还可以实现c/s系统，分布式系统权限管理，shiro属于轻量框架，越来越多企业项目开始使用shiro。

 

使用shiro实现系统 的权限管理，有效提高开发效率，从而降低开发成本。

### 3.1.2        基本架构

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image012.jpg)

 

subject：主体，可以是用户也可以是程序，主体要访问系统，系统需要对主体进行认证、授权。

securityManager：安全管理器，主体进行认证和授权都 是通过securityManager进行。

authenticator：认证器，主体进行认证最终通过authenticator进行的。

authorizer：授权器，主体进行授权最终通过authorizer进行的。

sessionManager：web应用中一般是用web容器对session进行管理，shiro也提供一套session管理的方式。

SessionDao： 通过SessionDao管理session数据，针对个性化的session数据存储需要使用sessionDao。

cache Manager：缓存管理器，主要对session和授权数据进行缓存，比如将授权数据通过cacheManager进行缓存管理，和**ehcache**整合对缓存数据进行管理。

realm：域，领域，相当于数据源，通过realm存取认证、授权相关数据。

cryptography：密码管理，提供了一套加密/解密的组件，方便开发。比如提供常用的散列、加/解密等功能。

 

比如 md5散列算法。Message digest 5

 

## 3.2   配置环境

```xml
<!--shiro的依赖--> 
<dependency>     
    <groupId>org.apache.shiro</groupId>     
    <artifactId>shiro-core</artifactId>     
    <version>1.4.1</version> 
</dependency> 
<dependency>    
    <groupId>commons-logging</groupId>     
    <artifactId>commons-logging</artifactId>     
    <version>1.2</version> 
</dependency>
```





## 3.3   认证

### 3.3.1        shiro认证流程

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image013.png)![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image014.png)

 

### 3.3.2        Demo

#### 3.3.2.1  配置shiro-first.ini

通过此配置文件创建securityManager工厂。

ini配置文件

```ini
# resources目录下的shiro-realm.ini配置文件
[users]
zhangsan=111111
lisi=222222
```



#### 3.3.2.2  代码

```
import  org.apache.shiro.mgt.SecurityManager;
```

 

```java
 @Test
    public void testLoginAndLogout() {
        //此示例获得ini的SecurityManager

        // 创建securityManager工厂，通过ini配置文件创建securityManager工厂
        IniSecurityManagerFactory securityManagerFactory = new IniSecurityManagerFactory("classpath:shiro-realm.ini");
        // 创建SecurityManager
        SecurityManager securityManager = securityManagerFactory.getInstance();

        // 将SecurityManager设置当前的运行环境中
        SecurityUtils.setSecurityManager(securityManager);
        // 从SecurityUtils里创建一个Subject
        Subject subject = SecurityUtils.getSubject();

        // 在认证提交前准备token（令牌）
        // 这里的账号和密码 将来是由用户输入进去
        UsernamePasswordToken usernamePasswordToken = new UsernamePasswordToken("zhangsan", "111111");
        try{
        	//执行认证提交
        	subject.login(usernamePasswordToken);
        } catch( AuthenticationException e) {
            e.printStackTrack();
        }
		//是否认证通过
        boolean authenticated = subject.isAuthenticated();
        System.out.println("是否认证通过：" + authenticated);
        
        //退出操作
        subject.logout();
    }
```

 如果认证不同过则无法授权

```ini
[users]
songge=zhenshuai,role1
zhaoge=niubi,role2
heidashuai=fangjia,role3
[roles]
role1=user:query,user:update
role2=user:delete,user:insert
role3=user:query,user:update,user:delete,user:insert
```

```java
//基于角色的授权
boolean role1 = subject.hasRole("role1");
System.out.println("是否拥有role1的角色：" + role1);

ArrayList<String> roleList = new ArrayList<>();
roleList.add("role1");
roleList.add("role2");
roleList.add("role3");
boolean[] hasRoles = subject.hasRoles(roleList);
System.out.println("对role123分别拥有的角色：" + Arrays.toString(hasRoles));

boolean hasAllRoles = subject.hasAllRoles(roleList); //判断list里的权限是否全部拥有
System.out.println("是否拥有全部角色：" + hasAllRoles);
```

```java
//基于权限的授权
String insertPermission = "user:insert";
String deletePermission = "user:delete";
String updatePermission = "user:update";
String queryPermission = "user:query";
boolean[] permitteds = subject.isPermitted(insertPermission, deletePermission, updatePermission, queryPermission);//可变参数，或list
System.out.println("增删改查多个的权限：" + Arrays.toString(permitteds));

boolean permitted1 = subject.isPermitted(queryPermission);
System.out.println("单个权限：" + permitted1);

boolean permittedAll = subject.isPermittedAll(insertPermission, deletePermission, updatePermission, queryPermission);//可变参数，或list
System.out.println("是否拥有全部权限：" + permittedAll);
```



#### 3.3.2.3  代码分析

代码的执行流程如下：

1、通过ini配置文件创建securityManager

2、调用subject.login方法主体提交认证，提交的token

3、securityManager进行认证，securityManager最终由<mark>ModularRealmAuthenticator</mark>进行认证。

4、ModularRealmAuthenticator调用IniRealm(给realm传入token) 去ini配置文件中查询用户信息

5、IniRealm根据输入的token（UsernamePasswordToken）从 shiro-first.ini查询用户信息，根据账号查询用户信息（账号和密码）

如果查询到用户信息，就给ModularRealmAuthenticator返回用户信息（账号和密码）

如果查询不到，就给ModularRealmAuthenticator返回null

6、ModularRealmAuthenticator接收IniRealm返回Authentication认证信息

​     如果返回的认证信息是null，ModularRealmAuthenticator抛出异常（org.apache.shiro.authc.UnknownAccountException）

​     如果返回的认证信息不是null（说明inirealm找到了用户），对IniRealm返回用户密码 （在ini文件中存在）和 token中的密码 进行对比，如果不一致抛出异常（org.apache.shiro.authc.IncorrectCredentialsException）

 

```java
package org.apache.shiro.authc.pam;
public class ModularRealmAuthenticator extends AbstractAuthenticator {...}
```



### 3.3.3        自定义realm实现认证（重点）

将来实际开发需要realm从数据库中查询用户信息。

#### 3.3.3.1  Realm

```
继承AuthorizingRealm
继承的方法：
c org.apache.shiro.realm.AuthorizingRealm
	m doGetAuthorizationInfo(principalCollection:PrincipalCollection):AuthoriaztionInfo
c org.apache.shiro.realm.AuthenticatingRealm
	m doGetAuthenticatioInfo(authenticationToken:AuthenticationToken):AuthenticationInfo
```

 

```java
public class CustomRealm extends AuthorizingRealm {

//    UserMapper userMapper;

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        //通过token获得用户名信息（token中的信息就是你认证的时传入的信息）
        UsernamePasswordToken token = (UsernamePasswordToken) authenticationToken;
        //这个username是接下来要去执行查询的条件→查询当前用户在系统中的密码
        String username = token.getUsername();

        //通过name信息去获得存在于系统或数据库的真实的密码（凭证）信息
        String passwordFromDb = queryPasswordByUsername(username);

        //第一个参数，是你想要存储在系统中的user信息Admin User Map String→是你接下来通过subject能够获得的用户信息
        User user = new User(username, passwordFromDb);
        //第二个参数，是用户正确的密码（系统维护的密码）
        //第三个参数，是realm的名字
        SimpleAuthenticationInfo authenticationInfo = new SimpleAuthenticationInfo(user,passwordFromDb,this.getName());
        return authenticationInfo;//若是数据库没有该用户可以返回null
    }
        //查询当前用户（已经认证通过的用户）的授权信息
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        //就是SimpleAuthenticationInfo的第一个参数
        User primaryPrincipal = (User) principalCollection.getPrimaryPrincipal();
        // user role permission 从数据库获取
        List<String> permissions = queryPermissionByUser(primaryPrincipal.getUsername());

        SimpleAuthorizationInfo authorizationInfo = new SimpleAuthorizationInfo();
        authorizationInfo.addRole("role1");//需要的话也可以添加角色信息，从数据库获取
        authorizationInfo.addStringPermissions(permissions);//通常用这个，比较方便冲数据库取字符串list的权限信息
        return authorizationInfo;
    }
    //如果从数据库返回的是这些权限
    private List<String> queryPermissionByUser(String username) {
        ArrayList<String> perms = new ArrayList<>();
        perms.add("user:insert");
        perms.add("user:delete");
        return perms;
    }
}
```



#### 3.3.3.2  配置

```ini
# custom.ini自定义realm的配置文件
customRealm=com.cskaoyan.realm.CustomRealm
securityManager.realm=$customRealm 
```

测试代码同上边的入门程序，需要更改ini配置文件路径：

`Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:custom.ini");`



```
密码不正确
org.apache.shiro.authc.IncorrectCredentialsException: Submitted credentials for token [org.apache.shiro.authc.UsernamePasswordToken - songge, rememberMe=false] did not match the expected credentials.
用户不存在doGetAuthenticationInfo返回Null时
org.apache.shiro.authc.UnknownAccountException: Realm [com.cskaoyan.realm.CustomRealm@234bef66] was unable to find account data for the submitted AuthenticationToken [org.apache.shiro.authc.UsernamePasswordToken - zhangsan, rememberMe=false].
```

 测试：

```java
@Test
public void test1(){
    IniSecurityManagerFactory factory = new IniSecurityManagerFactory("classpath:custom.ini");
    SecurityManager securityManager = factory.getInstance();
    SecurityUtils.setSecurityManager(securityManager);
    Subject subject = SecurityUtils.getSubject();

    subject.login(new UsernamePasswordToken("songge","123456"));

    boolean authenticated = subject.isAuthenticated();
    System.out.println(authenticated);

    boolean role1 = subject.hasRole("role1");//true

    boolean[] permitted = subject.isPermitted("user:insert", "user:delete", "user:query", "user:update");
    System.out.println(Arrays.toString(permitted));//true true false false
}
```



## 3.4   授权 

### 3.4.1        Shiro授权的流程

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image024.png)

### 3.4.2        Demo

#### 3.4.2.1  配置授权信息

shiro-permission.ini里边的内容相当于在数据库。

 

```
[users]
zhangsan=111111,role1
lisi=22222,role2
#权限
[roles]
#角色role1对资源user拥有create、update权限
role1=user:create,user:update,user:delete,user:query
#角色role2对资源user拥有create、delete权限
role2=user:update,user:query
#角色role3对资源user拥有create权限
role3=user:query
```

 

user:create：表示对用户资源进行create操作

#### 3.4.2.2  代码

认证通过才能授权

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image026.jpg)

 

#### 3.4.2.3  分析

### 3.4.3        自定义realm实现(重点)

#### 3.4.3.1  实际需求

上边的程序通过shiro-permission.ini对权限信息进行静态配置，实际开发中从数据库中获取权限数据。就需要自定义realm，由realm从数据库查询权限数据。realm根据用户身份查询权限数据，将权限数据返回给authorizer（授权器）。

 

#### 3.4.3.2  代码

自定义realm

在原来自定义的realm中，修改doGetAuthorizationInfo方法。

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image028.jpg)

#### 3.4.3.3  配置

同上自定义realm配置，在shiro-realm.ini中配置自定义的realm，将realm设置到securityManager中。

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image029.jpg)

 

测试代码同上边的入门程序，需要更改ini配置文件路径：

`Factory<SecurityManager> factory = **new** IniSecurityManagerFactory("classpath:shiro-realm.ini");`

 

#### 3.4.3.4  测试

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image031.jpg)

#### 3.4.3.5  小结

授权流程可以小结如下

1、对subject进行授权，调用方法isPermitted（"permission串"）

2、SecurityManager执行授权，通过ModularRealmAuthorizer执行授权

3、ModularRealmAuthorizer执行realm（自定义的CustomRealm）从数据库查询权限数据调用realm的授权方法：doGetAuthorizationInfo

4、realm从数据库查询权限数据，返回ModularRealmAuthorizer

5、ModularRealmAuthorizer调用PermissionResolver进行权限串比对

6、如果比对后，isPermitted中"permission串"在realm查询到权限数据中，说明用户访问permission串有权限，否则 没有权限，抛出异常。

 





# 4  项目使用xml

## 4.1   目标

给mall项目增加权限管理，权限管理的粒度细化到针对每一张表的增删改查操作。

 

 

## 4.2   整合shiro

### 4.2.1        引入shiro的依赖

```xml
<!-- shiro权限控制 包括了shiro-web和shiro-core --> 
<dependency>     
    <groupId>org.apache.shiro</groupId>     
    <artifactId>shiro-spring</artifactId>     
    <version>1.2.3</version> 
</dependency>
```

在web项目里除了引入shiro的核心依赖之外，还有引入shiro对web的支持（包含常用的web组件）以及shiro对spring的支持

 

### 4.2.2        配置

Shiro整合web的思路

基于一个Filter，拦截所有的url，然后使用shiro判断权限。

在web.xml中，配置如下的filter进行拦截。filter拦截后将操作权交给spring中配置的filterChain（过虑链）

 

#### 4.2.2.1  配置ShiroFilter

 

```xml
<!-- shiro的filter -->
<!-- shiro过虑器，DelegatingFilterProxy通过代理模式将spring容器中的bean和filter关联起来 -->
<filter>
  <filter-name>shiroFilter</filter-name>
  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
  <!-- 设置true由servlet容器控制filter的生命周期 -->
  <init-param>
    <param-name>targetFilterLifecycle</param-name>
    <param-value>true</param-value>
  </init-param>
  <!-- 设置spring容器的Filter id，如果不设置，则找于filter-name一致的bean-->
  <init-param>
    <param-name>targetBeanName</param-name>
    <param-value>shiroFilter</param-value>
  </init-param>

</filter>
<filter-mapping>
  <filter-name>shiroFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

#### 4.2.2.2  配置spring 

l 配置spring容器中的filter bean实例

 

```xml
l   <!-- web.xml中shiro的filter对应的bean -->
<!-- Shiro 的Web过滤器 -->
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
   <property name="securityManager" ref="securityManager" />
   <!-- loginUrl认证提交地址，如果没有认证将会请求此地址进行认证，请求此地址将由 formAuthenticationFilter进行表单认证 -->
   <property name="loginUrl" value="/" />
   <!-- 认证成功统一跳转到first.action，建议不配置，shiro认证成功自动到上一个请求路径 -->
   <!-- <property name="successUrl" value="/first.action"/> -->
   <!-- 通过unauthorizedUrl指定没有权限操作时跳转页面-->
   <property name="unauthorizedUrl" value="/refuse.jsp" /> 
   
   <!-- 过虑器链定义，从上向下顺序执行，一般将/**放在最下边 -->
   <property name="filterChainDefinitions">
      <value>
         <!-- -->
         /** = anon
      </value>
   </property>
</bean>
```

shiro提供很多filter。可以构成一个filterChain

l SecurityManager

```xml
l   <!-- securityManager安全管理器 -->
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
   <property name="realm" ref="customRealm"/>
</bean>
```

 

l 自定义Realm

 

```xml
<!-- realm -->
<bean id="customRealm" class="xxx.xxx.CustomRealm"></bean>
```

 

### 4.2.3        Shiro中的过滤器简介

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image033.jpg)

 

 

 

 

 

anon:例子/admins/**=anon 没有参数，表示可以匿名使用。

authc:例如/admins/user/**=authc表示需要认证(登录)才能使用，FormAuthenticationFilter是表单认证 

perms：例子/admins/user/**=perms[user:add],参数可以写多个，多个时必须加上引号，并且参数之间用逗号分割，例如/admins/user/**=perms["user:add,user:modify"]，当有多个参数时必须每个参数都通过才通过，想当于isPermitedAll()方法。

user:例如/admins/user/**=user没有参数表示必须存在用户, 身份认证通过或通过记住我认证通过的可以访问，当登入操作时不做检查

 

## 4.3   测试配置是否成功

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image035.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image037.jpg)

## 4.4   实现登录功能

首先把之前的登录拦截器去掉。

 

### 4.4.1        基本思路

使用FormAuthenticationFilter过虑器实现 ，原理如下：

将用户没有认证时，转到loginurl配置的页面进行认证，用户身份和用户密码提交数据到loginurl

FormAuthenticationFilter拦截住取出request中的username和password（两个参数名称是可以配置的）

FormAuthenticationFilter调用realm传入一个token（username和password）

realm认证时根据username查询用户信息（在Activeuser中存储，包括 userid、usercode、username、menus）。

如果查询不到，realm返回null，FormAuthenticationFilter向request域中填充一个参数（记录了异常信息）

 

 

### 4.4.2        具体实现

#### 4.4.2.1  登录页面

http://localhost/loginForm

 

#### 4.4.2.2  Spring拦截器的配置

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image039.jpg)

注意，上的配置，loginurl既表示没有登录跳转到哪个地址，又表示登录页面提交到哪个地址。

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image041.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image043.jpg)

表示所有的地址没有登录认证之前都会跳转到loginFrom上

 

#### 4.4.2.3  登录提交地址

这里注意 刚刚提到了，登录提交的地址，需要跟loginURL保持一致，所以修改

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image045.jpg)

修改完之后，根据上述认证的流程

将用户没有认证时，转到loginurl配置的页面进行认证，用户身份和用户密码提交数据到loginurl

FormAuthenticationFilter拦截住取出request中的username和password（两个参数名称是可以配置的）把参数封装到一个token里，

FormAuthenticationFilter调用realm传入一个token（username和password）

realm认证时根据username查询用户信息（在Activeuser中存储，包括 userid、usercode、username、menus）

需要去写自定义realm里的代码

#### 4.4.2.4  自定义realm

先写死认证信息（后续再用数据库查）

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image047.jpg)

 

定义好realm，用户登录的时候会走到这里，如果验证成功， 会走到successUrl /main

 

如果验证失败，则会走到我们的提交路径。loginForm

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image049.jpg)

登录成功，用户的Session信息如何保存？后续看。

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image051.jpg)

 

## 4.5   实现注销功能

之前我们自己手动实现

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image053.jpg)

使用shiro，可以增加一个注销的过滤器

 

不用我们去实现退出，只要去访问一个退出的url（该 url是可以不存在），由LogoutFilter拦截住，清除session。

在applicationContext-shiro.xml配置LogoutFilter：

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image055.jpg)

 

可以删除原来的logout的controller方法代码。

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image057.jpg)

 

## 4.6   Shiro保存的认证信息

啥是认证信息？就是用户登录成功之后记录的一些信息。

之前我们是通过Session保存用户登录信息。那么shiro呢？

他的用户认证信息如何保存？

查看realm的代码，如下

```
SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(
      activeUser, password, this.getName());
```

它的第一个参数就是。

 

认证信息，最后会放在Session里。（org.apache.shiro.subject.support.DefaultSubjectContext_PRINCIPALS_SESSION_KEY）

所以我们新建一个新的类

ActiveUser，用来保存认证信息。它里面可以包含realm从数据库查询用户信息，以及后续的用户菜单、usercode、username等。都通过ActiveUser设置在SimpleAuthenticationInfo中。

 

可以先构建一个

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image059.jpg)

放进去

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image061.jpg)

 

## 4.7   增加授权操作

授权过滤器的使用

在配置文件里，增加某些需要授权才能操作的配置

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image063.jpg)

对于没有授权的访问，配置一个授权失败页面

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image065.jpg)

然后在授权的回调函数里，通过当前用户的subject，确认用户是否拥有该权限。

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image067.jpg)

 

 

## 4.8   整合数据库

### 4.8.1        根据权限管理模型建表

用户sys_user

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image068.png)

角色sys_role

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image070.jpg)

用户角色关系sys_user_role

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image071.png)

权限sys_permission

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image073.jpg)

角色权限关系sys_role_permission

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image074.png)

 

### 4.8.2        实现service和Dao

根据数据库的表生成对应的Bean和Mapper

 

 

### 4.8.3        认证

#### 4.8.3.1  思路：

修改realm的doGetAuthenticationInfo，从数据库查询用户信息，realm返回的用户信息中包括用户名和密码，实现让shiro进行用户名和密码的校验

#### 4.8.3.2  实现

 

代码

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image076.jpg)![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image078.jpg)

 

### 4.8.4        授权

#### 4.8.4.1  思路

在需要授权的地方增加鉴权代码，然后修改realm的doGetAuthorizationInfo，从数据库查询权限信息。

 

#### 4.8.4.2  实现

##### 4.8.4.2.1   修改realm从数据库查询权限信息

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image080.jpg)![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image082.jpg)

如何在项目中需要鉴权的地方增加鉴权？

##### 4.8.4.2.2   方法1同整合数据库之前，通过配置过滤器对url鉴权

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image084.jpg)

 

 

##### 4.8.4.2.3   通过jsp标签，对显示的资源鉴权

引入标签库

<%@ tagliburi="http://shiro.apache.org/tags" prefix="shiro" %>

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image086.jpg)

##### 4.8.4.2.4   通过注解，对方法鉴权

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image088.jpg)

注解要生效需要在springmvc的配置文件里增加如下代码

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image090.jpg)

导包：

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image092.jpg)

 

注意需要增加在controller的扫描配置文件中

注解的方式鉴权失败默认会返回500错误，如需客制化，需要另配失败转向页面。

```
<!-- 配置异常跳转页面，此处异常页面是使用shiro注解时没有权限访问的跳转页面，不配置则会报500错误 -->
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <prop key="org.apache.shiro.authz.UnauthorizedException">
                <!-- 没有权限时跳转的页面 -->
                /refuse
            </prop>
        </props>
    </property>
</bean>
```

 

 

## 4.9   补充

### 4.9.1        Shiro缓存

#### 4.9.1.1  原因

没有配置之前，每次授权都需要查数据库，比较耗时耗资源

#### 4.9.1.2  简介

shiro中提供了对认证信息和授权信息的缓存。

shiro默认是关闭认证信息缓存的，对于授权信息的缓存shiro默认开启的。

主要研究授权信息缓存，因为授权的使用场景多，授权相关的数据量大。

实现目标：

用户认证通过

该 用户第一次授权：调用realm查询数据库

该 用户第二次授权：不调用realm查询数据库，直接从缓存中取出授权信息（权限标识符）。

#### 4.9.1.3  引入依赖

```
<!-- shiro缓存 -->
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-ehcache</artifactId>
  <version>1.2.3</version>
</dependency>
```

```
<!-- 缓存ehcache -->
<dependency>
       <groupId>net.sf.ehcache</groupId>
       <artifactId>ehcache</artifactId>
       <version>2.8.1</version>
   </dependency>
```

 

#### 4.9.1.4  配置cacheManager

```
<!-- 缓存管理器 -->
<bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
       <property name="cacheManagerConfigFile" value="classpath:shiro-ehcache.xml"/>
   </bean>
```

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image094.jpg)

 

#### 4.9.1.5  引入ehcache的配置文件

```
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">
   <!--diskStore：缓存数据持久化的目录 地址  -->
   <diskStore path="E:\develop\ehcache" />
   <defaultCache 
      maxElementsInMemory="1000" 
      maxElementsOnDisk="10000000"
      eternal="false" 
      overflowToDisk="false" 
      diskPersistent="false"
      timeToIdleSeconds="120"
      timeToLiveSeconds="120" 
      diskExpiryThreadIntervalSeconds="120"
      memoryStoreEvictionPolicy="LRU">
   </defaultCache>
</ehcache>
```

 

简单说明

<defaultCache

​      maxElementsInMemory="10000"

内存中缓存的最大的对象条目

​      eternal="false"

是否放在磁盘

​      timeToIdleSeconds="120

对象的空闲时间 单位是s 超时即从内存去掉

​      timeToLiveSeconds="120"

对象的生存周期 单位是s 2min

​      overflowToDisk="true"

内存中超过上限之后否放在磁盘中

​      maxElementsOnDisk="10000000"

disk中最多保存多少个对象

​      diskPersistent="false"

jvm 终止的时候是否保存到磁盘中

​      diskExpiryThreadIntervalSeconds="120"

120s 轮询一次，去清除过期的对象

​      memoryStoreEvictionPolicy="LRU"

内存清除策略：LRU 把某些cache干掉

Least Recently Used 算法 最近最少使用算法

去最近的一段时间内最不经常使用的那些对象。

LFU least Frequency Used 最不经常使用

使用频率最小的那些对象

FIFO fisrt in first out 队列  />

#### 4.9.1.6  缓存带来的问题

如果超级管理员修改了某个用户的权限，此时数据库的信息发生修改。

但是如果某用户是在修改之前登录的，权限信息会缓存，此时这个用户的权限使用缓存，不会立即发生变化，如需变化，需要更新缓存。

什么时候更新缓存？

如果用户正常退出，缓存自动清空。

如果用户非正常退出，缓存自动清空。

手动代码实现更新缓存：

在realm中定义clearCached方法：

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image096.jpg)

 

修改权限后调用该方法即可。

测试ok

 

 

### 4.9.2        Shiro会话管理

#### 4.9.2.1  配置

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image098.jpg)

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image100.jpg)

#### 4.9.2.2  测试

修改会话时间测试看是否生效

 

### 4.9.3        密码非明文存储

#### 4.9.3.1  MD5简介

#### 4.9.3.2  MD5Demo

 

#### 4.9.3.3  Shiro对MD5的支持

 

#### 4.9.3.4  自定义realm实现

##### 4.9.3.4.1   JavaSe实现

 

配置

```
[main]
 credentialsMatcher=org.apache.shiro.authc.credential.HashedCredentialsMatcher
 credentialsMatcher.hashAlgorithmName=md5
 credentialsMatcher.hashIterations=1

customRealm=com.cskoayan.shrio.second.CustomRealm
customRealm.credentialsMatcher=$credentialsMatcher
securityManager.realm=$customRealm
```

 

##### 4.9.3.4.2   整合web实现

配置

```
<!-- realm -->
<bean id="customRealm" class="org.deepsl.hrm.shiro.CustomRealm">
   <!-- 将凭证匹配器设置到realm中，realm按照凭证匹配器的要求进行散列 -->
   <property name="credentialsMatcher" ref="credentialsMatcher"/>
</bean>

<!-- 凭证匹配器 -->
<bean id="credentialsMatcher"
  class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
   <property name="hashAlgorithmName" value="md5"></property>
   <property name="hashIterations" value="1"></property>
</bean>
```

```
 
```

 

 

### 4.9.4        验证码

#### 4.9.4.1  思路

shiro使用FormAuthenticationFilter进行表单认证，验证校验的功能应该加在FormAuthenticationFilter中，在认证之前进行验证码校验。

需要写FormAuthenticationFilter的子类，继承FormAuthenticationFilter，改写它的认证方法，在认证之前进行验证码校验。

#### 4.9.4.2  实现

 

##### 4.9.4.2.1   自定义FormAuthenticationFilter

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image102.jpg)

##### 4.9.4.2.2   配置自定义的FormAuthenticationFilter

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image104.jpg)

```
<!-- 自定义form认证过虑器 -->
<!-- 基于Form表单的身份验证过滤器，不配置将也会注册此过虑器，表单中的用户账号、密码及loginurl将采用默认值，建议配置 -->
<bean id="formAuthenticationFilter" class="org.deepsl.hrm.shiro.CustomFormAuthenticationFilter">
   <!--表单中账号的input名称-->
   <property name="usernameParam" value="username" />
   <!--表单中密码的input名称-->
   <property name="passwordParam" value="password" />
    
</bean> 
```

 

 

##### 4.9.4.2.3   错误提示

之前有错误就直接返回到登录页面上了。

如果需要看到提示信息。

需要自己去写登录错误后的处理。url需要跟登录提交页面一样。但是只有登录失败才会走到。

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image106.jpg)

然后自定义错误处理

拿到这个错误的提示信息，放入到request域里，根据错误类型，转回到登录页面显示。

 

 

##### 4.9.4.2.4   前端显示

增加验证码显示前端

```
<div class="control-group">
   <div class="controls">
      <div class="input-icon left">
         <i class="icon-lock"></i>
         <input class="m-wrap placeholder-no-fix" style="width: 70px" type="password"
            placeholder="验证码" id="randomcode" name="randomcode">
         <img id="randomcode_img" src="${baseurl}validatecode.jsp" alt=""
               width="56" height="20" align='absMiddle' />
         <a href=javascript:randomcode_refresh()>刷新</a>
      </div>
   </div>
</div>
```

验证码刷新脚本

```
<script>
       //刷新验证码
       //实现思路，重新给图片的src赋值，后边加时间，防止缓存
       function randomcode_refresh() {
           $("#randomcode_img").attr('src',
               '${baseurl}validatecode.jsp?time' + new Date().getTime());
       }
   </script>
```

 

### 4.9.5        其他

#### 4.9.5.1  常见shiro的标签

Jsp页面添加：

<%@ tagliburi="http://shiro.apache.org/tags" prefix="shiro" %>

 

| 标签名称                            | 标签条件（均是显示标签内容） |
| ----------------------------------- | ---------------------------- |
| <shiro:authenticated>               | 登录之后                     |
| <shiro:notAuthenticated>            | 不在登录状态时               |
| <shiro:guest>                       | 用户在没有RememberMe时       |
| <shiro:user>                        | 用户在RememberMe时           |
| <shiro:hasAnyRoles name="abc,123" > | 在有abc或者123角色时         |
| <shiro:hasRole name="abc">          | 拥有角色abc                  |
| <shiro:lacksRole name="abc">        | 没有角色abc                  |
| <shiro:hasPermission name="abc">    | 拥有权限资源abc              |
| <shiro:lacksPermission name="abc">  | 没有abc权限资源              |
| <shiro:principal>                   | 显示用户身份名称             |

 <shiro:principal property="username"/>   显示用户身份中的属性值

 

 

#### 4.9.5.2  Shiro常见注解

Shiro共有5个注解， 详细如下

·    **RequiresAuthentication:**

使用该注解标注的类，实例，方法在访问或调用时，当前Subject必须在当前session中已经过认证。

·    **RequiresGuest:**

使用该注解标注的类，实例，方法在访问或调用时，当前Subject可以是“gust”身份，不需要经过认证或者在原先的session中存在记录。

·    **RequiresPermissions:**

当前Subject需要拥有某些特定的权限时，才能执行被该注解标注的方法。如果当前Subject不具有这样的权限，则方法不会被执行。

·    **RequiresRoles:**

当前Subject必须拥有所有指定的角色时，才能访问被该注解标注的方法。如果当天Subject不同时拥有所有指定角色，则方法不会执行还会抛出AuthorizationException异常。

·    **RequiresUser**

当前Subject必须是应用的用户，才能访问或调用被该注解标注的类，实例，方法。

 

注意：

**Shiro****的认证注解处理是有内定的处理顺序的，如果有个多个注解的话，前面的通过了会继续检查后面的，若不通过则直接返回，处理顺序依次为（与实际声明顺序无关）：**

RequiresRoles 

RequiresPermissions 

RequiresAuthentication 

RequiresUser 

RequiresGuest

示例

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image108.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Shiro-notes.assets\clip_image110.jpg)

 

#  SpringBoot整合Shiro前后端分离项目

依赖

```xml

```

Shiro组件配置类

ShiroConfig.class

```
// ShiroFilterFactoryBean
// 自定义SecurityManager继承DefaultWebSessionManager重写getSessionId方法，从请求头获取sessionId保证前后端分离跨域session的一致性
// DefaultWebSessionManager (继承重写getSesionId方法，从请求头中获取sessionId保证跨域的session一致性)
// AuthorizationAttributeSourceAdvisor(声明式权鉴注解组件 需要aspectjweaver依赖)
// 自定义Authenticator继承ModularRealmAuthenticator重写doAuthenticate方法对多个realms进行分发
/**
不在配置类中
自定义token继承UsernamePasswordToken增加type属性支持区别以分发多个realms
自定义realm
**/
```

```java
@Configuration
public class ShiroConfig {
    /**
     * 过滤器 ShiroFilterFactoryBean
     */
    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(DefaultWebSecurityManager securityManager) {
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        shiroFilterFactoryBean.setSecurityManager(securityManager);
        //这个loginUrl不仅是字面的登录url，而且是认证失败的重定向url,认证失败状态码是302
        //可以配也可以不配置
//        shiroFilterFactoryBean.setLoginUrl("/unauthc");//,由于这是前后端分离项目，后端不返回view，这个就暂不配置了
        //对请求过滤filter，这个filterChainDefinitionMap需要是有序的Map
        //key为请求url，value为过滤器
        //anon是匿名处理器
        LinkedHashMap<String, String> filterChainDefinitionMap = new LinkedHashMap<>();
        filterChainDefinitionMap.put("/admin/auth/login", "anon");
        filterChainDefinitionMap.put("/wx/**", "anon");
        //图片等静态资源也可以配置到anon匿名处理器不需要认证
        filterChainDefinitionMap.put("/wx/storage/fetch/**", "anon");//不用阿里云oos的话本地图片请求不需要认证
//        filterChainDefinitionMap.put("/unauthc", "anon");//loginUrl,前后分离原因不配置

        //可以在这里配置perms的，但是不建议，建议在Handler方法上使用注解声明式权鉴 @RequiresPermissions(value = {"perm1","perm2"},logical = Logical.OR)
//        filterChainDefinitionMap.put("/need/perm","perms[perm1]");

        filterChainDefinitionMap.put("/**", "authc");//需要认证的请求放在最后
        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterChainDefinitionMap);
        return shiroFilterFactoryBean;
    }

    /**
     * 核心组件DefaultWebSecurityManager
     */
    @Bean//多个realm使用list
    public DefaultWebSecurityManager securityManager(AdminRealm adminRealm,
                                                     UserRealm userRealm,
                                                     DefaultWebSessionManager sessionManager,
                                                     CustomAuthenticator authenticator) {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
//        securityManager.setRealm(adminRealm);
        List<Realm> realms = new ArrayList<>();
        realms.add(adminRealm);
        realms.add(userRealm);
        securityManager.setRealms(realms);
        securityManager.setSessionManager(sessionManager);
        securityManager.setAuthenticator(authenticator);
        return securityManager;
    }

    /**
     * 自己写继承DefaultWebSessionManager重写getSessionId方法，从请求头获取sessionId保证前后端分离跨域session的一致性
     */
    @Bean
    public DefaultWebSessionManager sessionManager() {
        DefaultWebSessionManager sessionManager = new CustomSessionManager();
        //可以设置一些属性
        //删除失效的session
        sessionManager.setDeleteInvalidSessions(true);
        //session过期时间
        sessionManager.setGlobalSessionTimeout(7 * 24 * 60 * 60 * 1000);
        sessionManager.setSessionValidationSchedulerEnabled(true);
        return sessionManager;
    }

    /**
     * 声明式鉴权通知组件，需要aspectjweaver依赖支持
     * handler方法加注解如：@RequiresPermissions(value = {"perm1","perm2"},logical = Logical.OR)
     */
    @Bean//需要set一个securityManager属性
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(DefaultWebSecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }

    /**
     * 自定一定认证器
     * 可以直接在自定一定认证器上加@Component注解生效，写在这里方便统一查看Shiro配置
     */
    @Bean
    public CustomAuthenticator authenticator(AdminRealm adminRealm, UserRealm userRealm) {
        CustomAuthenticator authenticator = new CustomAuthenticator();
        ArrayList<Realm> realms = new ArrayList<>();
        realms.add(adminRealm);
        realms.add(userRealm);
        authenticator.setRealms(realms);
        return authenticator;
    }
}

```

自定义sessionManager

```java
public class CustomSessionManager extends DefaultWebSessionManager {
    @Override
    protected Serializable getSessionId(ServletRequest request, ServletResponse response) {
        HttpServletRequest req = (HttpServletRequest) request;
        String sessionId = req.getHeader("X-cskaoyan-mall-Admin-Token");//后台管理前端请求头
        if (StringUtils.hasText(sessionId)) {
            return sessionId;
        }

        sessionId = req.getHeader("X-Litemall-Token");//微信端请求头
        if (StringUtils.hasText(sessionId)) {
            return sessionId;
        }

        return super.getSessionId(request, response);
    }
}
```

自定义认证器(可以在shiro配置类用@Bean注册，也可以直接用@Component注册)

```java
/**
 * 自定一定认证器，对多个realm惊醒分发
 * 重写父类ModularRealmAuthenticator的doAuthenticate方法
 *
 * （另一种实现方法：
 * 如果只写了1个realm也可以直接在realm的doGetAuthenticationInfo中分发，
 * 同样利用强转为CustomToken获取type的不同，去执行不同的业务逻辑，
 * 返回不同的AuthenticationInfo）
 */
public class CustomAuthenticator extends ModularRealmAuthenticator {
    @Override
    protected AuthenticationInfo doAuthenticate(AuthenticationToken authenticationToken) throws AuthenticationException {
        this.assertRealmsConfigured();
        Collection<Realm> originRealms = this.getRealms();
        //对realms进行分发
        CustomToken token = (CustomToken) authenticationToken;
        String type = token.getType();

        //AdminRealm -> adminrealm admin/user
        List<Realm> realms = new ArrayList<>();
        for (Realm originRealm : originRealms) {
            if (originRealm.getName().toLowerCase().contains(type)) {
                realms.add(originRealm);
            }
        }

        return realms.size() == 1 ? this.doSingleRealmAuthentication((Realm)realms.iterator().next(), authenticationToken) : this.doMultiRealmAuthentication(realms, authenticationToken);
    }
}
```

自定义token（继承UsernamePasswordToken）

```java
@Data
public class CustomToken extends UsernamePasswordToken {
    String type;
    public CustomToken(String username, String password, String type) {
        super(username, password);
        this.type = type;
    }
}
```

TODO 补全

自定义realm

```java
@Component
public class AdminRealm extends AuthorizingRealm {
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {

        return null;
    }
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }
}
@Component
public class UserRealm extends AuthorizingRealm {


    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        return null;
    }
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

}

```

