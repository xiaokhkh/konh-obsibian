### 概念  
- **依赖倒置原则( DIP )**
    - 抽象不应该依赖实现，实现也不应该依赖实现，实现应该依赖抽象。
-   **依赖注入（dependency injection，简写为 DI）**
	- 依赖是指依靠某种东西来获得支持。将创建对象的任务转移给其他class，并直接使用依赖项的过程，被称为“依赖项注入”。
-   **控制反转（Inversion of Control, 简写为 IoC）**
	- 指一个类不应静态配置其依赖项，应由其他一些类从外部进行配置。
-  元数据（metaData）
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
将类的实例化控制权从程序员手里交给框架管理；
利用[[#反射机制]]实现控制反转；


1.  元数据反射是实现依赖注入的基础；
2.  总结依赖注入的过程，nest 主要做了三件事情 
	1.  知道哪些类需要哪些对象
	2.  创建对象
	3.  并提供所有这些对象














###### 反射机制

例：使用orm驱动数据库；