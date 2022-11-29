### 概念  
- **依赖倒置原则( DIP )**
    - 抽象不应该依赖实现，实现也不应该依赖实现，实现应该依赖抽象。
-   **依赖注入（dependency injection，简写为 DI）**
	- 依赖是指依靠某种东西来获得支持。将创建对象的任务转移给其他class，并直接使用依赖项的过程，被称为“依赖项注入”。
-   **控制反转（Inversion of Control, 简写为 IoC）**
	- 指一个类不应静态配置其依赖项，应由其他一些类从外部进行配置。
-  元数据（metaData）
	- 描述数据的数据，类似于JavaScript中object的属性，描述object的状态和行为；
---

#### 理解
##### 依赖注入
```js
class B {}
this.b = new B()
class A {
	constructor(a){
		this.a = a;
	}
	setDep(a){
		this.a = a;
	}
}
new A(new B())
new A(this.b)
const a = new A();
a.setDep(new B())
a.setDep(this.b)
```

##### 控制反转
将类的实例化控制权从程序员手里交给程序框架管理；
利用[[#反射]]实现控制反转；

##### 总结
Nest 利用反射技术、实现了控制反转，提供了元编程能力，开发者使用 @Module 装饰器修饰类并定义==元数据(providers|imports|exports)==，元数据被存储在全局对象中（使用 ==reflect-metadata== 库）。程序运行后，Nest 框架内部的控制程序读取和注册模块树，扫描元数据并实例化类，使其成为提供者，并根据模块元数据中的 providers|imports|exports 定义，在所有模块的提供者中寻找当前类的其它依赖类的实例（提供者），找到后通过构造函数注入。














## 反射
- 反射是一种获取和使用元数据的技术；
- 反射是一种在运行时获取和使用元数据的技术
- 使用场景
	- 大部分都是动态设置类型字段/属性，或动态调用方法。


我们可以通过Reflection这个命名空间来获取这些元数据，对应**一个程序集的描述是Assembly类**，对应**一个Class的元数据就是Type**，当然还有**ConstructorInfo、PropertyInfo、FieldInfo、EventInfo、MethodInfo、ParameterInfo、Attribute**等。利用这个反射命名空间，我们可以获取一个Assembly中所有类型的描述，利用这些元数据可以动态的创建类的实例，获取/设置属性，获取事件并订阅事件，调用方法，获取一个类/属性/字段/方法/构造器/参数的Attribute。我们在编程时能做到的事情，利用反射也基本能够做到。


###### 元数据
描述数据的数据，主要是描述数据属性的信息，也可以理解为描述程序的数据。
在C#，其元数据存在Module中，用于描述这个Module所有类型的信息，这种信息包括类型描述、类型Attribute、成员、事件信息等，总之，关于一个Class，所有的描述信息都在这个Module的元数据中定义。
在JavaScript，元数据的概念类似object的属性，描述object的行为，成员；


反射用例1：就是数据库中的 **ORM（对象关系映射）**，使用 ORM 只需要定义表字段，ORM 库会自动把对象数据转换为 SQL 语句。
```js
const data = TableModel.build();
data.time = 1;
data.browser = 'chrome';
data.save();
// SQL: INSERT INTO tableName (time,browser) [{"time":1,"browser":"chrome"}]
```
---

## NEST使用
1. 模块（Modules）
2. 控制层（Controllers）
3. 服务层（Providers）
4. 中间件(Middleware)
5. 过滤器(Exception filters)
6. 管道(Pipes)
7. 守卫(Guards)
8. 拦截器(interceptors)


#### Modules 
dynamic module

