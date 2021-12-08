## **1.1  Cookie机制**


在程序中，会话跟踪是很重要的事情。理论上，**一个用户的所有请求操作都应该属于同一个会话**，而另一个用户的所有请求操作则应该属于另一个会话，二者不能混淆。例如，用户A在超市购买的任何商品都应该放在A的购物车内，不论是用户A什么时间购买的，这都是属于同一个会话的，不能放入用户B或用户C的购物车内，这不属于同一个会话。

而Web应用程序是使用HTTP协议传输数据的。**HTTP协议是无状态的协议。一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话**。即用户A购买了一件商品放入购物车内，当再次购买商品时服务器已经无法判断该购买行为是属于用户A的会话还是用户B的会话了。要跟踪该会话，必须引入一种机制。

Cookie就是这样的一种机制。它可以弥补HTTP协议无状态的不足。在Session出现之前，基本上所有的网站都采用Cookie来跟踪会话。

### **1.1.1  什么是Cookie**


Cookie意为“甜饼”，是**由W3C组织提出**，最早由Netscape社区发展的一种机制。目前Cookie已经成为标准，所有的主流浏览器如IE、Netscape、Firefox、Opera等都支持Cookie。

由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？就**给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理**。

Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。

查看某个网站颁发的Cookie很简单。在浏览器地址栏输入`javascript:alert (document. cookie)`就可以了（需要有网才能查看）。JavaScript脚本会弹出一个对话框显示本网站颁发的所有Cookie的内容，如图1.1所示。

如果浏览器不支持Cookie（如大部分手机中的浏览器）或者把Cookie禁用了，Cookie功能就会失效。

### **1.1.2  Cookie的不可跨域名性**


很多网站都会使用Cookie。例如，Google会向客户端颁发Cookie，Baidu也会向客户端颁发Cookie。那浏览器访问Google会不会也携带上Baidu颁发的Cookie呢？或者Google能不能修改Baidu颁发的Cookie呢？

答案是否定的。**Cookie具有不可跨域名性**。根据Cookie规范，浏览器访问Google只会携带Google的Cookie，而不会携带Baidu的Cookie。Google也只能操作Google的Cookie，而不能操作Baidu的Cookie。

Cookie在客户端是由浏览器来管理的。浏览器能够保证Google只会操作Google的Cookie而不会操作Baidu的Cookie，从而保证用户的隐私安全。浏览器判断一个网站是否能操作另一个网站Cookie的依据是域名。Google与Baidu的域名不一样，因此Google不能操作Baidu的Cookie。

需要注意的是，虽然网站images.google.com与网站www.google.com同属于Google，但是域名不一样，二者同样不能互相操作彼此的Cookie。

注意：用户登录网站www.google.com之后会发现访问images.google.com时登录信息仍然有效，而普通的Cookie是做不到的。这是因为Google做了特殊处理。本章后面也会对Cookie做类似的处理。

### **1.1.3  Unicode编码：保存中文**


中文与英文字符不同，**中文属于Unicode字符，在内存中占4个字符，而英文属于ASCII字符，内存中只占2个字节**。Cookie中使用Unicode字符时需要对Unicode字符进行编码，否则会乱码。

提示：Cookie中保存中文只能编码。一般使用UTF-8编码即可。不推荐使用GBK等中文编码，因为浏览器不一定支持，而且JavaScript也不支持GBK编码。

### **1.1.4  BASE64编码：保存二进制图片**


Cookie不仅可以使用ASCII字符与Unicode字符，还可以使用二进制数据。例如在Cookie中使用数字证书，提供安全度。使用二进制数据时也需要进行编码。

注意：本程序仅用于展示Cookie中可以存储二进制内容，并不实用。由于浏览器每次请求服务器都会携带Cookie，因此Cookie内容不宜过多，否则影响速度。Cookie的内容应该少而精。

### **1.1.5  设置Cookie的所有属性**


Cookie常用属性

### Table


| 属  性  名     | 描    述                                                                                                                                                                                          |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name           | 该Cookie的名称。Cookie一旦创建，名称便不可更改                                                                                                                                                    |
| value          | 该Cookie的值。如果值为Unicode字符，需要为字符编码。如果值为二进制数据，则需要使用BASE64编码                                                                                                       |
| int maxAge     | 该Cookie失效的时间，单位秒。如果为正数，则该Cookie在maxAge秒之后失效。如果为负数，该Cookie为临时Cookie，关闭浏览器即失效，浏览器也不会以任何形式保存该Cookie。如果为0，表示删除该Cookie。默认为–1 |
| boolean secure | 该Cookie是否仅被使用安全协议传输。安全协议。安全协议有HTTPS，SSL等，在网络上传输数据之前先将数据加密。默认为false                                                                                 |
| String path    | 该Cookie的使用路径。如果设置为“/sessionWeb/”，则只有contextPath为“/sessionWeb”的程序可以访问该Cookie。如果设置为“/”，则本域名下contextPath都可以访问该Cookie。注意最后一个字符必须为“/”           |
| String domain  | 可以访问该Cookie的域名。如果设置为“.google.com”，则所有以“[google.com](http://google.com)”结尾的域名都可以访问该Cookie。注意第一个字符必须为“.”                                                   |
| String comment | 该Cookie的用处说明。浏览器显示Cookie信息的时候显示该说明                                                                                                                                          |
| int version    | 该Cookie使用的版本号。0表示遵循Netscape的Cookie规范，1表示遵循W3C的RFC 2109规范                                                                                                                   |

### **1.1.6  Cookie的修改、删除**


Cookie并不提供修改、删除操作。如果要修改某个Cookie，只需要新建一个同名的Cookie，添加到response中覆盖原来的Cookie。

如果要删除某个Cookie，只需要新建一个同名的Cookie，并将maxAge设置为0，并添加到response中覆盖原来的Cookie。注意是0而不是负数。负数代表其他的意义。读者可以通过上例的程序进行验证，设置不同的属性。

注意：修改、删除Cookie时，新建的Cookie除value、maxAge之外的所有属性，例如name、path、domain等，都要与原Cookie完全一样。否则，浏览器将视为两个不同的Cookie不予覆盖，导致修改、删除失败

### **1.1.7  Cookie的域名**


Cookie是不可跨域名的。域名www.google.com颁发的Cookie不会被提交到域名www.baidu.com去。这是由Cookie的隐私安全机制决定的。隐私安全机制能够禁止网站非法获取其他网站的Cookie。

正常情况下，同一个一级域名下的两个二级域名如www.helloweenvsfei.com和images.helloweenvsfei.com也不能交互使用Cookie，因为二者的域名并不严格相同。如果想所有helloweenvsfei.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数，例如：

```javaScript
Cookie cookie = new Cookie("time","20080808");     // 新建Cookie
cookie.setDomain(".helloweenvsfei.com");           // 设置域名
cookie.setPath("/");                               // 设置路径
cookie.setMaxAge(Integer.MAX_VALUE);               // 设置有效期
response.addCookie(cookie);                        // 输出到客户端
```


注意：domain参数必须以点(".")开始。另外，name相同但domain不同的两个Cookie是两个不同的Cookie。如果想要两个域名完全不同的网站共有Cookie，可以生成两个Cookie，domain属性分别为两个域名，输出到客户端。

### **1.1.8  Cookie的路径**


domain属性决定运行访问Cookie的域名，而path属性决定允许访问Cookie的路径（ContextPath）。例如，如果只允许/sessionWeb/下的程序使用Cookie，

可以这么写：

```javaScript
Cookie cookie = new Cookie("time","20080808");     // 新建Cookie
cookie.setPath("/session/");                       // 设置路径
response.addCookie(cookie);                        // 输出到客户端
```


设置为“/”时允许所有路径使用Cookie。path属性需要使用符号“/”结尾。name相同但domain不同的两个Cookie也是两个不同的Cookie。

注意：页面只能获取它属于的Path的Cookie。例如/session/test/a.jsp不能获取到路径为/session/abc/的Cookie。使用时一定要注意。

### **1.1.9  Cookie的安全属性**


HTTP协议不仅是无状态的，而且是不安全的。使用HTTP协议的数据不经过任何加密就直接在网络上传播，有被截获的可能。使用HTTP协议传输很机密的内容是一种隐患。如果不希望Cookie在HTTP等非安全协议中传输，可以设置Cookie的secure属性为true。浏览器只会在HTTPS和SSL等安全协议中传输此类Cookie。下面的代码设置secure属性为true：
```javaScript
Cookie cookie = new Cookie("time", "20080808"); // 新建Cookie
cookie.setSecure(true);                           // 设置安全属性
response.addCookie(cookie); // 输出到客户端
```


提示：secure属性并不能对Cookie内容加密，因而不能保证绝对的安全性。如果需要高安全性，**需要在程序中对Cookie内容加密、解密，以防泄密。**

### **1.1.10  案例：永久登录**


如果用户是在自己家的电脑上上网，登录时就可以记住他的登录信息，下次访问时不需要再次登录，直接访问即可。实现方法是**把登录信息如账号、密码等保存在Cookie中，并控制Cookie的有效期，下次访问时再验证Cookie中的登录信息即可。**

保存登录信息有多种方案。最直接的是把用户名与密码都保持到Cookie中，下次访问时检查Cookie中的用户名与密码，与数据库比较。这是**一种比较危险的选择，一般不把密码等重要信息保存到Cookie中**。

还有**一种方案是把密码加密后保存到Cookie中，下次访问时解密并与数据库比较**。这种方案略微安全一些。如果不希望保存密码，还可以把登录的时间戳保存到Cookie与数据库中，到时只验证用户名与登录时间戳就可以了。

这几种方案验证账号时都要查询数据库。

本例将采用另一种方案，只在登录时查询一次数据库，以后访问验证登录信息时不再查询数据库。实现方式是**把账号按照一定的规则加密后，连同账号一块保存到Cookie中。下次访问时只需要判断账号的加密规则是否正确即可**。本例把账号保存到名为account的Cookie中，把账号连同密钥用MD1算法加密后保存到名为ssid的Cookie中。验证时验证Cookie中的账号与密钥加密后是否与Cookie中的ssid相等。

提示：该加密机制中最重要的部分为算法与密钥。由于MD1算法的不可逆性，即使用户知道了账号与加密后的字符串，也不可能解密得到密钥。因此，只要保管好密钥与算法，该机制就是安全的。
