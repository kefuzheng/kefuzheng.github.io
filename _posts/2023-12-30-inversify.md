---
title: Theia中的inversify
key: 2023-12-30
tags: theia
---

### 1. 什么是IOC
IoC 英文全称为 Inversion of Control，即控制反转   
在代码结构设计中，保证高内聚、低耦合是非常重要的，不仅仅是开发效率，包括后续的代码维护扩展效率都会收到很大的影响，尤其是在大型代码架构中，如果模块间耦合度过高，修改往往是牵一发而动全身。   
在面向对象程序设计中，我们在使用一个类功能是要先将类进行实例化，业务庞杂的功能是通过类之间的相互调用相互依赖完成的，在常规的面向对象操作中，我们往往会将依赖对象的实例化工作放在类内进行，但这就违反了单一职责原则----该类除了本身的职责外，还要负责依赖对象的实例化工作。   
而在IOC中，用第三方容器来处理对象的实例化过程。当类需要使用某个对象时，类会自动创建或获取一个实例注入到类当中，这样类与类之间没有了直接关联。对象创建和使用的控制权从类转移到了容器， 即所谓控制反转。   
![dependency](/assets/images/inversify.png)   
```ts
import { ModuleA } from './module-A';
import { ModuleB } from './module-B';

class ModuleC {
  constructor() {
    this.a = new ModuleA();
    this.b = new ModuleB();
  }
}

// 这么做会造成ModuleC依赖于ModuleA和ModuleB，产生了模块间的耦合。为了解决模块间的强耦合性，IoC的概念就产生了
// 通过使用一个容器来管理模块，这样模块之间的耦合性就降低了（下面这个例子只是模仿 IoC 的过程，Container 需要另外实现）：

// container.js
import { ModuleA } from './module-A';
import { ModuleB } from './module-B';

// Container是我们假设的一个模块容器
export const container = new Container();
container.bindModule(ModuleA);
container.bindModule(ModuleB);

// ModuleC.js
import { container } from './container';
class ModuleC {
  constructor() {
    this.a = container.getModule('ModuleA');
    this.b = container.getModule('ModuleB');
  }
}
```

##### 1. 什么是 DI
DI 英文全称为 Dependency Injection，即依赖注入。依赖注入是控制反转最常见的一种应用方式，即通过控制反转，在对象创建的时候，自动注入一些依赖对象。

### 2. InversifyJs
InversifyJs是基于TS利用依赖注入实现IOC的轻量工具库。通过Decorator+Reflect Metadata实现元数据的注入和读取。
##### 1. 装饰器（Decorator）
TypeScript中的装饰器是基于ECMAScript标准的，而装饰器提案仍处于stage2，存在很多不稳定因素，而且API在未来可能会出现破坏性的更改，所以该特性在TS中仍是一个实验性特性，默认是不启用的
```json
// 开启装饰器功能
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```
##### 2. 元数据（Reflect MetaData）
在像依赖注入这种模式下，我们会有这样的需求：在保持原有class一致性的前提下，可以为class添加metadata(元数据)，并可以方便的读取metadata，在这个过程中不会对class的结构产生任何影响。 我们会想到用装饰器+反射来注入metadata，然后通过反射来获取metadata。

### 3. 基本用法
简单理解就是有一个容器来管理所有的可注入的内容，然后使用者就直接使用装饰器语法就能获取到可注入的内容，比如下面的例子就是可注入的是类，获取到的是类实例。这样使用者就不用关心类如何实例化以及如何保证单一实例等。   
```ts
// 先声明几个可注入的类
@injectable()
class Katana {
  public hit() {
    return "cut!";
  }
}

@injectable()
class Ninja {
  @inject(Katana)
  public katana: Katana;
  
  public fight() { return this.katana.hit(); }
}

// 初始化一个容器来承载这些可注入的类
const container = new Container();
// 向容器中注入类
container.bind<Katana>(Katana).to(Katana).inSingletonScope();
container.bind<Ninja>(Ninja).to(Ninja).inSingletonScope();
// 获取类
const ninja = container.get(Ninja);
// 输入结果是cut!
console.log(ninja.fight());
```
##### 1. Container
InversifyJs的实现离不开Container的概念，Container可以看作是整个依赖注入链路的入口。容器本身就是一个类实例，而inversify要做的就是利用这么一个类实例来管理诸多别的类实例，而且依靠一套有序的方法实现。   
容器本身还有父容器和子容器的概念，所以Container对象有一个字段parent来表示，这样可以做到继承。这个概念在使用Container.resolve的时候有用到。

##### 2. ContainerModule
在theia这样的大型项目中，如果我们全部的依赖都直接绑定在Container上，显然不那么美观。而ContainerModule则是用于管理众多绑定的方法。   
通过ContainerModule，我们就可以把绑定分散到不同的模块中，可以使架构条理更清晰   
ContainerModule的构造函数只有一个registry的方法回调参数，该方法提供了一些容器的基本工具方法：bind(绑定)、unbind(解除绑定)、isbound(判断是否绑定)、rebind(重新绑定)等
```ts
// 创建一个服务标识符
const SERVICE_IDENTIFIER = Symbol.for("MyService");

// 创建一个服务实现类
class MyService {
  // ...
}

// 在容器模块中使用 bind 方法
export default new ContainerModule((bind) => {
  bind(SERVICE_IDENTIFIER).to(MyService);
})
```

##### 3. scope
scope支持以下三种模式：   
- Transient(Default)：每次从容器中获取的时候(也就是每次请求)都是一个新的实例
- Singleton：每次从容器中获取的时候（也就是每次请求）都是同一个实例
- Request：社区里也称为Scoped模式，每次请求的时候都会获取新的实例，如果在这次请求中该类被require多次，那么依然还是用同一个实例返回

##### 4. bind
bind语法的参数既支持字符串，也支持我们传入类   
如果用字符串，那就会遇到命名空间的问题，所以这里可以使用Symbols来创建服务标识符（serviceIdentifier）来解决冲突问题
```ts
// 类
container.bind<Katana>(Katana).to(Katana).inSingletonScope();
// 字符串
container.bind('Katana').to(Katana).inSingletonScope();
// Symbols来创建服务标识符（serviceIdentifier）
const TYPES = {
    Katana: Symbol.for("Katana")
};
container.bind(TYPES.Katana).to(Katana).inSingletonScope();
```

##### 5. to和toself
to：绑定一个类（获取类的实例）   
toSelf：to 的简化版，当serviceIdentifier（标识符）是构造函数时，直接绑定自身。   
```ts
bind(WidgetManager).toSelf().inSingletonScope();
// 等于
bind(WidgetManager).to(WidgetManager).inSingletonScope();
```

##### 6. toConstantValue
绑定一个常量   
```ts
// 创建一个容器
const container = new Container();

// 创建一个服务标识符
const SERVICE_IDENTIFIER = Symbol.for("MyService");

// 创建一个常量值
const myConstant = "Hello, world!";

// 将服务标识符绑定到常量值
container.bind(SERVICE_IDENTIFIER).toConstantValue(myConstant);

// 从容器中获取服务
const myService = container.get(SERVICE_IDENTIFIER);

// 输出服务的值
console.log(myService); // Hello, world!
```

##### 7. toDynamicValue
绑定为动态值，在获取的时候会去执行对应的函数   
```ts
// example 1
bind(WidgetFactory).toDynamicValue(context => ({
    id: CALLHIERARCHY_ID,
    createWidget: () => createHierarchyTreeWidget(context.container)
}));

// example 2
// 创建一个服务标识符
const SERVICE_IDENTIFIER = Symbol.for("MyService");

// 创建一个动态值的函数
const myDynamicValue = (context: interfaces.Context) => {
  // 从上下文中获取其他服务或参数
  const someDependency = context.container.get("SomeDependency");
  const someConfig = context.container.get("SomeConfig");
  // 根据其他服务或参数创建一个值
  const myValue = new MyService(someDependency, someConfig);
  // 返回这个值
  return myValue;
};

// 将服务标识符绑定到动态值
container.bind(SERVICE_IDENTIFIER).toDynamicValue(myDynamicValue);

// 从容器中获取服务
const myService = container.get(SERVICE_IDENTIFIER);

// 输出服务的值
console.log(myService); // MyService { ... }
```

##### 8. toFactory
绑定为工厂函数，与刚才的动态值不一样，动态值会执行完动态函数返回值，而工厂函数则会返回一个高阶函数，允许进一步定制值，返回的是一个工厂函数
```ts
// example 1
bind(LoggerFactory).toFactory(ctx =>
    (name: string) => {
        const child = new Container({ defaultScope: 'Singleton' });
        child.parent = ctx.container;
        child.bind(ILogger).to(Logger).inTransientScope();
        child.bind(LoggerName).toConstantValue(name);
        return child.get(ILogger);
    }
);

// example 2
export class SaveImageDialogProps extends DialogProps {
    defaultFileName?: string;
}

export type SaveImageDialogFactory = (props: SaveImageDialogProps) => SaveImageDialog;
export const SaveImageDialogFactory = Symbol('SaveImageDialogFactory');

export function createSaveImageDialogFactory(ctx: interfaces.Context): SaveImageDialogFactory {
    return (props: SaveImageDialogProps): SaveImageDialog => {
        const container = ctx.container.createChild();

        container.bind(SaveImageDialogProps).toConstantValue(props);
        container.bind(SaveImageDialog).toSelf().inSingletonScope();
        const provider = container.get<SaveImageDialog>(SaveImageDialog);

        return provider;
    };
}

bind(SaveImageDialogFactory).toFactory(createSaveImageDialogFactory);

// another call
bind(BootgenDialogFactory).toFactory(
        ctx => (props: BootgenDialogProps) => createBootgenDialogContainer(ctx.container, props).get(BootgenDialog)
    );

// inject, it`s a function
@inject(SaveImageDialogFactory) readonly saveImageDialogFactory: SaveImageDialogFactory;
this.saveImageDialogFactory({title: 'Export to SVG'});
```

##### 9. toService
绑定为一个服务，让其解析为以前声明过的别的类型绑定，这个绑定很特殊，没有别的任何后续操作，因为它没有返回值   
```ts
bind(FrontendApplicationContribution).toService(IconThemeApplicationContribution);

public toService(service: string | symbol | interfaces.Newable<T> | interfaces.Abstract<T>): void {
    this.toDynamicValue(
        (context) => context.container.get<T>(service)
    );
}
```

##### 10. BindingWhenSyntax
这个对象主要用于设置绑定条件。在theia中有用到whenTargetNamed、whenTargetIsDefault等方法   
```ts
myContainer.bind<Warrior>(TYPES.Warrior).to(Ninja).whenTargetNamed('ninja');
myContainer.bind<Warrior>(TYPES.Warrior).to(Knight).whenTargetNamed('knight');

@injectable()
class Use{
    @inject(TYPES.Warrior) @named('ninja')
    pirvate _warrior: Warrior; // Ninja
}
```



----

[inversifyJs详解](https://juejin.cn/post/7049717544109752350)  
[Theia之inversify](https://juejin.cn/post/6971722030831894542)   
[inversifyJS](https://doc.inversify.cloud/zh_cn/)   
