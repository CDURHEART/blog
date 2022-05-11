---
title: 前端常用设计模式
tags: 设计模式
key: 7356e307-c6b3-4f94-a11c-09d1a8b794a0
---

> 什么是设计模式？怎么看待设计模式？啥！前端也有设计模式？🤔 在我看来设计模式很有可能就是一堆套路，而这个路，就好像是那个因为人走得多了才有了路的那个路。有点绕啊 🤣，简单来说设计模式是一套特定情况下程序开发的最佳实践<!--more-->。通常被有经验的面向对象的软件开发人员所采用。

# 设计原则

在去寻找如何去学习设计模式时，我发现了一个东西叫设计原则，它是一个很好的设计模式的指导，我想...在学习设计模式之前，得先了解一下它。

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

   如果其中一个类需要调用另一个类的某一个方法的话，可以通过第三者转发这个调用。

6. **合成复用原则（Composite Reuse Principle）**

   合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。

7. **单一职责原则（Single responsibility principle）**

   它规定一个类应该只有一个发生变化的原因。

# 设计模式

将设计模式可以主要分为三大类：**_创建型模式（Creational Patterns）、结构型模式（Structural Patterns）、行为型模式（Behavioral Patterns）_**。

## 创建型

### 工厂模式

这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。在工厂模式中，我们在创建对象时不会对外暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。<br/>

**意图：** 定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。<br/>
**主要解决：** 主要解决接口选择的问题。<br/>
**何时使用：** 我们明确地计划不同条件下创建不同实例时。<br/>
**如何解决：** 让其子类实现工厂接口，返回的也是一个抽象的产品。<br/>
**关键代码：** 创建过程在其子类执行。<br/>

**实例：**<br/>
> BMW，需要生产多种产品，x5、x6等。<br/>

不使用工厂实现：
```javascript
// BMW类
class Bmw {
  constructor(model, price, maxSpeed) {
    this.model = model;
    this.price = price;
    this.maxSpeed = maxSpeed;
  }
}

const bmwX5 = new Bmw('X5', 108000, 300);
const bmwX6 = new Bmw('X6', 111000, 320);
```

使用工厂模式：

```javascript
// BMW工厂
class BmwFactory {
  static create(type) {
    if (type === 'X5') {
      return new Bmw(type, 108000, 300);
    }
    if (type === 'X6') {
      return new Bmw(type, 111000, 320);
    }
  }
}

// BMW类
class Bmw {
  constructor(model, price, maxSpeed) {
    this.model = model;
    this.price = price;
    this.maxSpeed = maxSpeed;
  }
}

const bmwX5 = BmwFactory.create('X5');
const bmwX6 = BmwFactory.create('X6');
```

**优点：**

1. 一个调用者想创建一个对象，只要知道其名称就可以了。
2. 扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。
3. 屏蔽产品的具体实现，调用者只关心产品的接口。

**缺点：** 每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。

<!-- ### 单例模式 -->

## 结构型

### 装饰器模式

装饰器模式（Decorator Pattern）允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。<br/>

**意图：** 动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。<br/>
**主要解决：** 一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。<br/>
**何时使用：** 在不想增加很多子类的情况下扩展类。<br/>
**如何解决：** 将具体功能职责划分，同时继承装饰者模式。<br/>

**实例：**<br/>
> 针对汽车服务项目，例如洗车服务，可能需要给它附加一些额外的销售策略。比如洗车可以附加一些服务，或者可以让洗车服务参与优惠活动。在这种场景下:

``` javascript
// 汽车服务类
class CarService {
  constructor() {
    this.price = 0;
  }
  getPrice() {
    return this.price;
  }
}

// 洗车服务类
class CarWash extends CarService {
  constructor() {
    super();
    this.price = 8;
  }
}

// 汽车服务装饰器
class CarServiceDecorator extends CarService {
  constructor(carService) {
    super();
    this.carService = carService;
  }

  getPrice() {
    return this.carService.getPrice();
  }
}

// 附加服务装饰器
class AffixDecorator extends CarServiceDecorator {
  constructor(carService) {
    super(carService);
  }

  getPrice() {
    return super.getPrice() + 6;
  }
}

// 折扣装饰器
class DiscountDecorator extends CarServiceDecorator {
  constructor(carService) {
    super(carService);
  }

  getPrice() {
    return super.getPrice() * 0.9;
  }
}

const carWash = new CarWash();
console.log(carWash.getPrice()); // 8

const carWashAffix = new AffixDecorator(new CarWash());
console.log(carWashAffix.getPrice()); // 14

const carWashDiscount = new DiscountDecorator(new AffixDecorator(new CarWash()));
console.log(carWashDiscount.getPrice()); // 12.6
```

较新的 ES 标准 或者 TypeScript 中能找得到[装饰器语法](https://www.typescriptlang.org/docs/handbook/decorators.html){:target="_blank"}，尽管目前它可能还是实验性的。如果熟悉 Angular ，应该了解其中装饰器已经被大量使用，`@Component()`、`@Module()` 等内置装饰器，正是装饰器模式的一种体现。

``` typescript
import { Component } from '@angular/core';

@Component({
  selector: 'hello-world',
  template: `
    <h2>Hello World</h2>
    <p>This is my first component!</p>
  `
})
export class HelloWorldComponent {
  // The code in this class drives the component's behavior.
}
```

**优点：** 装饰类和被装饰类可以独立发展，不会相互耦合，装饰器模式是继承的一个替代模式，装饰器模式可以动态扩展一个实现类的功能。<br/>
**缺点：** 多层装饰比较复杂。<br/>

<!-- ### 代理模式 -->

## 行为型

### 策略模式

在策略模式（Strategy Pattern）中，一个类的行为或其算法可以在运行时更改。这种类型的设计模式属于行为型模式。<br/>
在策略模式中，我们创建表示各种策略的对象和一个行为随着策略对象改变而改变的 context 对象。策略对象改变 context 对象的执行算法。<br/>

**意图：**定义一系列的算法，把它们一个个封装起来， 并且使它们可相互替换。<br/>
**主要解决：**在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。<br/>
**何时使用：**一个系统有许多许多类，而区分它们的只是它们直接的行为。<br/>
**如何解决：**将这些算法封装成一个一个的类，任意地替换。<br/>
**关键代码：**实现同一个接口。<br/>

**实例：**<br/>

> 实现购物车类，让它可以根据不同客户类型，计算不同的折扣。如果是普通客户，没有折扣；如果是 VIP，享受 9 折优惠；如果是 VVIP，享受 8 折优惠。

上来就写，我们可能会写成这样：

```javascript
// 购物车类
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
console.log(result); // 80
```

使用策略模式后：

```javascript
// 购物车类
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
**优点：**

1. 算法可以自由切换。
2. 避免使用多重条件判断。
3. 扩展性良好。

**缺点：**

1. 策略类会增多。
2. 所有策略类都需要对外暴露。

### 观察者模式

当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。比如，当一个对象被修改时，则会自动通知依赖它的对象。观察者模式属于行为型模式。<br/>

**意图：**定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。<br/>
**主要解决：**一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。<br/>
**何时使用：**一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知，进行广播通知。<br/>
**如何解决：**使用面向对象技术，可以将这种依赖关系弱化。<br/>

**实例：**<br/>
> 定义产品类，它有一个价格属性，当价格改变时，通知所有的观察者。

```javascript
// 被观察者类
class Product {
  constructor() {
    this.price = 0;
    this.actions = [];
  }

  // 更新价格并通知
  setBasePrice(val) {
    this.price = val;
    this.notifyAll();
  }

  // 注册观察者动作
  register(observer) {
    this.actions.push(observer);
  }

  // 取消注册动作
  unregister(observer) {
    this.actions = this.actions.filter(el => !(el instanceof observer));
  }

  // 通知动作
  notifyAll() {
    return this.actions.forEach(el => el.update(this));
  }
}

// 观察者类 Fees
class Fees {
  update(product) {
    // product.price = product.price * 1.2;
    console.log('Fees: product.price :>> ', product.price);
  }
}

// 观察者类 Proft
class Proft {
  update(product) {
    // product.price = product.price * 2;
    console.log('Proft: product.price :>> ', product.price);
  }
}

const product = new Product();

const fees = new Fees();
const proft = new Proft();

// 注册观察者
product.register(new Fees());
product.register(new Proft());

product.setBasePrice(100);

// 输出：
// Fees: product.price :>>  100
// Proft: product.price :>>  100

product.unregister(Fees);

product.setBasePrice(200);

// 输出：
// Proft: product.price :>>  200
```
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