---
layout: post
title: "kerberos简介"
description: 
modified: 2015-10-18
category: Security
tags: [Security]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

##简介

Kerberos这一名词来源于希腊神话“三个头的狗——地狱之门守护者”系统设计上采用客户端/服务器结构与DES加密技术，并且能够进行相互认证，即客户端和服务器端均可对对方进行身份认证。可以用于防止窃听、防止replay攻击、保护数据完整性等场合，是一种应用对称密钥体制进行密钥管理的系统。支持SSO。

整个认证过程涉及到三方：客户端、服务端和KDC（Key Distribution Center）。在Windows域环境中，KDC的角色由DC（Domain Controller）来担当。

某个用户采用某个域帐号登录到某台主机，并远程访问处于相同域中另一台主机时，如何对访问者和被访问者进行身份验证（这是一种双向的验证）？这就是Kerberos需要解决的场景。接下来我尽量以比较直白的语言来介绍我所知道的Kerberos认证的整个流程。

Kerberos实际上是一种基于票据（Ticket）的认证方式。客户端要访问服务器的资源，需要首先购买服务端认可的票据。也就是说，客户端在访问服务器之前需要预先买好票，等待服务验票之后才能入场。在这之前，客户端需要先买票，但是这张票不能直接购买，需要一张认购权证。客户端在买票之前需要预先获得一张认购权证。这张认购权证和进入服务器的入场券均有KDC发售。右图（点击看大图）一张图基本揭示了Kerberos整个认证的过程。

##认证流程

首先，我们来看看客户端如何获得“认购权证”。这里的认购权证有个专有的名称——TGT（Ticket Granting Ticket），而TGT的是KDC一个重要的服务——认证服务（KAS：Kerberos Authentication Service）。当某个用户通过输入域帐号和密码试图登录某台主机的时候，本机的Kerberos服务会向KDC的认证服务发送一个认证请求。该请求主要包括两部分内容，明文形式的用户名和经过加密的用于证明访问者身份的Authenticator（我实在找不到一个比较贴切的中文翻译没，Authenticator在这里可以理解为仅限于验证双反预先知晓的内容，相当于联络暗号）。

当KDC接收到请求之后，通过AD获取该用户的信息。通过获取的密码信息生成一个秘钥对Authenticator进行解密。如果解密后的内容和已知的内容一致，则证明请求着提供的密码正确，即确定了登录者的真实身份。

KAS成功认证对方的身份之后，会先生成一个用于确保该用户和KDC之间通信安全的会话秘钥——Logon Session Key，并采用该用户密码派生的秘钥进行加密。KAS接着为该用户创建“认购权证”——TGT。TGT主要包含两方面的内容：用户相关信息和Logon Session Key，而整个TGT则通过KDC自己的密钥进行加密。最终，被不同密钥加密的Logon Session Key和TGT返回给客户端。

经过上面的步骤，客户端获取了购买进入同域中其他主机入场券的“认购凭证”——TGT，以及Logon Session Key，它会在本地缓存此TGT和Logon Session Key。如果现在它需要访问某台服务器的资源，它就需要凭借这张TGT向KDC购买相应的入场券。这里的入场券也有一个专有的名称——服务票据（ST：Service Ticket）。

具体来说，ST是通过KDC的另一个服务TGS（Ticket Granting Service）出售的。客户端先向TGS发送一个ST购买请求，该请求主要包含如下的内容：客户端用户名；通过Logon Session Key加密的Authenticator；TGT和访问的服务器（其实是服务）名。

TGS接收到请求之后，现通过自己的密钥解密TGT并获取Logon Session Key，然后通过Logon Session Key解密Authenticator，进而验证了对方的真实身份。

TGS存在的一个根本的目有两点：其一是避免让用户的密码客户端和KDC之间频繁传输而被窃取。其二是因为密码属于Long Term Key（我们一般不会频繁的更新自己的密码），让它作为加密密钥的安全系数肯定小于一个频繁变换得密钥（Short Term Key）。而这个Short Term Key就是Logon Session Key，它确保了客户端和KDC之间的通信安全。

TGS完成对客户端的认证之后，会生成一个用于确保客户端-服务器之间通信安全的会话秘钥——Service Session Key，该会话秘钥通过Logon Session Key进行加密。然后出售给客户端需要的入场券——ST。ST主要包含两方面的内容：客户端用户信息和Service Session Key，整个ST通过服务器密码派生的秘钥进行加密。最终两个被加密的Service Session Key和ST回复给客户端。

客户端接收到TGS回复后，通过缓存的Logon Session Key解密获取Service Session Key。同时它也得到了进入服务器的入场券——ST。那么它在进行服务访问的时候就可以借助这张ST凭票入场了。该Serivce Session Key和ST会被客户端缓存。

但是，服务端在接收到ST之后，如何确保它是通过TGS购买，而不是自己伪造的呢？这很好办，不要忘了ST是通过自己密码派生的秘钥进行加密的。具体的操作过程是这样的，除了ST之外，服务请求还附加一份通过Service Session Key加密的Authenticator。服务器在接收到请求之后，先通过自己密码派生的秘钥解密ST，并从中提取Service Session Key。然后通过提取出来的Service Session Key解密Authenticator，进而验证了客户端的真实身份。

实际上，到目前为止，服务端已经完成了对客户端的验证，但是，整个认证过程还没有结束。谈到认证，很多人都认为只是服务器对客户端的认证，实际上在大部分场合，我们需要的是双向验证（Mutual Authentication）——访问者和被访问者互相验证对方的身份。现在服务器已经可以确保客户端是它所声称的那么用户，客户端还没有确认它所访问的不是一个钓鱼服务呢。

为了解决客户端对服务器的验证，服务要需要将解密后的Authenticator再次用Service Session Key进行加密，并发挥给客户端。客户端再用缓存的Service Session Key进行解密，如果和之前的内容完全一样，则可以证明自己正在访问的服务器和自己拥有相同的Service Session Key，而这个会话秘钥不为外人知晓。

##优点

* 身份委派：当Windows的服务为某个客户端访问资源时会扮演这个客户端，在多数情况下，在本机上一个服务能够为客户端完成访问资源的工作，因为NTLM和Kerberos都能为服务提供需要扮演客户端的信息。可是，在分布式应用被设计为前端服务扮演客户端连接到在其他服务器上的后端服务，Kerberos V5协议包括一个允许服务扮演客户端连接到其他服务器上的服务的代理机制，NTLM则没有这样的功能。


* 互操作：微软的Kerberos V5实现是基于IETF的推荐标准规范。这样，Windows Server 2003的Kerberos V5实现就为其他使用Kerberos V5协议的网络的互操作打下了基础。

* 更高效率的身份验证：对于NTLM，为了验证每一个客户端，应用服务器必须连接到域控制器以证实客户端身份。对于Kerberos V5身份验证协议，服务器不用去连接域控制器，相应的，服务器可以检验客户端提供的验证票。客户端可以为特定的服务获取一次验证票并在一次登录过程中反复使用这个验证票。可更新的会话票据（session tickets）替代了pass-through authenticatio。

* 相互身份验证：通过使用Kerberos协议，在网络连接的一端都可以验证网络另一端的声明是它自己的实体。虽然NTLM允许服务器验证客户端的身份，但是它没有提供客户端验证服务端身份的功能，也没有提供服务器验证另一个服务器身份的功能。NTLM被设计为假设服务器都是真实的网络环境，Kerberos则没有这个假设。

##缺陷

* 失败于单点：它需要中心服务器的持续响应。当Kerberos服务结束前，没有人可以连接到服务器。这个缺陷可以通过使用复合Kerberos服务器和缺陷认证机制弥补。
* Kerberos要求参与通信的主机的时钟同步。票据具有一定有效期，因此，如果主机的时钟与Kerberos服务器的时钟不同步，认证会失败。默认设置要求时钟的时间相差不超过10分钟。在实践中，通常用网络时间协议后台程序来保持主机时钟同步。
* 管理协议并没有标准化，在服务器实现工具中有一些差别。
* 因为所有用户使用的密钥都存储于中心服务器中，危及服务器的安全的行为将危及所有用户的密钥。
* 一个危险客户机将危及用户密码。

