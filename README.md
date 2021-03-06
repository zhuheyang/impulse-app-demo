Express-build
===================
a simple application imitating the impulse interface

### 基础
1. 入口: loading, 方式: get
2. 示例参数: mcode=a1b2c3d4; opid=b1n2m3z2x2c3v4b5n5; customerclient=weixin

### 要求 
1. 需要将参数隐藏.
2. 存储opid会员卡号, 用数据库存储会员资料.
(购买记录 , 金额, 余额, 会员名, 会员来源, 会员卡号.)  
3. 登录后的界面(也即展示给用户的界面) : 模仿现有的脉冲应用(使用grid布局), 但不能直接复制.
4. url应看不到mcode, opid这些敏感信息.
(mcode是机器使用的,opid是用户自己的)  
只做微信即可!加油!  

### 过程
1. 用户扫码进入网址后, 识别是使用什么扫码的,(支付宝,微信,普通应用扫码)
2. 进入相关的登录确认(支付包权限, 微信权限)
3. 登录微信账号后, 会被分配一个服务器ID(也即opid), 支付宝另说.
4. 数据路由数据库,同时加载显示页面.

# 基本过程
1. 用户扫码(码中url含有固定机器参数`mcode`),登入机器url,发送http请求,
2. 服务器识别用户请求中的User-Agent等讯息,记录`mcode`及生成对应`opid`,返回登陆请求.
  Eg:通过参数判断其来自**UC内核**还是**QQ浏览器内核**,同时加上自己还不能理解的其他判断条件,自动帮助用户登录支付宝或微信,如果用户请求中两者的信息均无, 则转到*404页面*友好提示页面. 
3. 用户接受请求, 自动登陆, 响应对应支付宝或微信账户信息.
4. 服务器接受信息并进行数据库存储,返回loading页面.
3. 用户进入到特定的一致的loading页面,只不过一个是使用支付宝账号登陆支付,一个是使用微信登陆支付.
5. 之后加载信息,加载成功后进入购买支付页面 (此时页面按钮变颜色且可按下,并跳转支付对应金额)

### 客户端与服务器端交互过程:
      |->接收404页面  
! -> 扫码    自动登陆->返回用户信息      loading页面->请求加载   购买页面->发送支付请求  
      ↓       ↑            ↓              ↑            ↓       ↑            ↓  
     识别->请求用户登陆   记录用户信息,发送loading       发送加载信息        处理支付请求  
      |->返回404页面 

### Loading页面构建:
#### Header
Header用了超级多的<span><div>标签.新的html5标签用到的有很基础的header,footer.头部logo用浮动向左缩进, 相关信息直接<br>换行,很low的做法,但会改进的.
#### Body1
Body1中用到了text-align将提示信息居中,flex布局将三个选择按钮, 用到:弹性盒子设置display: flex; 盒内元素直接flex: 1 1 auto; 同时设定相关padding,直接就可以自动伸缩了.但超过放大超过150%的时候就会溢出, 主要是固定了按钮的大小, 则不具有伸缩性了(会尝试改进).
#### Body2
Body2中主要也是margin与padding的使用,以及clear:both的应用.但有遇到一个问题是[卡余额￥***]用span元素标记的话无法清除浮动,要使用段落p元素进行标记才行.

### 以上只是将网页端的基本表示层构建好, 但在手机上一塌糊涂````
### 应该是更改了网页的meta信息的缘故!
### 主要完成屏幕尺寸的设定以及meta信息的注入!了解! 加油! flexbox的总结!
