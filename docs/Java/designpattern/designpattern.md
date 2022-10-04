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
在这种方式的run方法中，违背了**单一职责原则**，下面是解决方法，根据交通工具运行的方法不同，分解成不同的类。

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
#### 3、依赖倒转原则
依赖倒转原则是指：高层模块不应该依赖底层模块，二者都应该依赖其抽象；**抽象不应该依赖细节，细节应该依赖抽象**；中心思想是**面向接口编程**；设计理念：相对于细节的多变性，抽象的东西要稳定的多，以抽象为基础搭建的架构比以细节为基础的架构要稳定的多（在Java中，抽象指的是接口或抽象类，细节就是具体的实现类）。使用接口或抽象类的目的是制定好规范，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成。

(1)案例1

在这个案例中，实现了Person接收消息的功能。
```java
package com.principle.inversion;

public class DependencyInversion {
    public static void main(String[] args) {
        Person person=new Person();
        person.receive(new Email());
    }
}
class Email{
    public String getInfo(){
        return "电子邮件信息:hello world!";
    }
}
// 完成Person接收消息的功能
// 方式1
class Person {
    public void receive(Email email){
        System.out.println(email.getInfo());
    }
}
```
但是在这个案例中，Person通过Email发送消息，如果我们想要通过微信、短信等发送消息，我们需要新增类，同时也要在Person中新增相应的接收方法。

（2）案例2

对上一个案例的改进，引入一个抽象接口IReceiver，表示接收者，这样Person类与接口IReceiver发生依赖，因为Email,WeiXin等等属于接收范围，它们各自实现IReceiver接口就行了，这样就是依赖倒转原则。
```java
package com.principle.inversion.improve;

public class DependencyInversion {
    public static void main(String[] args) {
        Person person=new Person();
        person.receive(new Email());
        person.receive(new WeiXin());
    }
}
// 定义一个接口
interface IReceiver{
    public String getInfo();
}
class Email implements IReceiver{
    public String getInfo(){
        return "电子邮件信息:hello world!";
    }
}
// 增加微信
class WeiXin implements IReceiver{
    @Override
    public String getInfo() {
        return "微信消息:hello wechat";
    }   
}
// 完成Person接收消息的功能
// 方式2
class Person {
    public void receive(IReceiver receiver){
        System.out.println(receiver.getInfo());
    }
}
```
以上案例通过接口来实现依赖倒转原则。

（3）三种实现依赖倒转的方式

实现依赖倒转除了可以通过上面的接口传递的方式，还可以通过构造方法传递和setter方式传递。
```java
package com.principle.inversion;

public class DependencyPass {
    public static void main(String[] args) {
        ChangHong changHong1 = new ChangHong();
        OpenAndClose openAndClose = new OpenAndClose();
        openAndClose.open(changHong1);
        // 通过构造器进行依赖传递
        OpenAndClose1 openAndClose1 = new OpenAndClose1(changHong1);
        openAndClose1.open();
        // 通过 setter 方法进行依赖传递
        ChangHong2 changHong2 = new ChangHong2();
        OpenAndClose2 openAndClose2 = new OpenAndClose2();
        openAndClose2.setTv(changHong2);
        openAndClose2.open();
    }
}

// 方式 1： 通过接口传递实现依赖
// 开关的接口
interface IOpenAndClose {
    public void open(ITV tv); // 抽象方法,接收接口
}

interface ITV { // ITV 接口
    public void play();
}

class ChangHong implements ITV {
    @Override
    public void play() {
        System.out.println("长虹电视机，打开");
    }
}

// 实现接口
class OpenAndClose implements IOpenAndClose {
    public void open(ITV tv) {
        tv.play();
    }
}

// 方式 2: 通过构造方法依赖传递
interface IOpenAndClose1 {
    public void open(); // 抽象方法
}

class OpenAndClose1 implements IOpenAndClose1 {
    public ITV tv; // 成员

    public OpenAndClose1(ITV tv) { // 构造器
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
}

// 方式 3 , 通过 setter 方法传递
interface IOpenAndClose2 {
    public void open(); // 抽象方法
    public void setTv(ITV2 tv);
}

interface ITV2 { // ITV 接口
    public void play();
}

class OpenAndClose2 implements IOpenAndClose2 {
    private ITV2 tv;

    public void setTv(ITV2 tv) {
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
}

class ChangHong2 implements ITV2 {
    @Override
    public void play() {
        System.out.println("长虹电视机，打开");
    }
}
```
依赖倒转原则中需要注意：底层模块尽量都要由抽象类或接口，或者两者都有，程序稳定性更好，变量的声明类型**尽量是抽象类或接口**，这样我们的变量引用和实际对象间，就存在一个缓冲层，利于程序扩展和优化，继承遵循里氏替换原则。
#### 4、里氏替换原则
**继承存在的问题**：继承包含这样一层含义：父类中凡是已经实现好的方法，实际上是在设定规范和契约，虽然它不强制要求所有的子类必须遵循这些契约，但是如果子类对这些已经实现的方法任意修改，就会对整个继承体系造成破坏。 继承在给程序设计带来便利的同时，也带来了弊端。比如使用继承会给程序带来侵入性，程序的可移植性降低，增加对象间的耦合性，如果一个类被其他的类所继承，则当这个类需要修改时，必须考虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障。

**里氏替换原则的介绍：** 如果对每个类型为 T1 的对象 o1，都有类型为 T2 的对象 o2，使得以 T1 定义的所有程序 P 在所有的对象 o1 都代换成 o2 时，程序 P 的行为没有发生变化，那么类型 T2 是类型 T1 的子类型。换句话说，**所有引用基类的地方必须能透明地使用其子类的对象。**在继承时，子类中尽量不要重写父类的方法，实际上，继承使两个类的耦合性增强了，在适当的情况下，可以通过聚合、组合、依赖来解决问题。

（1）案例1
```java
package com.principle.liskov;

public class Liskov {
    public static void main(String[] args) {
        A a=new A();
        System.out.println("11-3="+a.func1(11, 3));
        System.out.println("1-8="+a.func1(1, 8));
        System.out.println("--------------------------");
        B b=new B();
        System.out.println("11-3="+b.func1(11, 3));
        System.out.println("1-8="+b.func1(1, 8));
        System.out.println("11+3+9="+b.func2(11, 3));
    }
}
// A类，返回两个数字的差
class A {
    public int func1(int num1,int num2){
        return num1-num2;
    }
}
// B类，继承了A类
class B extends A{
    public int func1(int a, int b) {
        return a+b;
    }
    public int func2(int a,int b){
        return func1(a,b)+9;
    }
}
```
 
我们发现原来运行正常的相减功能发生了错误。原因就是类 B 无意中重写了父类的方法，造成原有功能出现错误。在实际编程中，我们常常会通过重写父类的方法完成新的功能，这样写起来虽然简单，但整个继承体系的复用性会比较差。特别是运行多态比较频繁的时候。 通用的做法是：**原来的父类和子类都继承一个更通俗的基类**，原有的继承关系去掉，采用依赖，聚合，组合等关系代替。

（2）案例二
```java
package com.principle.liskov.improve;

public class Liskov {
    public static void main(String[] args) {
        A a=new A();
        System.out.println("11-3="+a.func1(11, 3));
        System.out.println("1-8="+a.func1(1, 8));
        System.out.println("--------------------------");
        B b=new B();
        System.out.println("11+3="+b.func1(11, 3));
        System.out.println("1+8="+b.func1(1, 8));
        System.out.println("11+3+9="+b.func2(11, 3));
        System.out.println("11-3="+b.func3(11, 3));
    }
}
//创建一个更加基础的基类
class Base {
    // 把更加基础的方法和成员写到积累里面
}
// A类，返回两个数字的差
class A extends Base {
    public int func1(int num1,int num2){
        return num1-num2;
    }
}
// B类，继承了Base类
class B extends Base{
    // 如果B需要使用A类的方法，使用组合关系
    private A a=new A();
    public int func1(int a, int b) {
        return a+b;
    }
    public int func2(int a,int b){
        return func1(a,b)+9;
    }
    //我们仍然要使用A的方法
    public int func3(int a,int b){
        return this.a.func1(a, b);
    }
}
```
在这个案例中，我们创建了一个更加基础的基类，把更加基础的方法和成员写到Base类中，使A和B类同时继承基类Base，如果B要使用A类中的方法，使用组合关系。
#### 5、开闭原则
开闭原则（Open Closed Principle）是编程中**最基础、最重要**的设计原则。一个软件实体如类，模块和函数应该对扩展开放（对提供方），对修改关闭（对使用方）。用抽象构建框架，用实现扩展细节。当软件需要变化时，尽量通过**扩展软件实体的行为**来实现变化，而不是通过修改已有代码来实现变化。

（1）案例1
```java
package com.principle.ocp;

public class Ocp {
    public static void main(String[] args) {
        GraphicEditor graphicEditor=new GraphicEditor();
        graphicEditor.drawRectangle(new Rectangle());
        graphicEditor.drawCircle(new Circle()); 
    }
}

// 这是一个用于绘图的类
class GraphicEditor{
    public void drawShape(Shape s){
        if(s.m_type==1){
            drawRectangle(s);
        }else if(s.m_type==2){
            drawCircle(s);
        }
    }
    public void drawRectangle(Shape r){
        System.out.println("绘制矩形");
    }
    public void drawCircle(Shape r){
        System.out.println("绘制圆形");
    }
}
class Shape{
    int m_type;
}
class Rectangle extends Shape{
    Rectangle(){
        super.m_type=1;
    }
}
class Circle extends Shape{
    Circle(){
        super.m_type=2;
    }
}
```
在这个案例中，并没有遵守**开闭原则**，即当我们给类增加新功能的时候，尽量不修改代码，或者尽可能少修改代码，但是在这个案例中，我们需要新增一个图形种类的时候，我们需要修改较多的地方。

（2）案例2

把创建 Shape 类做成抽象类，并提供一个抽象的 draw 方法，让子类去实现即可，这样我们有新的图形种类时，只需要让新的图形类继承 Shape，并实现 draw 方法即可，使用方的代码就不需要修改，满足了开闭原则。
```java
package com.principle.ocp.improve;

public class Ocp {
    public static void main(String[] args) {
        GraphicEditor graphicEditor=new GraphicEditor();
        graphicEditor.drawShape(new Rectangle());
        graphicEditor.drawShape(new Circle()); 
    }
}

// 这是一个用于绘图的类
class GraphicEditor{
    public void drawShape(Shape s){
       s.draw();
    }
}
abstract class Shape{
    int m_type;
    public abstract void draw();//抽象方法
}
class Rectangle extends Shape{
    Rectangle(){
        super.m_type=1;
    }

    @Override
    public void draw() {
        System.out.println("绘制矩形");
    }

    
}
class Circle extends Shape{
    Circle(){
        super.m_type=2;
    }
    @Override
    public void draw() {
        System.out.println("绘制圆形");
    }
}
```
这个案例满足了开闭原则，在新增功能的时候，修改少量的代码就可以完成。
#### 6、迪米特法则
迪米特法则又叫**最少知道原则**，即一个类对自己依赖的类知道的越少越好。也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部，对外除了提供的public方法，不对外泄露任何信息。或者更简单的定义：只与直接朋友通信。 **直接的朋友**：每个对象都会与其他对象有耦合关系，只要两个对象之间有耦合关系，我们就说这两个对象之间是朋友关系。耦合的方式很多，依赖，关联，组合，聚合等。其中，我们称出现的**成员变量，方法参数，方法返回值**中的类为直接的朋友，而出现在局部变量中的类不是直接的朋友。也就是说，陌生的类最好不要以局部变量的形式出现在类的内部。

（1）案例1

这个案例中：有一个学校，下属有各个学院和总部，打印出学校总部员工 ID 和学院员工的 id。
```java
package com.principle.demeter;

import java.util.ArrayList;
import java.util.List;

public class Demeter1 {
    public static void main(String[] args) {
        // 创建一个SchoolManager对象
        SchoolManager schoolManager=new SchoolManager();
        schoolManager.printAllEmployee(new CollegeManager());
    }   
}
class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}
	public String getId() {
		return id;
	}
}
class CollegeEmployee {
	private String id;
	public void setId(String id) {
		this.id = id;
	}
	public String getId() {
		return id;
	}
}

class CollegeManager {
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) {
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}
}
// SchoolManager类的直接朋友有Employee和CollegeManager
// CollegeEmployee不是直接朋友，这样做违反了迪米特法则
class SchoolManager {
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();	
		for (int i = 0; i < 5; i++) {
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}
	void printAllEmployee(CollegeManager sub) {	
		List<CollegeEmployee> list1 = sub.getAllEmployee();
		System.out.println("------------分公司员工------------");
        // CollegeEmployee不是SchoolManager的直接朋友
        // 因为它是以局部变量的形式存在
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}
```
在这个案例中SchoolManager类的直接朋友有Employee和CollegeManager，CollegeEmployee不是直接朋友（因为它是以局部变量的形式存在的），这样做违反了迪米特法则。

（2)案例2

前面设计的问题在于 SchoolManager 中，CollegeEmployee 类并不是 SchoolManager 类的直接朋友。 按照迪米特法则，应该避免类中出现这样非直接朋友关系的耦合。
```java
package com.principle.demeter.improve;

import java.util.ArrayList;
import java.util.List;

public class Demeter1 {
    public static void main(String[] args) {
        // 创建一个SchoolManager对象
        SchoolManager schoolManager=new SchoolManager();
        schoolManager.printAllEmployee(new CollegeManager());
    }
}

class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


class CollegeEmployee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


class CollegeManager {
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) {
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	// 输出学院员工的信息
	public void printEmployee(){
		List<CollegeEmployee> list1 = this.getAllEmployee();
		System.out.println("------------学院员工------------");
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
	}

}

class SchoolManager {
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();
		
		for (int i = 0; i < 5; i++) {
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	void printAllEmployee(CollegeManager sub) {
		sub.printEmployee();		
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}
```
**迪米特法则的核心是降低类之间的耦合**, 
但是注意：由于每个类都减少了不必要的依赖，因此迪米特法则只是要求降低类间(对象间)耦合关系，并不是要求完全没有依赖关系。

#### 7、合成复用原则
合成复用原则是**尽量使用合成/聚合的方式，而不是继承**。核心思想是：找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起。针对接口编程，而不是针对实现编程，为了交互对象之间的松耦合设计而努力。

## 三、设计模式概述
设计模式是程序员在面对同类软件工程设计问题所总结出来的有用的经验，模式不是代码，而是某类问题的通用解决方案，设计模式（Design pattern）代表了最佳的实践。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。设计模式的本质**提高软件的维护性、通用性和扩展性，并降低软件的复杂度**。设计模式并不局限于某种语言，java，php，c++ 都有设计模式。设计模式分为三种类型，共23种。

* 创建型模式：单例模式、抽象工厂模式、原型模式、建造者模式、工厂模式。
* 结构型模式：适配器模式、桥接模式、装饰模式、组合模式、外观模式、享元模式、代理模式。
* 行为型模式： 模版方法模式、命令模式、访问者模式、迭代器模式、观察者模式、中介者模式、备忘录模式、解释器模式（Interpreter 模式）、状态模式、策略模式、职责链模式(责任链模式)

> 不同的书籍上对分类和名称略有差异

## 四、单例设计模式
### 1、简介
所谓类的单例设计模式，就是**采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例**，并且该类只提供一个取得其对象实例的方法(静态方法)。
>比如 Hibernate 的 SessionFactory，它充当数据存储源的代理，并负责创建 Session 对象。SessionFactory 并不是轻量级的，一般情况下，一个项目通常只需要一个 SessionFactory 就够，这是就会使用到单例模式。

### 2、单例设计模式的八种方式
#### （1）饿汉式（静态常量）
创建一个饿汉式（静态常量）应用实例的步骤可以分为一下几步：

* 构造器私有化（防止外部new）
* 类的内部创建对象
* 向外暴露一个静态的公共方法。getInstance

实例代码如下：
```java
package com.singleton.type1;

public class Singleton1 {
    public static void main(String[] args) {
        Singleton instance1=Singleton.getInstance();
        Singleton instance2=Singleton.getInstance();
        System.out.println(instance1==instance2);      
        System.out.println("instance2.hashCode1="+instance1.hashCode());
        System.out.println("instance2.hashCode1="+instance2.hashCode());
    }
}
// 饿汉式（使用静态变量来写）
class Singleton{
    // 1、先将构造器私有化，外部不能new
    private Singleton(){

    }
    // 2、本类内部创建对象实例
    private final static Singleton instance = new Singleton();
    // 3、对外提供一个公有的方法返回实例对象
    public static Singleton getInstance(){
        return instance;
    }
}
```
最后程序代码输出的结果是：两个对象相等，并且返回的hash值也是一样的。
>优缺点说明：

> * 优点：这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同步问题。
> * 在类装载的时候就完成实例化，没有达到 Lazy Loading的效果。如果从始至终从未使用过这个实例，则会造成内存的浪费。
> * 结论：这种单例模式可用，可能造成内存浪费
 
这种方式基于 classloder 机制避免了多线程的同步问题，不过，instance 在类装载时就实例化，在单例模式中大多数都是调用 getInstance 方法，但是导致类装载的原因有很多种，因此不能确定有其他的方式（或者其他的静态方法）导致类装载，这时候初始化 instance 就没有达到 lazy loading 的效果。
#### （2）饿汉式（静态代码块）
这种方法是根据上一种方法进行改进的，将上一种的静态变量改为静态代码块。
```java

// 饿汉式（使用静态变量来写）
class Singleton{
    // 1、先将构造器私有化，外部不能new
    private Singleton(){

    }
    // 2、本类内部创建对象实例
    private final static Singleton instance;

    static { //在静态代码块中创建单例对象
        instance = new Singleton();
    }
    // 3、对外提供一个公有的方法返回实例对象
    public static Singleton getInstance(){
        return instance;
    }
}
```
最终的输出结果和上一个相同
> 优缺点说明

> * 这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块中，也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。优缺点和上面是一样的。
> * 结论：这种单例模式可用，但是可能造成内存浪费


#### （3）懒汉式（线程不安全）
上述两种方法都是线程安全的方法，但是都会造成线程的浪费。下面是一种线程不安全的方法。
```java
package com.singleton.type3;

public class Singleton3 {
    public static void main(String[] args) {
        Singleton instance1=Singleton.getInstance();
        Singleton instance2=Singleton.getInstance();
        System.out.println(instance1==instance2);      
        System.out.println("instance2.hashCode1="+instance1.hashCode());
        System.out.println("instance2.hashCode1="+instance2.hashCode());
    }
}

class Singleton{
    private static Singleton instance;
    private Singleton(){}
    // 提供一个静态的公有方法，当使用该方法时，才去创建instance
    // 懒汉式
    // 用到时候才创建
    public static Singleton getInstance(){
        if(instance==null){
            instance=new Singleton();
        }
        return instance;
    }
}
```
> 优缺点说明

> * 起到了Lazy Loading的效果，但是只能在单线程下使用。 
> * 如果在多线程下，一个线程进入了 if (singleton == null)判断语句块，还未来得及往下执行，另一个线程也通过了这个判断语句，这时便会产生多个实例。所以在多线程环境下不可使用这种方式。
> * 结论：在实际开发中，不要使用这种方式。

#### （4）懒汉式（线程安全，同步方法）
针对上一种方法的缺陷，使用**synchronized**关键字可以实现线程安全。
```java
package com.singleton.type4;

public class Singleton4 {
    public static void main(String[] args) {
        Singleton instance1=Singleton.getInstance();
        Singleton instance2=Singleton.getInstance();
        System.out.println(instance1==instance2);      
        System.out.println("instance2.hashCode1="+instance1.hashCode());
        System.out.println("instance2.hashCode1="+instance2.hashCode());
    }
}
//懒汉式第二种（线程安全，同步方法）
class Singleton{
    private static Singleton instance;
    private Singleton(){}
    // 提供一个静态的公有方法，加入了一个同步处理的代码，当使用该方法时，才去创建instance
    // 懒汉式
    // 用到时候才创建
    public static synchronized Singleton getInstance(){
        if(instance==null){
            instance=new Singleton();
        }
        return instance;
    }
}
```
> 优缺点说明：

> * 解决了线程安全问题
> * 效率太低了，每个线程在想获得类的实例时候，执行getInstance()方法都要进行同步。而其实这个方法只执行一次实例化代码就够了，后面的想获得该类实例，直接 return 就行了。方法进行同步效率太低。
> * 结论：在实际开发中，不推荐使用这种方式
#### （5）懒汉式（线程安全，同步代码块）
这种方式只是对上一种方式的改进，同样不推荐使用。只是将synchronized关键字换了一个位置而已。
```java
 public static Singleton getInstance(){
        if(instance==null){
            synchronized (Singleton.class){
                instance=new Singleton();
            }
        }
        return instance;
    }
```
#### （6）双重检查
根据以上方式存在的优缺点，提出了双重检查的方式，提供一个静态的公有方法，加入双重检查代码，解决线程安全问题, 同时解决懒加载问题
同时保证了效率, 推荐使用。
```java

//懒汉式第二种（线程安全，同步方法）
class Singleton{
    private static volatile Singleton instance;
    private Singleton(){}
    // 提供一个静态的公有方法，加入双重检查的代码，解决线程安全，同时解决懒加载的问题
    // 同时保证了效率
    public static Singleton getInstance(){
        if(instance==null){
            synchronized(Singleton.class){
                if(instance==null){
                    instance=new Singleton();
                }
            }
        }
        return instance;
    }
}
```
> 优缺点说明

> *  Double-Check 概念是多线程开发中常使用到的，如代码中所示，我们进行了两次 if (singleton == null)检查，这样就可以保证线程安全了。
> * 这样，实例化代码只用执行一次，后面再次访问时，判断 if (singleton == null)，直接 return 实例化对象，也避免的反复进行方法同步。
> * 线程安全，延迟加载，效率较高
> * 在实际开发中，推荐使用这种单例设计模式。
#### （7）静态内部类
上述方法都是作用于方法上的，可以直接创建一个内部类来使用。
```java
//静态内部类完成，推荐使用
class Singleton{
    private Singleton(){}

    // 写一个静态内部类，该类有一个静态属性Singleton
    private static class SingletonInstance{
        private static final Singleton INSTANCE =new Singleton();
    }

    // 提供一个静态的公有方法，加入双重检查的代码，解决线程安全，同时解决懒加载的问题
    // 同时保证了效率
    public static Singleton getInstance(){
        return SingletonInstance.INSTANCE;
    }
}
```
> 优缺点说明

> * 这种方式采用了类装载的机制来保证初始化实例时只有一个线程。
> * 静态内部类方式在 Singleton 类被装载时并不会立即实例化，而是在需要实例化时，调用 getInstance 方法，才会装载 SingletonInstance 类，从而完成 Singleton 的实例化。
> * 类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM 帮助我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的。
> * 优点：避免了线程不安全，利用静态内部类特点实现延迟加载，效率高。
> * 开发中推荐使用
#### （8）枚举
通过枚举也可以实现单例
```java
package com.singleton.type8;

public class Singleton8 {
    public static void main(String[] args) {
        Singleton instance1=Singleton.INSTANCE;
        Singleton instance2=Singleton.INSTANCE;
        System.out.println(instance1==instance2);
        System.out.println("instance2.hashCode1="+instance1.hashCode());
        System.out.println("instance2.hashCode1="+instance2.hashCode());
        instance1.sayOK();
    }    
}

// 使用枚举
enum Singleton {
    INSTANCE; //属性
    public void sayOK(){
        System.out.println("ok~");
    }
}
```
> 优缺点说明：

> * 这借助 JDK1.5 中添加的枚举来实现单例模式。不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。
> * 这种方式是 Effective Java 作者 Josh Bloch 提倡的方式
> 结论：推荐使用。

### 3、单例模式注意事项和细节说明
 单例模式保证了 系统内存中该类只存在一个对象，节省了系统资源，对于一些需要频繁创建销毁的对象，使用单例模式可以提高系统性能。当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是使用 new。 
单例模式使用的场景：需要**频繁的进行创建和销毁的对象、创建对象时耗时过多或耗费资源过多**(即：重量级对象)，但又经常用到的对象、工具类对象、频繁访问数据库或文件的对象(比如数据源、session工厂等)。
