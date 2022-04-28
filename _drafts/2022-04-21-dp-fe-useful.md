---
title: 前端常用的设计模式
tags: 设计模式
---

> 什么是设计模式？怎么看待设计模式？啥！前端也有设计模式？我们学习一个东西不应该给自己制造障碍，而是想办法让自己更容易将新知识吸收进去吧。在我看来设计模式很有可能就是一堆套路，而这个路，就像是那个因为人走的多了才有了路的那个路。简单来说设计模式是一些个最佳实践的程序开发套路，先别管对不对，因为我也不大会 😁，我们先由浅入深的学习一下，看看到底是不是这样，先从浅的来吧...

<!--more-->

# 设计原则

在去寻找如何去学习设计模式的时，我发现了一个东西叫设计原则，它是一个很好的设计模式的指导，我想...在学习设计模式之前，咱得先了解一下它。

1. **开闭原则（Open Close Principle）**

   开闭原则的意思是：对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。

2. **里氏代换原则（Liskov Substitution Principle）**

   里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

3. **依赖倒转原则（Dependence Inversion Principle）**

   这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。

4. **接口隔离原则（Interface Segregation Principle）**

   这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

5. **迪米特法则，又称最少知道原则（Demeter Principle）**

   最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

6. **合成复用原则（Composite Reuse Principle）**

   合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。

7. **单一职责原则（Single responsibility principle）**

   它规定一个类应该只有一个发生变化的原因。

# 设计模式

将设计模式可以主要分为三大类：**_创建型模式（Creational Patterns）、结构型模式（Structural Patterns）、行为型模式（Behavioral Patterns）_**。具体哪些设计模式属于哪一种类等到我们足够了解设计模式的时再回头仔细分析。先学习一下适合前端的设计模式，边学边补充。

## 创建型

### 工厂模式

### 单例模式

## 结构型

### 装饰器模式

### 代理模式

## 行为型

### 策略模式

在策略模式（Strategy Pattern）中，一个类的行为或其算法可以在运行时更改。这种类型的设计模式属于行为型模式。<br/>
在策略模式中，我们创建表示各种策略的对象和一个行为随着策略对象改变而改变的 context 对象。策略对象改变 context 对象的执行算法。<br/>

**意图：**定义一系列的算法，把它们一个个封装起来， 并且使它们可相互替换。<br/>
**主要解决：**在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。<br/>
**何时使用：**一个系统有许多许多类，而区分它们的只是它们直接的行为。<br/>
**如何解决：**将这些算法封装成一个一个的类，任意地替换。<br/>
**关键代码：**实现同一个接口。<br/>

**优点：**

1. 算法可以自由切换。
2. 避免使用多重条件判断。
3. 扩展性良好。

**缺点：**

1. 策略类会增多。
2. 所有策略类都需要对外暴露。

**实例：**<br/>

> 实现购物车类，让它可以根据不同客户类型，计算不同的折扣。如果是普通客户，没有折扣；如果是 VIP，享受 9 折优惠；如果是 VVIP，享受 8 折优惠。

上来就写，我们可能会写成这样：

```javascript
class ShoppingCart {
  constructor() {
    this.discountType = "guest";
    this.amount = 0;
  }

  // 查看折扣后的价格
  checkout() {
    if (this.discountType === "guest") {
      return this.amount;
    } else if (this.discountType === "vip") {
      return this.amount * 0.9;
    } else if (this.discountType === "vvip") {
      return this.amount * 0.8;
    }
  }

  // 设置折扣类型
  setDiscountType(type) {
    this.discountType = type;
  }

  // 设置原价
  setAmount(amount) {
    this.amount = amount;
  }
}

const cart = new ShoppingCart();
cart.setAmount(100);
cart.setDiscountType("vvip");

const result = cart.checkout();
console.log(result);
```

使用策略模式后：

```javascript
class ShoppingCart {
  constructor(discount) {
    this.discount = discount;
    this.amount = 0;
  }

  // 查看折扣后的价格
  checkout() {
    return this.discount(this.amount);
  }

  // 设置原价
  setAmount(amount) {
    this.amount = amount;
  }
}

// 普通客户策略
function guestStrategy(amount) {
  return amount;
}

// vip客户策略
function vipStrategy(amount) {
  return amount * 0.9;
}

// vvip客户策略
function vvipStrategy(amount) {
  return amount * 0.8;
}

const cart = new ShoppingCart(vipStrategy);
cart.setAmount(100);

const result = cart.checkout();
console.log(result); // 90
```

### 观察者模式

当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。比如，当一个对象被修改时，则会自动通知依赖它的对象。观察者模式属于行为型模式。<br/>

**意图：**定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。<br/>
**主要解决：**一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。<br/>
**何时使用：**一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知，进行广播通知。<br/>
**如何解决：**使用面向对象技术，可以将这种依赖关系弱化。<br/>
**优点：**
1. 观察者和被观察者是抽象耦合的。
2. 建立一套触发机制。

**缺点：**
1. 如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。
2. 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
3. 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

# 参考

- <https://www.runoob.com/design-pattern/design-pattern-intro.html>{:target="\_blank"}
- <https://juejin.cn/post/6844904138707337229>{:target="\_blank"}
- <https://github.com/fbeline/design-patterns-JS>{:target="\_blank"}
