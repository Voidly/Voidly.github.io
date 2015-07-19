title: 设计模式之第6章-迭代器模式(Java实现)
date: 2015-01-19 12:18:29
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“我已经过时了，就不要讲了吧，现在java自带有迭代器，还有什么好讲的呢？”“虽然已经有了，但是具体细节呢？知道实现机理岂不美哉？”“好吧好吧。”（迭代器闷闷不乐的答应下来。作者吃着小笼包，咂咂嘴道：哼，想偷懒，窗户都没有~）。

##迭代器模式之自我介绍

正如你们所见，我目前已经没落了，基本上没人会单独写一个迭代器，除非是产品性质的研发，我的定义如下：Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.意思呢，就是：提供一种方法，访问一个容器对象中各个元素，而又不需要暴露该对象的内部细节。

迭代器是为容器服务的，那么什么又是容器呢？所谓容器就是能容纳对象的所有的类型的统称。例如Collection集合类型、Set类型等等，这些在《Thinking in Java》里面的第Chapter 11里面有所介绍，想要深入了解容器的话还需要看Chapter 17，如果没有记错的话应该是这个，好了，继续，我呢，就是为了解决遍历这些容器而诞生的，当然，这些都是JDK1.0.8时代的事情了，现在jdk中早已有了迭代器的实现，所以呐，我也就是个过时的产物~下面上类图先：

![](http://images.cnitblog.com/blog/666211/201501/182009095265924.png)

我提供了便利容器的方便性，容器只要管理增减就可以了，需要遍历时交给我。下面具体分析类图：

* Iterator抽象迭代器：抽象迭代器负责定义访问和遍历元素的接口，而且基本上有固定的三个方法：first()获取第一个元素，next()获取下一个元素，isDone（）是否访问完了。Java中是hasNext()。
* ConcreteIterator具体迭代器：实现迭代器接口，完成容器元素遍历。
* Aggregate抽象容器：负责提供创建具体的迭代器角色接口。
* ConcreteAggregate具体容器：具体容器实现容器接口定义的方法，创建出容纳迭代器的对象。

##迭代器模式之自我分析

缺点么，还真没有，优点如下：

* 支持以不同的方法遍历一个聚合。
* 简化了聚合的接口。
* 在同一个聚合上可以有多个遍历。

##迭代器之实现

既然如此的不是很常用，那就直接进行通用代码的实现吧，首先是抽象迭代器：

	public interface Iterator{
	    //遍历下一个元素
	    public object next();
	    //是否已经到最后一个元素
	    public boolean hasNext();
	    //删除当前指向的元素
	    public boolean remove();
	}

接下来是具体的迭代器实现代码：　

	public class ConcreteIterator implements Iterator{
	    private Vector vector = new Vector();
	    //定义当前游标
	    public int cursor  = 0;
	    @SuppressWarnings("unchecked")
	    public ConcreteIterator(Vector vector){
	        this.vector = vector;
	    }
	
	    //判断是否是最后一个
	    public boolean hasNext(){
	        if(this.cursor == this.vector.size()){
	            return false;
	        }
	        else
	            return true;
	    }
	
	    //返回下一个元素
	    public Object next(){
	        Object result = null;
	        if(this.hasNext()){
	            result = this.vector.get(this.cursor++);
	        }
	        else
	            result = null;
	
	        return result;
	    }
	
	    //删除当前元素
	    public boolean remove(){
	        this.vector.remove(this.cursor);
	        return true;
	    }
	
	
	}

然后是抽象容器类：

	public interface Aggregate{
	    //是容器必有的元素增加
	    public void add(Object object);
	    //减少元素
	    public void remove(Object object);
	    //由迭代器遍历
	    public Iterator iterator():
	}

最后是具体容器的实现类：

	public class ConcreteAggregate implements Aggregate{
	    //容器对象的容器
	    private Vector vector = new Vector();
	    //增加一个元素
	    public void add(Object object){
	        this.vector.add(object);
	    }
	    //减少一个元素
	    public void remove(Object object){
	        this.remove(object);
	    }
	    //返回迭代对象
	    public void add(Object object){
	        return new ConcreteIterator(this.vector);
	    }
	}

　以上就是具体的通用方法的实现了。

##迭代器模式应用场合

虽然我这种模式不再使用，但是迭代这种容器却是使用的很频繁的，经常编程的你们想必也见到过很多~下面就来介绍一下具体的使用场合，其实我可以用来：

* 访问一个聚合对象的内容而无需暴露它的内部表示。
* 支持对聚合对象的多种遍历。
* 为遍历不同的聚合结构提供一个统一的接口（即：支持多态迭代）。
　　
以上。欲知后式为何物，且听下回分解。



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**