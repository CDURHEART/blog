---
title: Angular 变更检测与变更检测策略
tags: Angular
key: 703f1263-a516-41c6-ab28-40f0e9fb73c2
---

# Angular 变更检测与变更检测策略

## 变更检测（Change Detection）

```ts
@Component({
  template: `<div (click)="data = 'B'">{{ data }}</div>`,
})
export class DemoComponent {
  data = "A";
}
```

点击 Demo 组件 A ，页面会直接把 A 改成 B，看似很简单的一个改动，其实在 Angular 内部涉及到很多复杂的操作，包括 **`变更检测`** 、**`脏数据检查`** 、**`数据绑定`** 、**`单向数据流`** 、**`更新DOM`** 、**`NgZone`** 等等。

**Angular 应用其实就是组件树，Angular 会在需要更新视图的时候，沿着组件树从 `根组件` 开始至上而下执行变更检测（`单向数据流`），从而更新视图。**

> 某些情况导致数据变化了 => 触发变更检测 => 某些地方需要更新视图 => 更新视图

### 什么情况导致数据变化了/哪些行为会引起 Angular 的变更检测？

- DOM 事件：页面 click、submit、mouse down...
- 计时器：setTimeout()、setInterval()
- XHR：从服务端获取数据
- Promise

  只要发生了异步操作，Angular 就会认为有状态可能发生变化了，然后就会进行变更检测。

### Angular 怎么通知各个组件做变更检测？

**`NgZone（zone.js）`** 干的

NgZone 知道异步操作导致状态已经发生改变，通知 Angular 触发变更检测从而更新页面 DOM。

NgZone 可以简单的理解为是一个异步事件拦截器，它能够 hook 到异步任务的执行上下文，然后就可以来处理一些操作，比如每个异步任务 callback 以后就会去通知 Angular 做变更检测。

Angular 源码中有一个 ApplicationRef，可以监听 NgZones onTurnDone 事件，每当 onTurnDone 被触发后，它会立马执行 tick()方法，tick()会从上到下沿着组件树触发变更检测。ApplicationRef 简洁版代码如下：

```ts
// very simplified version of actual source
class ApplicationRef {
  changeDetectorRefs:ChangeDetectorRef[] = [];

  constructor(private zone: NgZone) {
    this.zone.onTurnDone
      .subscribe(() => this.zone.run(() => this.tick());
  }

  tick() {
    this.changeDetectorRefs
      .forEach((ref) => ref.detectChanges());
  }
}
```

每个 component 都有自己的变更检测器，负责检查它们各自的绑定。有了 NgZone 异步事件都会导致整个 Angular 应用发生变更检测。

### 怎知某些地方需要更新视图？

Angular 编译时：ngc (Angular Compiler) 会生成\*.ngfactory.js，其实 ngfactory 就是 `Component View` 组件视图。

在\*.ngfactory.js 过程中，ngc 会把所有可能发生变化的 `DOM Nodes/Elements` 都找出来，然后给这些 `DOM Nodes/Elements` 生成 `Bindings`，这些 `Bindings` 里会记录 `Element Name/Expression/OldValue`。

一旦符合触发条件，就会被 ngZone 捕获到，然后触发变更检测，也就是会从 `根组件` 开始，从上到下检查所有组件的 `Bindings`，对比 `NewVaule` 和 `OldValue`，如果不一致就会把新值更新到页面，同时把新值更新为旧值（这也就是我们经常提到的脏检查机制 Dirty Checking）

这大概就是 Angular 为什么能做到绑定的值一旦触发变化就会自动更新视图，也就是变更检测 Angular Change Detection

## 变更检测策略（Change Detection Strategy）

Angular 变更检测本身性能已经很好了，在毫秒内可以做成百上千次变更检测。但是随着项目越来越大，其实很多不必要的变更检测还是会在一定程度上影响性能，所以得有不同应对策略。

变更检测有两种策略：

```ts
enum ChangeDetectionStrategy {
  OnPush: 0
  Default: 1
}
```

### Default

Angular 默认的变更检测机制是 `ChangeDetectionStrategy.Default`：使用默认的 CheckAlways 策略，在该策略中，变更检测将自动执行，直到显式停用为止。异步事件 callback 结束后，NgZone 会触发整个组件树至上而下做变更检测，

### OnPush

但是在实际应用里，并不是每个异步操作需要变更检测，某些组件也可以完全不用做变更检测，应用越大页面越复杂，过多的变更检测会影响整个应用的性能。

`ChangeDetectionStrategy.OnPush`，使用 CheckOnce 策略，这意味着把此策略设置为 Default（CheckAlways ）将禁用自动变更检测，直到重新激活。变更检测仍然可以显式调用。此策略适用于所有子指令，并且不能被覆盖。

```ts
@Component({
    selector: "child",
    template: `<h3>here is email in the child: { { data.contact.email } } </h3>`,
    changeDetection: ChangeDetectionStrategy.OnPush
})
...
```

简单说，组件用 OnPush 策略，可以跳过以及它下面所有子组件之后的变更检测，但也不能就此不能变更检测了，

以下四种情况还是可以触发该组件变更检测的：

1. 组件的`@Input` 引用发生变化

   ```ts

   @Input() data:any; // {a:1,b:2}

   // data变化：
   // data.a=0：不触发
   // data={a:0,b:'newB"}：触发
   ```

1. 组件的 DOM 事件，包括它子组件的 DOM 事件，比如 click、submit、mouse down...

1. `Observable` 订阅事件，同时设置 `Async pipe`

   ```ts
   @Component({
     selector: "child",
     template: `<h3>here is email in the child: { { data.contact.email } }</h3>
       <h3>here is counter in the child: { { count$ | async } }</h3>
       <div style="margin-bottom:10px;">
         <button (click)="changeCounter()">change child counter</button>
       </div>`,
     changeDetection: ChangeDetectionStrategy.OnPush,
   })
   export class ChildComponent implements OnInit, OnChanges {
     @Input() data: any;
     counter: number = 1;
     count$: Observable<number>;

     ngOnInit() {
       this.count$ = interval(1000).pipe(map((count: number) => ++count));
     }
     changeCounter() {
       this.counter++;
     }
     ngOnChanges() {
       console.log(
         "data has been changed: " + this.data.name + " " + this.data.address
       );
     }
   }
   ```

1. 利用以下方式手动触发变更检测：
   ```ts
   ChangeDetectorRef.detectChanges(); // 立马触发当前组件和它子组件变更检测
   ChangeDetectorRef.markForCheck(); // 标记需要被变更检测，在当前或下一轮的变更检测中被触发
   ApplicationRef.tick(); // 触发整个应用的组件树从上到下执行变更检测
   ```
