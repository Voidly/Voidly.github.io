---
title: 设计模式之第1章-工厂方法模式(Java实现)
layout: post
date: 2015-01-16 18:00:18
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
comments: true
---
“我先来”，“不，老公，我先！”。远远的就听到几个人，哦不，是工厂方法模式和抽象工厂模式俩小夫妻在争吵，尼妹，又不是吃东西，谁先来不都一样（吃货的世界~）。“抽象工厂模式，赶紧的自我介绍，工厂方法模式，你身为男人，要懂得绅士风度，lady first懂不懂，稍后再来，急什么。”（画外音:鱼哥，这是我家祖传的小吃，还有我爹的好酒blablabla），“哎呀，那个抽象工厂模式，阿姨喊你回家吃饭了。”“哦，我去去就回，等我啊。”工厂方法，赶紧的。“等等，鱼哥，上次我还没说完，给我一分钟先，让我把应用场景给说了。”“恩，唔唔”（画外音：嘴里塞满老北京糖葫芦以及其它各色小吃的作者已被众人拖走）、、、

##福利：单例模式之应用场景

在下面的情况下，就轮到我出场了：

* 当类中只能有一个实例而且客户可以从一个众所周知的访问点访问它时。
* 当这个唯一的实例应该是通过子类化可扩展的，并且客户应该无需要更改代码就能使用一个扩展的实例时。

好了，有关我的就此结束，工厂方法，你来吧。

##工厂方法模式之自我介绍
好模式就是我，我就是好模式（挑眉）：工厂方法~大家鼓掌，（PS：等了几秒，鸦雀无声，群众：没有绅士风度的家伙还好男人，我勒个去，要脸吗），咳咳，那个，作为一个开发者，日常开发中我的出境率可是很高的，经常能看到我的身影，定义嘛如下：Define an interface for creating an object,but let subclasses decide which class to instantiate.Factory Method lets a class defer instantiation to subclasses.知道是什么意思不？（PS：就算英语再好，可是你那一口纯正的Chinglish让我们也是听得醉了）。不知道吧，我给你们翻译一下，这句话的意思嘛，就是：定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。还不懂？那上图说话：

![](http://images.cnitblog.com/blog/666211/201501/161554516202574.png)

上图就是工厂方法模式的通用类图。

在我的模式中，抽象产品类Product负责定义产品的共性，实现对事物最抽象的定义，Creator为抽象创建类，也就是抽象工厂，具体如何创建产品类是由具体的实现工厂ConcreteCreator完成的。我的子孙比较多（PS：即变种比较多），好了自我介绍到此结束，下面就是展示我优秀的品格的时候了。

##工厂方法模式之自我分析

　我这人最大的缺点就是没有缺点，完美真不好，想谦虚一下就不行，不信？那你可挺好了：

* 首先呢，封装性比较好，所以工厂方法使得代码结构清晰，利于维护。
* 其次嘛，扩展性也是超赞，想增加一个产品，某问题了，不要重构，不要复杂，只需要扩展一个工实现拥抱变化。
* 再次了，能屏蔽产品类的具体实现，使用的人不需要操心具体如何实现，只要调用接口即可，只要，那么上层模块就不会改变。
* 最后嘛，我可是典型的解耦框架，符合传说中的迪米特法则，依赖倒置原则。

缺点嘛，真心没有的说。

"你骗人，你要是不说，我就把你偷看***"，“停，打住，我说，我刚刚只是开个玩笑罢了，何必当真呢，俗话说，模式非圣贤，孰能无缺点嘛。”

缺点：

* 一个潜在的缺点在于客户可能仅仅为了创建一个特定的ConcreteProduct对象，就不得不创建Creator的子类。

##工厂方法模式之实现

什么，还是不懂，好吧，服了你啦，真是和郭靖一样笨，那你看好了，我做给你看。恩，知道作者大大是个纯吃货，哦不，美食爱好者，那就来个美食工厂吧。

	public interface Food{
	     public void createFood();
	}

这个Food接口是对食物的总称，每个Food都有一个制造食物的方法。

	public class Tanghulu implements Food{
	    public void createFood(){
	        System.out.println("我是好吃的老北京糖葫芦~");
	    }
	}

上面的代码是生产老北京糖葫芦的。

	public class Latiao implements Food{
	    public void createFood(){
	        System.out.println("我是好吃的卫龙辣条~");
	    }
	}

介个就是传说中的卫龙辣条，好了现在万事具备，只欠东风，错了，东风没用，是只欠工厂。这样只要吃货作者对工厂下达指令，那么就可以生产出好吃的小吃来了。

下面是抽象工厂的实现：

	public abstract class AbstractFoodFactory{
	    public abstract <T extends Food> T createFood(Class<T> c);
	}

可以看到，美食工厂生产美食的方法输入参数是Food接口的实现类，这个和类图保持一致。在这里我们用了泛型，通过定义泛型，对createFood的输入差生限制，必须是Class类，必须是Food类。

下面是美食工厂的实现：

	public class FoodFactory extends AbstractFoodFactory{
	    public <T extends Food> T createFood(Class<T> c){
	        Food food = null;
	        try{
	            food = (T)Class.forName(c.getName()).newInstance();
	        }
	        catch(Exception e){
	            System.out.println("食物生产失败！");
	        }
	        return (T)food;
	    }
	}

最后是测试类，生产食物供鱼哥实用~

	public class Author{
	    public static void main(String[] args) {
	        AbstractFoodFactory foodFactory = new FoodFactory;
	
	        //作者大大最爱的冰糖葫芦
	        Food tanghulu = foodFactory.createFood(Tanghulu.class);
	        tanghulu.createFood();
	
	        //作者喜欢的卫龙辣条~
	        Food latiao = foodFactory.createFood(Latiao.class);
	        latiao.createFood();
	    }
	}

有了这个美食工厂，作者大大再也不用担心零食了~至此，我的工厂方法模式实现结束，下面为了不再上演单例模式那家伙的错误，我要开始介绍什么时候应该轮到我上镜，“唉唉，你干什么，抽象工厂模式，你干嘛，我的耳朵，要掉了~”，“你竟敢逼着鱼哥哥拿你的贿赂，然后逼着他骗我，让你先来介绍，走，回去跪键盘。"老婆，我错了，你让我讲完这一点再走啊，哎，你们大家帮、、、帮、、、我、、、、"，（PS：看着越走越远的小夫妻，作者咂咂嘴，摸着鼓起来的小肚子，从后面慢悠悠的走来），“嗝~”，额那个，由于工厂方法模式因故离场，应用场景就放在下次当做福利来发了，真是的，连老婆的话都不听，该跪键盘（PS：作者碎碎念），至于工厂方法模式和抽象工厂模式之间的故事，咳咳，预知后事如何，且听下回分解。

> 第一篇：[设计模式之序章-UML类图那点事儿](http://voidy.gitcafe.com/2015/01/14/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E5%BA%8F%E7%AB%A0-UML%E7%B1%BB%E5%9B%BE%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF/)

> 第二篇：[设计模式之第0章-单例模式(Java实现)](http://voidy.gitcafe.com/2015/01/15/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B9%8B%E7%AC%AC0%E7%AB%A0-%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F/)

参考书籍：GoF的《Design Patterns:Elements of Reusable Object-Oriented Software》

　　　　　秦小波的《The Zen of Design Patterns second Edition》
		  程杰的《大话设计模式》



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
