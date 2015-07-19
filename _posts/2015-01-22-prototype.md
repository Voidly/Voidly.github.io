---
title: 设计模式之第9章-原型模式(Java实现)
layout: post
date: 2015-01-22 21:32:19
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“快到春节了，终于快放假了，天天上班好累的说。”“确实啊，最近加班比较严重，项目快到交付了啊。”“话说一到过节，就收到铺天盖地的短信轰炸，你说发短信就发吧，大多数还是一样的，群发。”“就是就是，上次我收到一个，竟然连名字都给弄错了，简直没法说啊，要不然就不发得了，干嘛弄得那么麻烦。”“所以说，才会有群发的短信我不回这个段子嘛。”“对了，今天你不是就要讲那个原型模式什么的，就是类似于群发的是吧。”“嘿嘿，天机不可泄露。”（PS：还天机不可泄露，学会吊起胃口来了。）

##原型模式之自我介绍

鄙人不才，正是原型模式是也，由于实现起来巨简单，所以应用的场景那可是相当的多啊。相关的定义如下：Specify the kinds of objects to create using a prototypical instance,and create new objects by copying this prototype.转换成中文就是：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。至于类图如下所示：

![](http://images.cnitblog.com/blog/666211/201501/212114254221924.png)

是不是简单的没话说。因为它的核心也就是一个clone方法罢了，通过该方法对对象进行拷贝。而Java又提供了Cloneable接口来标示这个方法是可拷贝的，看到这里有点迷糊？标示算几个意思？因为JDK中没有方法，如果想使用的话，覆盖clone()方法才可以。

## 原型模式之自我分析

由于我想了半天都没想出什么缺点，所以只好介绍优点来着，不是我不谦虚啊，真心是没有缺点啊。（PS：不要丢鸡蛋，那边那位，泥垢了，别砸，别砸了，哎呦喂，我的脸，别砸脸成么，我就是靠这个吃饭的啊。）优点：

* 对客户隐藏了具体的产品类，减少了客户知道名字的数目。
* 运行时可以增加和删除。
* 减少子类构造。
* 用类动态配置应用，性能比较好。

##原型模式之实现

好吧，我有一个恋爱，想和你谈谈，哦，不，口误，是一个原型模式的实现，就说那个短信吧，被人群发是不是很不爽？但是不群发一个一个发又很累是不是？没关系，这时候就是我登场的时候了，让你群发，又不让别人看出来。

首先是短信的模板类：

	public class Template{
	    //短信内容
	    private String context = "恭喜发财，红包拿来";
	
	    //取得内容
	    public String getContext(){
	        return this.context;
	    }
	}

短信类代码：

	public class Message implements Cloneable{
	    //收信人名字
	    private String receiver;
	    //短信内容
	    private String context;
	    //构造函数
	    public Message (Template template)
	    {
	        this.context = template.getContext();
	    }
	
	    @Override
	    public Message clone(){
	        Message message = null;
	        try{
	            message = (Message)super.clone();
	        }
	        catch(CloneNotSupportedException e){
	            e.printStackTrace();
	        }
	        return message;
	    }
	
	    //setter/getter方法
	    public String getContext(){
	        return context;
	    }
	
	    public void setContext(String context){
	        this.context = context;
	    }
	
	    public String getReceiver(){
	        return receiver;
	    }
	
	    public void setReceiver(String receiver){
	        this.receiver = receiver;
	    }
	
	
	}	

以上就是具体的原型模式的具体实现。怎么，不知道怎么用是么？好的，来个具体的场景类：

	public class Client{
	    public static void main(String[] args) {
	        String[] name = {"Jack","Tom","voidy"};
	        Message message = new Message(new Template());
	        for (String n :name ) {
	            message.setReceiver(n);
	            Message cloneMessage = message.clone();
	            sendMessage(cloneMessage);
	        }
	    }
	    public static void sendMessage(Message message){
	        System.out.println(message.getReceiver());
	        System.out.println(message.getContext());
	    }
	}

好了，基本上就是那么多了。

##原型模式之使用场景

当遇到以下场景时，就可以考虑使用原型模式了：

* 当要实例化的类是在运行时刻指定时。如：动态装载。
* 当为了避免创建一个与产品类层次平行的工厂时。
* 当一个类的实例只有几个不同的状态组合时。

恩，本次的设计模式就到此为止。预知后式为何，且听下回分解。

PS：因水平有限，若有不对之处，欢迎指出，以防误人子弟耳。


---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**