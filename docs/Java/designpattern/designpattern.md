# 设计模式(Java)
## 一、什么是设计模式
软件工程中，**设计模式**是对软件设计过程中普遍存在（反复出现）的各种问题，所提出的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。设计模式代表了最佳的实践，这个术语是由埃里希·伽玛（Erich Gamma）等人在1990年代从建筑设计领域引入到计算机科学的。
## 二、设计模式的目的和原则
### 1、目的
在编写软件过程中，程序员面临着来自**耦合性，内聚性以及可维护性，可扩展性，重用性，灵活性**等多方面的挑战，设计模式是为了让程序(软件)具有更好的：
* 代码重用性（相同功能的代码，不用多次编写）
* 可读性（编程规范性，便于其它程序员的阅读和理解）
* 可扩展性（当需要新增功能时，非常的方便）
* 可靠性（当我们新增新的功能后，对原来的功能没有影响）
* 使程序呈现高内聚，低耦合的特性
### 2、原则
设计模式的原则，其实就是程序员在编程时，应当遵守的规则，也就是各种设计模式的基础，下面简单介绍一下常用的七大原则：**单一职责原则、接口隔离原则、依赖倒转（倒置）原则、里氏替换原则、开闭原则、迪米特法则、合成复用原则**。
#### 1、单一职责原则
对类来说，**一个类应该只负责一项原则**，如果类A负责两个不同职责：职责1、职责2。当职责1需求变更而改变A时，可能造成职责2执行错误，所以需要将类A的粒度分解为A1、A2。下面以以交通工具为案例讲解。

（1）案例1
```java
package com.principle.singleresponsibility;

public class SingleResponsibility1 {
    public static void main(String[] args) {
        Vehicle vehicle=new Vehicle();
        vehicle.run("汽车");
        vehicle.run("摩托车");
        vehicle.run("飞机");
    }
}
// 交通工具类
class Vehicle{
    public void run(String vehicle){
        System.out.println(vehicle+"在公路上运行。。。");
    }
}
```
在这种方式的run方法中，违背了**单一职责原则**，下面是解决方法，更具交通工具运行的方法不同，分解成不同的类。

（2）案例2
```java
package com.principle.singleresponsibility;

public class SingleResponsibility2 {
    public static void main(String[] args) {
        RoadVehicle roadVehicle=new RoadVehicle();
        roadVehicle.run("汽车");
        roadVehicle.run("摩托车");

        AirVehicle airVehicle=new AirVehicle();
        airVehicle.run("飞机");
    }
}

// 单一职责原则，但是改动很大
class RoadVehicle{
    public void run(String vehicle){
        System.out.println(vehicle+"在公路运行");
    }
}

class AirVehicle{
    public void run(String vehicle){
        System.out.println(vehicle+"在天空运行");
    }
}

class WaterVehicle{
    public void run(String vehicle){
        System.out.println(vehicle+"在水上运行");
    }
}
```
这种方式遵循了单一职责原则，但是这样做的改动很大，即**将类分解**，同时修改了客户端，可以直接修改Vehicle类，改动的代码会比较少。
（3）案例3
```java
package com.principle.singleresponsibility;

public class SingleResponsibility3 {
    public static void main(String[] args) {
        Vehicle2 vehicle2=new Vehicle2();
        vehicle2.run("汽车");
        vehicle2.runWater("轮船");
        vehicle2.runAir("飞机");
    }
}
class Vehicle2{
    public void run(String vehicle){
        System.out.println(vehicle+"在公路上运行。。。");
    }

    public void runAir(String vehicle){
        System.out.println(vehicle+"在天空运行。。。");
    }

    public void runWater(String vehicle){
        System.out.println(vehicle+"在水中运行。。。");
    }
}   
```
这种修改方法没有对原来的类做很大的修改，只是增加了方法，这里虽然**没有在类级别上遵守单一职责原则**，但是**在方法级别上，仍然是遵守单一职责的**。

（4）注意的事项和细节
* 降低类的**复杂度**，一个类只负责一项职责。
* 提高类的**可读性，可维护性**。
* 降低变更引起的风险。
* 通常情况下，我们应当遵守单一职责原则，只要逻辑足够简单，才可以在代码级违反单一职责原则；只有类中方法足够少，可以在方法级别保持单一职责原则。
#### 2、接口隔离原则
客户端不应该依赖它不需要的接口，即**一个类对另一个类的依赖应该建立在最小的接口**上。以下图为例。
![接口隔离原则图片解释](./img/%E6%8E%A5%E5%8F%A3%E9%9A%94%E7%A6%BB%E5%8E%9F%E5%88%991.png)
图中案例，类A通过接口Interface1依赖类B，类C通过接口Interface1依赖类D，如果接口Interface1对于类A和类C来说不是最小接口，那么类B和类D必须要去实现它们不需要的方法。根据接口隔离原则，我们可以将Interface1**拆分为独立的几个接口**，类A和类C**分别与它们需要的接口建立依赖关系**。下面通过案例来对接口隔离原则进行解释。

（1）案例1

这个案例是最原始的案例，也就是上图中没有使用接口隔离原则的代码。
```java
package com.principle.segregation;

public class Segregation1 {
    
}

interface Interface1{
    void operation1();
    void operation2();
    void operation3();
    void operation4();
    void operation5();
}

class B implements Interface1{

    @Override
    public void operation1() {
        System.out.println("B实现了operation1");
    }

    @Override
    public void operation2() {
        System.out.println("B实现了operation2");
    }

    @Override
    public void operation3() {
        System.out.println("B实现了operation3");
    }

    @Override
    public void operation4() {
        System.out.println("B实现了operation4");
    }

    @Override
    public void operation5() {
        System.out.println("B实现了operation5");
    }
    
}

class D implements Interface1{

    @Override
    public void operation1() {
        System.out.println("D实现了operation1");
    }

    @Override
    public void operation2() {
        System.out.println("D实现了operation2");
    }

    @Override
    public void operation3() {
        System.out.println("D实现了operation3");
    }

    @Override
    public void operation4() {
        System.out.println("D实现了operation4");
    }

    @Override
    public void operation5() {
        System.out.println("D实现了operation5");
    }
}

class A{
    // A类通过接口Interface1 依赖B类，但是只会使用到123方法
    public void depend1(Interface1 i){
        i.operation1();
    }
    public void depend2(Interface1 i){
        i.operation2();
    }
    public void depend3(Interface1 i){
        i.operation3();
    }
}

class C{
    // C类通过接口Interface1 依赖D类，但是只会使用到145方法
    public void depend1(Interface1 i){
        i.operation1();
    }
    public void depend4(Interface1 i){
        i.operation4();
    }
    public void depend5(Interface1 i){
        i.operation5();
    }
}
```
在这个案例中，没有将Interface1拆分，从而破坏了接口隔离原则，可以将Interface1进行拆分为几个独立的接口，类A和类C分别与它们需要的接口建立依赖关系，也就是采用接口隔离原则，看下一个案例。

（2）案例2

将Interface1拆分为三个接口，如下图。
![接口隔离原则2](./img/%E6%8E%A5%E5%8F%A3%E9%9A%94%E7%A6%BB%E5%8E%9F%E5%88%992.webp)
类A根据需要依赖Interface1和2，类C根据需要依赖Interface2和3，案例代码如下：
```java
package com.principle.segregation.improve;

public class Segregation1 {
    public static void main(String[] args) {
        
        A a = new A();
        a.depend1(new B());//A类通过接口去依赖
        a.depend2(new B());
        a.depend3(new B());
        C c=new C();
        c.depend1(new D());
        c.depend4(new D());
        c.depend5(new D());
    }    
}

interface Interface1{
    void operation1();
}

interface Interface2{
    void operation2();
    void operation3();
}

interface Interface3{
    void operation4();
    void operation5();
}

class B implements Interface1,Interface2{

    @Override
    public void operation1() {
        System.out.println("B实现了operation1");
    }

    @Override
    public void operation2() {
        System.out.println("B实现了operation2");
    }

    @Override
    public void operation3() {
        System.out.println("B实现了operation3");
    }
    
}

class D implements Interface1,Interface3{

    @Override
    public void operation1() {
        System.out.println("D实现了operation1");
    }

    @Override
    public void operation4() {
        System.out.println("D实现了operation4");
    }

    @Override
    public void operation5() {
        System.out.println("D实现了operation5");
    }
    
}

class A{
    // A类通过接口Interface1,Interface2 依赖B类，但是只会使用到123方法
    public void depend1(Interface1 i){
        i.operation1();
    }
    public void depend2(Interface2 i){
        i.operation2();
    }
    public void depend3(Interface2 i){
        i.operation3();
    }
}

class C{
    // C类通过接口Interface1,interface3 依赖D类，但是只会使用到145方法
    public void depend1(Interface1 i){
        i.operation1();
    }
    public void depend4(Interface3 i){
        i.operation4();
    }
    public void depend5(Interface3 i){
        i.operation5();
    }
}
```
案例中类B和类D分别实现了类A和类C需要的接口，从而实现了接口隔离原则。
