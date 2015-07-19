---
title: 设计模式之第0章-单例模式(Java实现)
layout: post
date: 2015-01-15 21:57:17
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
comments: true
---
当当当当~首先有请最简单的单例模式登场，先来个自我介绍吧。
##单例模式之自我介绍
我，单例模式（Singleton Pattern）是一个比较简单的模式，我的定义如下：

Ensure a class has only one instance,and provide a global point of access to it.（确保其某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。）单例模式的通用类图如下：

![](http://images.cnitblog.com/blog/666211/201501/152050242295434.png)

Singleton类称为单例类，通过使用private的构造函数确保了在一个应用中只产生一个实例，并且是自行实例化的。
##单例模式之自我分析
做人，哦不，做模式不能自满，所以先谈谈我的缺点，我的缺点如下：

- 我一般没有接口，扩展很困难，如果想扩展的话，除了修改代码基本上没有第二种方式。当然了，特殊情况下，我也可以实现接口、被继承等，这个需要你们自行判断。
- 测试不便。在并行开发中，如果你们没有把我完成，那么是没有办法进行测试的。
- 还有一点，我与单一职责原则也有冲突，一个类本应实现一个逻辑，不该关心是否是单例的，是不是要单例取决于环境，但是我把单例和业务逻辑融合在一个类中了。

终于该说优点了~挺好了，比缺点可是要多的说：

* 对唯一实例的受控访问。由于我的类封装了我唯一的实例，所以可以严格控制用户以及何时访问它。
* 缩小名空间。我可以避免那些存储唯一实例的全局变量污染名空间。
* 允许对操作和表示的精化。
* 允许可变数目的实例。
* 比类操作更灵活。

##单例模式之实现

俗话说，说的比唱的好听。为了证明我唱的也很好听，接下来我就以人类的一夫一妻制来具体讲解一下如何实现我的模式~一夫一妻制又称作”单偶婚“，“个体婚”，据说一夫一妻制起源于秦始皇统一天下之后，自大秦一统天下，中国酒实行了一夫一妻制，尤其是在汉朝“罢黜百家，独尊儒术”开始，古人严格执行了这一制度blablabla...(此处省略n字)。咳咳，跑题了，我们接着谈一夫一妻制，哦不，单例模式，首先得有个妻子类：

	public class Wife{
	
	    private static final Wife wife = new Wife();
	    private Wife(){
	
	    }
	    public static Wife getInstance(){
	        return wife;
	    }
	    public static void say(){
	        System.out.println("I am Y's wife!");
	    }
	}

通过定义一个私有访问权限的构造函数，可以避免被其他类new出来一个对象，而Wife自己可以new一个对象出来，其他的类对该类的访问可以通过getInstance获得一个对象。妻子有了，老公Y自然要出场了，其类代码如下：

	public class Y{
	    public static void main(String[] args) {
	        for (int day=0; day<3;day++ ) {
	            Wife wife = Wife.getInstance();
	            wife.say();
	        }
	    }
	}	

运行结果如下：

　　I am Y's wife!

　　I am Y's wife!

　　I am Y's wife!

　　Y每天回家见到的妻子，都是同一个妻子，不会出现开门后，一看，呦呵，老婆怎么变了？如有此情况，请速与我联系！

##单例模式之优化OR问题

有人嫌这样麻烦，因为无论是都使用这个类，都会创建一个instance对象，如果创建这个很耗时，比如需要连接10**9（python的10的9次方写法）次数据库，并且还不一定使用，那该这么办？于是乎有“聪明人”想到如下的方法：　

	public class Singleton {
	    private static Singleton instance;
	    private Singleton (){}
	
	    public static Singleton getInstance() {
	    if (instance == null) {
	        instance = new Singleton();
	    }
	    return instance;
	    }
	}

是不是感觉很不错，很好？这个是传说中的懒汉模式其实这个有个很大的问题，如果是高并发情况下，可能A线程在创建实例，但是还没获取对象，B此时也在执行，判断也为真，所以又获得一个对象，如此下去，你的妻子会越来越多！没办法了么?不，当然有，且往下看：

	public class Singleton {
	    private static Singleton instance;
	    private Singleton (){}
	    public static synchronized Singleton getInstance() {
	    if (instance == null) {
	        instance = new Singleton();
	    }
	    return instance;
	    }
	}

这种写法确实解决了问题，但是效率么，啧啧，99%情况下不需要同步我会告诉你么？

##奇技淫巧：单例模式之反射实现
	public class Singleton{
	    private static Singleton singleton;
	    static{
	        try{
	            class cl = class.forName(Singleton.class.getName());
	            //获得无参构造
	            Constructor con = cl.getDeclaredConstructor();
	            //设置无参构造是可访问的
	            con.setAccessible(true);
	            //产生一个实例对象
	            singleton = (Singleton)con.newInstance();
	        }
	        catch(Exception e)
	        {
	            
	        }
	    }
	
	    public static Singleton getSingleton(){
	        return singleton;
	    }
	}

通过获得类构造，然后设置访问权限，生成一个对象，然后提供外部访问，保证内存对象单一。

##奇技淫巧：单例模式之枚举类型实现

其实，从Java1.5发行版起，实现Singleton还有一种方法，只需要编写一个包含单个元素的枚举类型：

	//Enum singleton - the preferred approach
	public enum Singleton{

		INSTANCE;

	}

这种方法在功能上与公有域方法相近，但是它更为简洁，无偿的提供了序列化机制，绝对防止多次实例化，即使是在面对复杂的序列化或者反射攻击的时候。**虽然此方法未被采用，但是单元素的枚举类型已经成为了实现Singleton的最佳方法。**


以上就是我，单例模式，谢谢大家~下回就是我的大哥来做演讲了，欲知后事如何，且听下回分解。


*枚举类型实现方法更于15年3月12日*

---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
