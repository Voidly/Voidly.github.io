---
layout: post
title: "《重构》阅读笔记"
description: 
modified: 2015-09-27
category: books-note
tags: [books-note]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}
##重构
最近看了重构这本书，说实在的，以前虽然经常也重构，但是并没有什么所谓的章法以及流程，也就是说其实就是野路子，然而看了这本书之后，才知道原来好多的方法我也在经常使用，而还有的方法我没有使用，如果使用了效果会更好。下面简单的把阅读笔记给写出来。

##笔记

- 定义(名词)：对软件内部结构的一种调整，目的是在不改变软件可观察行为的前提下，提高其可理解性，降低其修改成本。

- 定义(动词)：使用一系列手法，在不改变软件可观察行为的前提下，调整其结构。

- 有关重构：

    - 重构技术就是以微小的步伐修改程序，如果你犯下错误，很容易就可以发现它。

    - 写出人类易懂的代码才是优秀的程序员。

    - 代码应该表现自己的目的。好的代码应该清楚的表达自己的功能，变量名称是代码清晰的关键。

- 三次法则：

    - 事不过三，三则重构

- 什么时候重构：

    - 添加功能时

    - 修补错误时

    - 复审代码时

- 程序难以相与：

    - 难以阅读

    - 逻辑重复

    - 添加新行为需要修改已有代码

    - 带有复杂条件逻辑

- 愿景：

    - 易阅读

    - 所有逻辑在唯一地点指定

    - 新改动不会危及现有行为

    - 尽可能简单表达条件逻辑

- 哪怕你完全了解系统，也请实际度量它的性能，不要臆测。臆测会让你学到一些东西，但十有八九你是错的。

- 编写高性能软件的方法：

    - 时间预算法。为每个组件预分配一定资源：时间&执行轨迹。

    - 持续关注法。在每个方面都保持系统高性能。缺点：不关注整体只关注局部，过于狭隘，优化的部分可能很多很少被执行。90%优化都是白费功夫。

    - 利用上述90%的数据。
    
##代码坏味道

代码坏味道

- Duplicated Code（重复代码）
- Long Method（过长函数）
- Large Class(过大的类)
- Long Parameter List(过长参数列)
- Divergent Change(发散式变化)
- Shotgun Surgery(霰弹式修改)
- Feature Envy（依恋情结）
- Data Clumps(数据泥团)
- Primitive Obsession(基本类型偏执)
- Switch Statements(switch 惊悚现身)
- Parallel Inheritance Hierarchies(平行继承体系)
- Lazy Class(冗赘类)
- Speculative Generality (夸夸奇谈未来性)
- Temporary Field(令人迷惑的暂时字段)
- Message Chains(过度耦合的消息链)
- Middle Man(中间人)
- Inappropriate Intimacy(狎昵关系)
- Alternative Classes with Different Interface(异曲同工的类)
- Incomplete Library Class(不完美的库类)
- Data Class(纯维的数据类)
- Refused Bequest(被拒绝的遗赠)
- Comments（过多的注释）

####重构之根本

- 改进软件设计

- 易于理解

- 找bug

- 提高编程效率，加快开发

##重构常用方法


1．重新组织函数

- 1.1 Extract Method(提炼函数)
- 1.2 Inline Method(内联函数)
- 1.3 Inline Temp(内联临时变量)
- 1.4 Replace Temp with Query(以查询取代临时变量)
- 1.5 Introduce Explaining Variable(引入解析性变量)
- 1.6 Split Temporary Variable(分解临时变量)
- 1.7 Remove Assignments to Parameters(移除对参数的赋值)
- 1.8 Replace Method with Method Object(以函数对象取代函数)
- 1.9 Substitute Algorithm(替换算法)

2．在对象之间搬移特性

- 2.1 Move Method(搬移函数)
- 2.2 Move Field(搬移字段)
- 2.3 Extract Class(提炼类)
- 2.4 Inline Class(将类内联化)
- 2.5 Hide Delegate(隐藏“委托关系”)
- 2.6 Remove Middle Man(移除中间人)
- 2.7 Introduce Foreign Method(引入外加函数)
- 2.8 Introduce Local Extension(引入本地拓展)

3．重新组织数据

- 3.1 Self Encapsulate Field(自封装字段)
- 3.2 Replace Data Value with Object(以对象取代数据值)
- 3.3 Change Value to Reference(将值对象改为引用对象)
- 3.4 Change Reference to Value(将引用对象改为值对象)
- 3.5 Replace Array with Object(以对象取代数组)
- 3.6 Duplicate Observed Data(复制“被监视数据”)
- 3.7 Change Unidirectional Association to Bidirectional(将单向关联改为双向关联)
- 3.8 Change Bidirectional Association to Unidirectional(将双向关联改为单向关联)
- 3.9 Replace Magic Number with Symbolic Constant(以字面常量取代魔法数)
- 3.10 Encapsulate Field(封装字段)
- 3.11 Encapsulate Collection(封装集合)
- 3.12 Replace Record with Data Class(以数据类取代记录)
- 3.13 Replace Type Code with Class(以类取代类型码)
- 3.14 Replace Type Code with Subclasses(以子类取代类型码)
- 3.15 Replace Type Code with State/Strategy(以state/Strategy取代类型码)
- 3.16 Replace Subclass with Fields(以字段取代子类)

4．简化条件表达式

- 4.1 Decompose Conditional(分解条件表达式)
- 4.2 Consolidate Conditional Expression(合并条件表达式)
- 4.3 Consolidate Duplicate Conditional Fragments(合并重复的条件片段)
- 4.4 Remove Control Flag(移除控制标记)
- 4.5 Replace Nested Conditional with Guard Clauses(以卫语句取代嵌套条件表达式)
- 4.6 Replace Conditional with Polymorphism(以多态取代条件表达式)
- 4.7 Introduce Null Object(引入Null对象)
- 4.8 Introduce Assertion(引入断言)

5．简化函数调用

- 5.1 Rename Method(函数重命名)
- 5.2 Add Parameter(添加参数)
- 5.3 Remove Parameter(移除参数)
- 5.4 Separate Query from Modifier(将查询函数)
- 5.5 Parameterize Method(令函数携带参数)
- 5.6 Replace Parameter with Explicit Method(以明确函数取代参数)
- 5.7 Preserve Whole Object(保持对象完整)
- 5.8 Replace Parameter with Methods(以函数取代参数)
- 5.9 Introduce Parameter Object(引入参数对象)
- 5.10 Remove Setting Method(移除设值函数)
- 5.11 Hide Method(隐藏函数)
- 5.12 Replace Constructor with Factory Method(以工厂函数取代构造函数)
- 5.13 Encapsulate Downcast(封装向下转型)
- 5.14 Replace Error Code with Exception(以异常取代错误码)
- 5.15 Replace Exception with Test(以测试取代异常)

6．处理概括关系

- 6.1 Pull Up Field(字段上移)
- 6.2 Pull Up Method(函数上移)
- 6.3 Pull Up Constructor Body(构造函数本体上移)
- 6.4 Push Down Method(函数下移)
- 6.5 Push Down Field(字段下移)
- 6.6 Extract Subclass(提炼子类)
- 6.7 Extract Superclass(提炼超类)
- 6.8 Extract Interface(提炼接口)
- 6.9 Collapse Hierarchy(折叠继承体系)
- 6.10 Form Tem Plate Method(塑造模板函数)
- 6.11 Replace Inheritance with Delegation(以委托取代继承)
- 6.12 Replace Delegation with Inheritance(以继承取代委托)