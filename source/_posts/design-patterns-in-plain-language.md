---
title: 大话设计模式
date: 2023-07-12 16:29:00
tags: [ 'PHP' ]
categories: [ 'PHP' ]
excerpt: "用PHP进行演示"
---

# 1.【简单工厂模式】代码无错就是优

```php
<?php 
/**
 * Operation 
 */
class Operation
{
    protected  $a = 0;
    protected  $b = 0;
    public function setA($a)
    {
        $this->a = $a;
    }
    public function setB($b)
    {
        $this->b = $b;
    }
    public function getResult()
    {
        $result = 0;
        return $result;
    }
}
/**
 * Add 
 */
class OperationAdd extends Operation
{
    public function getResult()
    {
        return $this->a + $this->b;
    }
}
/**
 * Mul
 */
class OperationMul extends Operation
{
    public function getResult()
    {
        return $this->a * $this->b;
    } 
}
/**
 * Sub
 */
class OperationSub extends Operation
{
    public function getResult()
    {
        return $this->a - $this->b;
    }
}
/**
 * Div
 */
class OperationDiv extends Operation
{
    public function getResult()
    {
        return $this->a / $this->b;
    }
}
/**
 * Operation Factory
 */
class OperationFactory
{
    public static function createOperation($operation)
    {
        switch ($operation) {
            case '+':
                $oper = new OperationAdd();
                break;
            case '-':
                $oper = new OperationSub();
                break;
            case '/':
                $oper = new OperationDiv();
                break;
            case '*':
                $oper = new OperationMul();
                break;
        }
        return $oper;
    }
}
// 客户端代码
$operation = OperationFactory::createOperation('+');
$operation->setA(1);
$operation->setB(2);
echo $operation->getResult() . PHP_EOL;
```

## 总结
> 学会通过分封装，继承，多态把程序的藕合度降低
> 
> 复用，不是复制！
> 
> 高内聚，低耦合

# 2.【策略模式】商场促销

```php
<?php 
/**
* abstract class
*/
abstract class Strategy
{
    // 算法方法
    abstract public function AlgorithmInterface();    
}
/**
* 算法a
*/
class ConcreteStrategyA extends Strategy
{
    public function AlgorithmInterface()
    {
        echo "算法a实现\n";
    }
}
/**
* 算法b
*/
class ConcreteStrategyB extends Strategy
{
    public function AlgorithmInterface()
    {
        echo "算法b实现\n";
    }
}
/**
* 算法c
*/
class ConcreteStrategyC extends Strategy
{
    public function AlgorithmInterface()
    {
        echo "算法c实现\n";
    }
}
/**
* 上下文context
*/
class Context
{
    private $strategy;
    function __construct($strategy)
    {
        $this->strategy = $strategy;
    }
    public function contextInterface()
    {
        $this->strategy->AlgorithmInterface();
    }
}
$context = new Context(new ConcreteStrategyA());
$context->contextInterface();
$context = new Context(new ConcreteStrategyB());
$context->contextInterface();
$context = new Context(new ConcreteStrategyC());
$context->contextInterface();
```

策略模式和简单工厂模式相结合

```php
<?php
// 只需要修改上方的Context类
class Context
{
    private $strategy;
    function __construct($operation)
    {
        switch ($operation) {
            case 'a':
                $this->strategy = new ConcreteStrategyA();
                break;
            case 'b':
               $this->strategy = new ConcreteStrategyB();
                break;
            case 'c':
                $this->strategy = new ConcreteStrategyC();
                break;
        }
    }
    public function contextInterface()
    {
        return $this->strategy->AlgorithmInterface();
    }
}
//客户端代码
$context = new Context('a');
$context->contextInterface();
```

## 总结
> 面向对象的编程，并不是类越多越好，类的划分为了封装，但分类的基础是抽象，具有相同属性和功能的对象集合才是类。
> 
> 策略模式是一种定义一系列算法的方法，从概念上来看，所有这些算法完成的都是相同的工作，只是实现不同，它可以以相同的方式调用所有的算法，减少了各种算法类与使用算法类之间的耦合。
> 
> 策略模式的Strategy类层次为Context定义了一系列的可供重用的算法或行为。继承有助于析取出这些算法中公共功能。
> 
> 策略模式简化了单元测试，因为每个算法都有自己的类，可以通自己接口单独测试。
> 
> 当不同的行为堆砌在一个类中时，就很难避免使用条件语句来选择合适的行为。将这些行为封装在一个个Strategy类中，可以在使用这些行为的类中消除条件语句。
> 
> 策略模式就是用来封装算法的，但在实践中，我们发现可以用它来封装几乎任何类型的规则，只要在分析过程中听到需要在不同时间应用不同的业务规则，就可以考虑使用策略模式处理这种变化的可能性。
> 
> 在基本的策略模式中，选择所用具体实现的职责由客户端对象承担，并转给策略模式的Context对象。

# 3.【单一职责原则】拍摄UFO

## 总结
> 单一职责原则， 就一个类而言,应该仅有一个引起 它变化的原因。
> 
> 如果一个类承担的职责过多，就等于把职责耦合在一起，一个职责的变化可能会削弱或者抑制这个类完成其他职责的能力。这种耦合会导致脆弱的设计，当变化发生时，设计会遭受到意想不到的破环。
> 
> 软件设计真正要做的许多内容，就是发现职责并把这些职责相互分离。
> 
> 如果你能够想到多于一个的动机改变一个类，那么这个类就具有多于一个类的职责。


# 4.【装饰模式】穿什么有这么重要吗
```php
<?php
abstract class Component
{
    abstract public function Operation();
}
/**
* ConcreteComponent
*/
class ConcreteComponent extends Component
{
    public function Operation()
    {
        echo "具体对象的操作.\n";
    }
}
abstract class Decorator extends Component
{
    protected $component;
    // 设置component
    public function SetComponent($component)
    {
        $this->component = $component;
    }
    // 重写Operation(),实际执行的是component的Operation方法
    public function Operation()
    {
        if ($this->component != null)
        {
            $this->component->Operation();
        }
    }
}
/**
* ConcreteDecoratorA
*/
class ConcreteDecoratorA extends Decorator
{
    // 本类的独有功能，以区别于ConcreteDecoratorB
    private $addedState;
    public function Operation()
    {   
        // 首先运行原Component的Operation(),再执行本类的功能，
        // 如addedState,相当于对原Component进行了装饰
        parent::Operation();
        $this->addedState = "ConcreteDecoratorA Status";
        echo $this->addedState."\n";
        echo "具体装饰对象A的操作.\n";
    }
}
/**
* ConcreteDecoratorB
*/
class ConcreteDecoratorB extends Decorator
{
    public function Operation()
    {   
        // 首先运行原Component的Operation(),再执行本类的功能，
        // 如addedBehavior,相当于对原Component进行了装饰
        parent::Operation();
        $this->addedBehavior();
        echo "具体装饰对象B的操作.\n";
    }
    // 本类的独有功能，以区别于ConcreteDecoratorA
    private function addedBehavior()
    {
        echo "ConcreteDecoratorB Status.\n";
    }
}
// 客户端代码
// 装饰的方法是：首先用ConcreteComponent实例化对象c,
// 然后用ConcreteDecoratorA的实例对象$di来包装$c,
// 然后再用ConcreteDecoratorB的实例$d2包装$d1,
// 最终执行$d2的Operation(); 
$c = new ConcreteComponent();
$d1 = new ConcreteDecoratorA();
$d2 = new ConcreteDecoratorB();
$d1->SetComponent($c);
$d2->SetComponent($d1);
$d2->Operation();
```

> 当只有一个ConcreteComponent类而没有抽象的Component类 ，那么Decorator类可以是ConcreteComponent的一个子类。同样的道理，如果只有一个ConcreteDecorator类 ，那么就没有必要建立一个单独的Decorator类，而可以把Decorator类和ConcreteComponent类的责任合并成一个类。

书中用打扮的代码阐释了这样情况下的写法：

```php
<?php 
// 人
class Person
{   
    private $name;
    function __construct($name)
    {
        $this->name = $name;
    }
    public function show()
    {
        echo "打扮".$this->name."\n";
    }
}
// 服饰类
class Finery
{
    protected $person;
    public function decorate($person)
    {
        $this->person = $person;
    }
    public function show()
    {
        if ($this->person != null)
        {
            $this->person->show();
        }
    }
}
// 具体服饰类
class TShirts extends Finery
{
    public function show()
    {
        echo "大T恤\n";
        parent::show();
    }
}
class BigTrouser extends Finery
{
    public function show()
    {
        echo "跨裤\n";
        parent::show();
    }
}
class Sneakers extends Finery
{
    public function show()
    {
        echo "破球鞋\n";
        parent::show();
    }
}
class Suit extends Finery
{
    public function show()
    {
        echo "西装\n";
        parent::show();
    }
}
class Tie extends Finery
{
    public function show()
    {
        echo "领带\n";
        parent::show();
    }
}
class LeatherShoes extends Finery
{
    public function show()
    {
        echo "跨裤\n";
        parent::show();
    }
}
// 客户端代码
$person = new Person("alex");
$sneakers = new Sneakers();
$bigTrouser = new BigTrouser();
$tShirts = new TShirts();
$sneakers->decorate($person);
$bigTrouser->decorate($sneakers);
$tShirts->decorate($bigTrouser);
$tShirts->show();
```

## 总结

> 装饰模式，动态地给一个对象添加一些额外的职责，就增加功能来说，装饰模式比生成子类更为灵活。
> 
> 装饰模式是为已有功能动态的添加更多功能的一种方式。
> 
> 当系统需要新功能的时候，是向旧的类中添加新的代码。这些新的代码通常装饰了原有类的核心职责或主要行为。
> 
> 在主类中加入新的字段，新的方法和新的逻辑，从而增加了主类的复杂度，而这些新加入的东西仅仅是为满足一些只在某种特定情况下才会执行的特殊行为的需要。装饰模式却提供了一个非常好的解决方案，它把每个要装饰的功能放在单独的类中，并让这个类包装它要装饰的对象，对此，当需要执行特殊行为时，客户代码就可以在运行时根据需要选择地，按顺序地使用装饰功能包装对象了。
> 
> 装饰模式的优点就是把类中的装饰功能从类中搬移去除，这样可以简化原有的类。
> 
> 有效地把类的核心职责和装饰功能区分开了，而且可以去除相关类中的重复的装饰逻辑。


# 5.【代理模式】为别人做嫁衣

```php
<?php 
class SchoolGirl
{
    private $name;
    function __construct($name)
    {
        $this->name = $name;
    }
    public function getName()
    {
        return $this->name;
    }
}
// 代理接口
interface GiveGift
{
    public function GiveDolls();
    public function GiveFlowers();
    public function GiveChocolate();
}
// 代理实现送礼物接口
class Proxy implements GiveGift
{
    protected $pursuit;
    function __construct(SchoolGirl $girl)
    {
        $this->pursuit = new Pursuit($girl);
    }
    public function GiveDolls()
    {
        $this->pursuit->GiveDolls();
    }
    public function GiveFlowers()
    {
        $this->pursuit->GiveFlowers();
    }
    public function GiveChocolate()
    {
        $this->pursuit->GiveChocolate();
    }
}
// 追求者类实现送礼物接口
class Pursuit implements GiveGift
{
    protected $girl;
    function __construct(SchoolGirl $girl)
    {
        $this->girl = $girl;
    }
    public function GiveDolls()
    {
        echo $this->girl->getName()." 送你娃娃\n";
    }
    public function GiveFlowers()
    {
        echo $this->girl->getName()." 送你花\n";
    }
    public function GiveChocolate()
    {
        echo $this->girl->getName()." 送你巧克力\n";
    }
}
// 客户端代码
$girl = new SchoolGirl('李梅');
$proxy = new Proxy($girl);
$proxy->GiveDolls();
$proxy->GiveChocolate();
$proxy->GiveFlowers();
```

## 总结

> 代理模式，为其他对象提供一种代理以控制对这个对象的访问


# 6.【工厂方法模式】雷锋依然在人间
根据第一章，简单工厂模式（加减乘除）是这样的：
```php
class OperationFactory
{
    public static function createOperation($operation)
    {
        switch ($operation) {
            case '+':
                $oper = new OperationAdd();
                break;
            case '-':
                $oper = new OperationSub();
                break;
            case '/':
                $oper = new OperationDiv();
                break;
            case '*':
                $oper = new OperationMul();
                break;
        }
        return $oper;
    }
}
// 客户端代码
$operation = OperationFactory::createOperation('+');
$operation->setA(1);
$operation->setA(2);
echo $operation->getResult()."\n";
```
换成工厂方法模式
```php
interface IFactory
{
    public function CreateOperation();
}
class AddFactory implements IFactory
{
    public function CreateOperation()
    {
        return new OperationAdd();
    }
}
class SubFactory implements IFactory
{
    public function CreateOperation()
    {
        return new OperationSub();
    }
}
class MulFactory implements IFactory
{
    public function CreateOperation()
    {
        return new OperationMul();
    }
}
class DivFactory implements IFactory
{
    public function CreateOperation()
    {
        return new OperationDiv();
    }
}
//客户端代码
$operationFactory = new AddFactory();
$operation = $operationFactory->CreateOperation();
$operation->setA(10);
$operation->setB(10);
echo $operation->getResult()."\n";
```

## 总结
> 工厂方法模式，定义一个创建对象的接口，让子类确定实例化哪一个类。工厂方法使一个类的实例化延迟到7其子类。
> 
> 简单工厂模式的最大优点在于工厂类中包含了必要的逻辑判断，根据用户端的选择条件动态实例化相关的类，对于客户端来说，去除了与具体产品的依赖。
> 
> 工厂方法模式实现时，客户端需要决定实例化哪一个工厂来实现运算类，选择判断的问题还是存在的，也就是说，工厂方法把简单工厂的内部逻辑判断移到了客户端代码来进行。你 想要加功能，本来是改工厂类的，而现在是修改客户端。

# 7.【原型模式】简历复制

```php
<?php 
class Company
{
    private $company;
    public function setName($name)
    {
        $this->company = $name;
    }
    public function getName()
    {
        return $this->company;
    }
}
class Resume
{
    private $name;
    private $sex;
    private $age;
    private $timeArea;
    private $company;
    function __construct($name)
    {
        $this->name = $name;
        $this->company = new Company();
    }
    public function setPersonalInfo($sex, $age)
    {
        $this->sex = $sex;
        $this->age = $age;
    }
    public function setWorkExperience($timeArea, $company)
    {
        $this->timeArea = $timeArea;
        $this->company->setName($company);
    }
    public function display()
    {
        echo $this->name." ".$this->sex." ".$this->age."\n";
        echo $this->timeArea." ".$this->company->getName()."\n";
    }
    // 对引用执行深复制
    function __clone()
    {
        $this->company = clone $this->company;
    }
}
// 客户端代码
$resume = new Resume("大鸟");
$resume->setPersonalInfo("男", 29);
$resume->setWorkExperience("1998-2000","xxx 公司");
$resume2 = clone $resume;
$resume2->setPersonalInfo("男", 40);
$resume2->setWorkExperience("1998-2010","xx 公司");
$resume->display();
$resume2->display();
```

## 总结
> 原型模式，用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。
> 
> 原型模式其实就是从一个对象再创建另外一个可定制的对象，而且不需要知道任何创建的细节。
> 
> 一般在初始化的信息不发生变化的情况下，克隆是最好的办法。既隐藏了对象创建的细节，又对性能是大大的提高。

# 8.【模版方法模式】考题抄错也白搭

```php
<?php 
// 对甲乙两名同学所抄试卷，尽量将相同的部分提到父类
// 金庸小说考题试卷
class TestPaper
{
    public function TestQuestion1()
    {
        echo "杨过说过，后来给了郭靖，炼成倚天剑、屠龙刀的玄铁可能是［］a.球磨铸铁 b.马口铁 c.高速合金钢 d.碳素纤维 \n";
        echo "答案 ".$this->answer1()."\n";
    }
    public function TestQuestion2()
    {
        echo "杨过、程英、陆无双铲除了情花，造成［］a.使这种植物不在害人 b.使一种珍惜物种灭绝 c.破坏了那个生态圈的生态平衡 d.造成该地区沙漠化 \n";
        echo "答案 ".$this->answer2()."\n";
    }
    public function TestQuestion3()
    {
        echo "蓝凤凰致使华山师徒、桃谷六仙呕吐不止，如果你是大夫，会给他们开什么药［］a.阿司匹林 b.牛黄解毒片 c.氟哌酸 d.让他们喝大量的生牛奶 e.以上全不对 \n";
        echo "答案 ".$this->answer3()."\n";
    }
    protected function answer1()
    {
        return '';
    }
    protected function answer2()
    {
        return '';
    }
    protected function answer3()
    {
        return '';
    }
}
// 学生甲抄的试卷
class TestPaperA extends TestPaper
{
    protected function answer1()
    {
        return 'a';
    }
    protected function answer2()
    {
        return 'b';
    }
    protected function answer3()
    {
        return 'c';
    } 
}
// 学生乙抄的试卷
// 学生甲抄的试卷
class TestPaperB extends TestPaper
{
    protected function answer1()
    {
        return 'd';
    }
    protected function answer2()
    {
        return 'c';
    }
    protected function answer3()
    {
        return 'a';
    } 
}
// 客户端代码
echo "学生甲抄的试卷: \n";
$student = new TestPaperA();
$student->TestQuestion1();
$student->TestQuestion2();
$student->TestQuestion3();
echo "学生乙抄的试卷: \n";
$student2 = new TestPaperB();
$student2->TestQuestion1();
$student2->TestQuestion2();
$student2->TestQuestion3();
```

## 总结
> 模版方法模式，定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模版方法使得子类可以不改变算法的节结构即可重定义该算法的某些特定步骤。
> 
> 既然用了继承，并且肯定这个继承有意义，就应该要成为子类的模版，所有重复的代码都应该要上升到父类去，而不是让每个子类去重复。
> 
> 当我们要完成在某一细节层次一致的一个过程或一系列步骤，但其中个别步骤在更详细的层次上的实现可能不同时，我们通常考虑用模版方法模式来处理。
> 
> 模版方法模式是通过把不变行为搬移到超类，去除子类中的重复代码来体现它的优势。提供了一个很好的代码复用平台。
> 
> 当不变的和可变的行为在方法的子类实现中混合在一起的时候，不变的行为就会在子类中重复出现。通过模版方法把这些行为搬移到单一的地方，这样就帮助子类摆脱重复的不变行为的纠缠。

# 9.【迪米特法则】无熟人难办事

## 总结
> 迪米特法则，如果两个类不彼此直接通信，那么这两个类就不应当发生直接的相互作用。如果其中一个类需要调用另一个类的某一个方法的话，可以通过第三者转发这个调用。
> 
> 在类的结构设计上，每一个类都应当 尽量降低成员的访问权限
> 
> 类之间的耦合越弱，越有利于复用，一个处在弱耦合的类被修改，不会对有关系的类造成波及。


# 10.【外观模式】牛市股票还会亏钱
```php
<?php 
//子系统1
class SubSystemOne
{
    public function methodOne()
    {
        echo "子系统方法1\n";
    }
}
//子系统2
class SubSystemTwo
{
    public function methodTwo()
    {
        echo "子系统方法2\n";
    }
}
//子系统3
class SubSystemThree
{
    public function methodThree()
    {
        echo "子系统方法3\n";
    }
}
//子系统4
class SubSystemFourth
{
    public function methodFourth()
    {
        echo "子系统方法4\n";
    }
}
// 外观方法
class Facade
{
    private $systemOne;
    private $systemTwo;
    private $systemThree;
    private $systemFour;
    function __construct()
    {
        $this->systemOne = new SubSystemOne();
        $this->systemTwo = new SubSystemTwo();
        $this->systemThree = new SubSystemThree();
        $this->systemFour = new SubSystemFourth();
    }
    public function methodA()
    {
        echo "方法A() ---\n";
        $this->systemOne->methodOne();
        $this->systemThree->methodThree();
    }
        public function methodB()
    {
        echo "方法B() ---\n";
        $this->systemTwo->methodTwo();
        $this->systemFour->methodFourth();
    }
}
//客户端代码
$facade = new Facade();
$facade->methodA();
$facade->methodB();
```

## 总结
> 外观模式，为子系统中的一组接口提供一个一致的界面，此模式定义了一个高层接口，这个接口使得这一子系统更容易使用。
> 
> 首先，在设计初期阶段，应该要有意识的将不同的两个层分离，层与层之间建立外观Facade；其次，在开发阶段，子系统往往因为不断的重构演化而变得越来越复杂，增加外观Facade可以提供一个简单的接口，减少它们之间的依赖；另外在维护一个遗留的大型系统时，可能这个系统已经非常难以维护和扩展了，为新系统开发一个外观Facade类，来提供设计粗糙或高度复杂的遗留代码的比较清晰简单的接口，让系统与Facade对象交互，Facade与遗留代码交互所有复杂的工作

# 11.【建造者模式】好菜没回味不同

```php
<?php 
//画小人
abstract class PersonBuilder
{
    abstract public function BuildHead();
    abstract public function BuildBody();
    abstract public function BuildArmLeft();
    abstract public function BuildArmRight();
    abstract public function BuildLegLeft();
    abstract public function BuildLegRight();
}
class PersonThinBuilder extends PersonBuilder
{
    public function BuildHead()
    {
        echo "小头\n";
    }
    public function BuildBody()
    {
        echo "小身子\n";
    }
    public function BuildArmRight()
    {
        echo "右臂\n";
    }
    public function BuildArmLeft()
    {
        echo "左臂\n";
    }
    public function BuildLegLeft()
    {
        echo "左腿\n";
    }
    public function BuildLegRight()
    {
        echo "右腿\n";
    }
}
class PersonFatBuilder extends PersonBuilder
{
    public function BuildHead()
    {
        echo "大头\n";
    }
    public function BuildBody()
    {
        echo "大身子\n";
    }
    public function BuildArmRight()
    {
        echo "右臂\n";
    }
    public function BuildArmLeft()
    {
        echo "左臂\n";
    }
    public function BuildLegLeft()
    {
        echo "左腿\n";
    }
    public function BuildLegRight()
    {
        echo "右腿\n";
    }
}
class PersonDirector
{
    private $personBuilder;
    function __construct($personBuilder)
    {
        $this->personBuilder = $personBuilder;
    }
    public function CreatePerson()
    {
        $this->personBuilder->BuildHead();
        $this->personBuilder->BuildBody();
        $this->personBuilder->BuildArmRight();
        $this->personBuilder->BuildArmLeft();
        $this->personBuilder->BuildLegLeft();
        $this->personBuilder->BuildLegRight();
    }
}
//客户端代码
echo "苗条的:\n";
$thinDirector = new PersonDirector(new PersonThinBuilder());
$thinDirector->CreatePerson();
echo "\n胖的:\n";
$fatDirector = new PersonDirector(new PersonFatBuilder());
$fatDirector->CreatePerson();
```

## 总结
> 建造者模式，将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
> 
> 如果我们用了建造者模式，那么用户只需要指定需要建造的类型就可以得到他们，而具体建造的过程和细节就不需要知道了。
> 
> 主要用于创建一些复杂的对象，这些对象内部构建间的建造顺序通常是稳定的，但对象内部的构建通畅面临着复杂的变化。
> 
> 建造者模式是在当创建复杂对象的算法应该独立于改对象的组成部分以及它们的装配方式时适用的模式。

# 12.【观察者模式】老板回来，我不知道
```php
<?php 
abstract class Subject
{
    private $observers = [];
    public function attach(Observer $observer)
    {
        array_push($this->observers, $observer);
    }
    public function detatch($observer)
    {
        foreach ($this->observers as $key => $value) {
            if ($observer === $value) {
                unset($this->observers[$key]);
            }
        }
    }
    public function notify()
    {
        foreach ($this->observers as $observer) {
            $observer->update();
        }
    }
}
abstract class Observer
{
    abstract function update();
}
class ConcreteSubject extends Subject
{
    private $subjectState;
    public function setState($state)
    {
        $this->subjectState = $state;
    }
    public function getState()
    {
        return $this->subjectState;
    }
}
class ConcreteObserver extends Observer
{
    private $name;
    private $subject;
    function __construct(ConcreteSubject $subject, $name)
    {
        $this->subject = $subject;
        $this->name = $name;
    }
    public function update()
    {
        echo "观察者 ".$this->name."的新状态是:".$this->subject->getState()."\n";
    }
}
$s = new ConcreteSubject();
$s->attach(new ConcreteObserver($s, "x"));
$s->attach(new ConcreteObserver($s, "y"));
$z = new ConcreteObserver($s, "z");
$s->attach($z);
$s->detatch($z);
$s->setState('ABC');
$s->notify();
```

## 总结
> 观察者模式，定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态发生变化时，会通知所有观察者对象，使它们能够自动更新自己。
> 
> 将一个系统分割成一系列相互协作的类有一个很不好的副作用，那就是要维护相关对象间的一致性。我们不希望为了维持一致性而使各类紧密耦合，这样会给维护、扩展和重用都带来不便。
> 
> 观察者模式所做的工作其实就是在接触耦合。让耦合的双方都依赖于抽象，而不是依赖于具体。从而使得各自的变化都不会影响另一边的变化。


# 13.【抽象工厂模式】就不能不换DB吗？
```php
<?php 
/////////version1
//数据库访问
class User
{
    private $id = null;
    public function setId($id)
    {
        $this->id = $id;
    }
    public function getId($id)
    {
        return $this->id;
    }
    private $name = null;
    public function setName($name)
    {
        $this->name = $name;
    }
    public function getName($name)
    {
        return $this->id;
    }
}
class Department
{
    private $id = null;
    public function setId($id)
    {
        $this->id = $id;
    }
    public function getId($id)
    {
        return $this->id;
    }
    private $name = null;
    public function setName($name)
    {
        $this->name = $name;
    }
    public function getName($name)
    {
        return $this->id;
    }
}
interface IUser
{
    public function insert(User $user);
    public function getUser($id);
}
class SqlserverUser implements IUser 
{
    public function insert(User $user)
    {
        echo "往SQL Server中的User表添加一条记录\n";
    }
    public function getUser($id)
    {
        echo "根据id得到SQL Server中User表一条记录\n";
    }
}
class AcessUser implements IUser 
{
    public function insert(User $user)
    {
        echo "往Acess Server中的User表添加一条记录\n";
    }
    public function getUser($id)
    {
        echo "根据id得到Acess Server中User表一条记录\n";
    }
}
// interface IFactory
// {
//     public function CreateUser();
//     public function CreateDepartment();
// }
// class SqlserverFactory implements IFactory
// {
//     public function CreateUser()
//     {
//         return new SqlserverUser();
//     }
//     public function CreateDepartment()
//     {
//         return new SqlserverDepartment();
//     }
// }
// class AcessFactory implements IFactory
// {
//     public function CreateUser()
//     {
//         return new AcessUser();
//     }
//     public function CreateDepartment()
//     {
//         return new AcessDepartment();
//     }
// }
//简单工厂替换抽象工厂
class DataBase
{
    const DB = 'Sqlserver';
    // private $db = 'Access';
    public static function CreateUser()
    {   
        $class = static::DB.'User';
        return new $class();
    }
    public static function CreateDepartment()
    {
        $class = static::DB.'Department';
        return new $class();
    }
}
interface IDepartment
{
    public function insert(Department $user);
    public function getDepartment($id);
}
class SqlserverDepartment implements IDepartment 
{
    public function insert(Department $department)
    {
        echo "往SQL Server中的Department表添加一条记录\n";
    }
    public function getDepartment($id)
    {
        echo "根据id得到SQL Server中Department表一条记录\n";
    }
}
class AcessDepartment implements IDepartment 
{
    public function insert(Department $department)
    {
        echo "往Acess Server中的Department表添加一条记录\n";
    }
    public function getDepartment($id)
    {
        echo "根据id得到Acess Server中Department表一条记录\n";
    }
}
//客户端代码
// $user = new User();
// $iu = (new AcessFactory())->CreateUser();
// $iu->insert($user);
// $iu->getUser(1);
// $department = new Department();
// $id = (new AcessFactory())->CreateDepartment();
// $id->insert($department);
// $id->getDepartment(1);
/////////////////////////////////////////////////
//改为简单工厂后的客户端代码
$user = new User();
$iu = DataBase::CreateUser();
$iu->insert($user);
$iu->getUser(1);
$department = new Department();
$id = DataBase::CreateDepartment();
$id->insert($department);
$id->getDepartment(1);
```

## 总结
> 抽象工厂模式，提供一个创建一系列相关或相互依赖对象的接口，而无需指定他们的具体类。
> 
> 菜鸟程序员遇到问题，只会用时间来摆平。
> 
> 工厂方法模式是定义一个用于创建对象的接口，让子类决定实例化哪一个类。
> 
> 抽象工厂模式的好处便是易于交换产品系列，由于具体工厂类，在一个应用中只需要在初始化的时候出现一次，这就使得改变一个应用的具体工厂变得非常容易，它只是需要改变具体工厂即可使用不同的产品配置。它让具体的创建实例过程与客户端分离，客户端是通过它们的抽象接口操作实例，产品的具体类名也被具体工厂的实现分离，不会出现在客户端代码中。
> 
> 编程是门艺术，大批量的改动是非常丑陋的做法。
> 
> 所有在用简单工厂的地方，都可以考虑用反射技术来去除switch或if，解除分支判断代码的耦合。

# 14.【状态模式】无尽加班何时休

```php
//工作状态
abstract class State
{
    abstract public function WriteProgram(Work $w);
}
class ForenoonState extends State
{
    public function WriteProgram(Work $w)
    {
        if ($w->getHour() < 12) {
            echo "当前时间：".$w->getHour()." 上午工作，精神百倍\n";
        } else {
            $w->setState(new NoonState());
            $w->WriteProgram();
        }
    }
}
class NoonState extends State
{
    public function WriteProgram(Work $w)
    {
        if ($w->getHour() < 13) {
            echo "当前时间：".$w->getHour()." 饿了，午饭；犯困，午休\n";
        } else {
            $w->setState(new AfterNoonState());
            $w->WriteProgram();
        }
    }
}
class AfterNoonState extends State
{
    public function WriteProgram(Work $w)
    {
        if ($w->getHour() < 17) {
            echo "当前时间：".$w->getHour()." 下午状态不错，继续努力\n";
        } else {
            $w->setState(new EveningState());
            $w->WriteProgram();
        }
    }
}
class EveningState extends State
{
    public function WriteProgram(Work $w)
    {
        if ($w->getTaskFinishedState()) {
            //如果完成任务，下班
            $w->setState(new RestState());
            $w->WriteProgram();
        } else {
            if ($w->getHour() < 21) {
                echo "当前时间：".$w->getHour()." 加班哦，疲惫之极\n";
            } else {
                //超过21点，则转入睡眠工作状态
                $w->setState(new SleepingState());
                $w->WriteProgram();
            }
        }
    }
}
class SleepingState extends State
{
    public function WriteProgram(Work $w)
    {
        echo "当前时间：".$w->getHour()." 不行了，睡觉\n";
    }
}
class RestState extends State
{
    public function WriteProgram(Work $w)
    {
        echo "当前时间：".$w->getHour()." 下班回家\n";
    }
}
class Work
{
    private $current;
    function __construct()
    {
        $this->current = new ForenoonState();
    }
    private $hour;
    public function getHour()
    {
        return $this->hour;
    }
    public function setHour($hour)
    {
        $this->hour = $hour;
    }
    private $finished = false;
    public function setTaskFinished($bool)
    {
        $this->finished = $bool;
    }
    public function getTaskFinishedState()
    {
        return $this->finished;
    }
    public function setState(State $state)
    {
        $this->current = $state;
    }
    public function WriteProgram()
    {
        $this->current->WriteProgram($this);
    }
}
//客户端代码
$emergencyProjects = new Work();
$emergencyProjects->setHour(9);
$emergencyProjects->WriteProgram();
$emergencyProjects->setHour(10);
$emergencyProjects->WriteProgram();
$emergencyProjects->setHour(12);
$emergencyProjects->WriteProgram();
$emergencyProjects->setHour(13);
$emergencyProjects->WriteProgram();
$emergencyProjects->setHour(14);
$emergencyProjects->WriteProgram();
$emergencyProjects->setHour(17);
$emergencyProjects->setTaskFinished(false);
$emergencyProjects->WriteProgram();
$emergencyProjects->setHour(19);
$emergencyProjects->WriteProgram();
$emergencyProjects->setHour(22);
$emergencyProjects->WriteProgram();
```

## 总结
> 状态模式，当一个对象的内在状态改变时允许改变其行为，这个对象看起来像是改变了其类。
> 
> 面向对象设计其实就是希望做到代码的责任分解。
> 
> 状态模式主要解决的当控制一个对象状态转换的条件表达式过于复杂时的情况。把状态的判断逻辑转移到表示不同状态的一系列类当中，可以把复杂的判断逻辑简单化。
> 
> 将于特定状态相关的行为局部化，并且将不同状态的行为分割开来。
> 
> 将特定的状态相关的行为都放入一个对象中，由于所有与状态相关的代码都存在于某个ConcreteState中，所以通过定义的子类可以很容易地增加新的状态和转换。
> 
> 消除了庞大的条件分支语句。
> 
> 状态模式通过把各种状态转移逻辑分布到State的子类之间，来减少项目之间的依赖。
> 
> 当一个对象的行为取决于它的状态，并且它必须在运行时刻根据状态改变它的行为时，就可以考虑使用状态模式了。


# 15.【适配器模式】在NBA我需要翻译
```php
<?php 
//篮球翻译适配器
abstract class Player
{
    protected $name;
    function __construct($name)
    {
        $this->name = $name;
    }
    abstract public function Attack();
    abstract public function Defense();
}
//前锋
class Forwards extends Player
{
    public function Attack()
    {
        echo "前锋:".$this->name." 进攻\n";
    }
    public function Defense()
    {
        echo "前锋:".$this->name." 防守\n";
    }
}
//中锋
class Center extends Player
{
    function __construct()
    {
        parent::__construct();
    }
    public function Attack()
    {
        echo "中锋:".$this->name." 进攻\n";
    }
    public function Defense()
    {
        echo "中锋:".$this->name." 防守\n";
    }
}
//外籍中锋
class ForeignCenter
{
    private $name;
    public function setName($name)
    {
        $this->name = $name;
    }
    public function getName()
    {
        return $this->name;
    }
    public function 进攻()
    {
        echo "外籍中锋:".$this->name." 进攻\n";
    }
    public function 防守()
    {
        echo "外籍中锋:".$this->name." 防守\n";
    }
}
//翻译者
class Translator extends Player
{
    private $foreignCenter;
    function __construct($name)
    {
        $this->foreignCenter = new ForeignCenter();
        $this->foreignCenter->setName($name);
    }
    public function Attack()
    {
        $this->foreignCenter->进攻();
    }
    public function Defense()
    {
        $this->foreignCenter->防守();
    }
}
// 客户端代码
$forwards = new Forwards("巴蒂尔");
$forwards->Attack();
$forwards->Defense();
$translator = new Translator("姚明");
$translator->Attack();
$translator->Defense();
```

## 总结

> 适配器模式，将一个类的接口转化成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
> 
> 系统的数据和行为都正确，但接口不符时，我们应该考虑用适配器，目的是使控制范围之外的一个原有对象与某个接口匹配。适配器模式主要应用于希望复用一些现存的类。但是接口又与复用环境要求不一致的情况。
> 
> 两个类所做的事情相同或相似，但是具有不同的接口时要使用它。
> 
> 在双方都不太容易修改的时候再使用适配器模式适配。

# 16.【备忘录模式】如果再回到从前

```php
<?php
//发起人类
class Originator
{   
    // 需要保存的属性，可能有多个
    private $state;
    public function setState($state)
    {
        $this->state = $state;
    }
    public function getState()
    {
        return $this->state;
    }
    //创建备忘录，将当前需要保存的信息导入并实例化出一个memento对象。
    public function createMemento()
    {
        return new Memento($this->state);
    }
    //恢复备忘录，将memento导入并将相关数据恢复。
    public function setMemento(Memento $memento)
    {   
        $this->state = $memento->getState();
    }
    //显示数据
    public function show()
    {
        echo "status ".$this->state."\n";
    }
}
//备忘录类
class Memento
{
    private $state;
    //构造方法，将相关数据导入
    function __construct($state)
    {
        $this->state = $state;
    }
    //获取需要保存的数据，可以多个
    public function getState()
    {
        return $this->state;
    }
}
//管理者类
class CareTaker
{
    private $memento;
    public function getMemento()
    {   
        return $this->memento;
    }
    //设置备忘录
    public function setMemento(Memento $memento)
    {   
        $this->memento = $memento;
    }
}
//客户端程序
$o = new Originator(); //Originator初始状态，状态属性on
$o->setState("On");
$o->show();
//保存状态时，由于有了很好的封装，可以隐藏Originator的实现细节
$c = new CareTaker();
$c->setMemento($o->createMemento());
// 改变属性
$o->setState("Off");
$o->show();
// 恢复属性
$o->setMemento($c->getMemento());
$o->show();
```

## 总结
> 备忘录模式，在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之前保存这个状态。这样以后就可将该对象恢复到原先保存的状态。
> 
> 代码无措未必优
> 
> 如果在某个系统中使用命令模式时，需要实现命令的撤销功能，那么命令模式可以使用备忘录模式来存储可撤销操作的状态。
> 
> 使用备忘录可以把复杂的对象内部信息对其他的对象屏蔽起来。

# 17.【组合模式】分公司=一部分
```php
<?php
// component为组合中的对象接口，在适当的情况下，实现所有类共有接口的默认行为。声明一个接口用于访问和管理Component的字部件。
abstract class Component
{
    protected $name;
    function __construct($name)
    {
        $this->name = $name;
    }
    //通常用add和remove方法来提供增加或移除树枝货树叶的功能
    abstract public function add(Component $c);
    abstract public function remove(Component $c);
    abstract public function display($depth);
}
//leaf在组合中表示叶节点对象，叶节点对象没有子节点。
class Leaf extends Component
{   
    // 由于叶子没有再增加分枝和树叶，所以add和remove方法实现它没有意义，
    // 但这样做可以消除叶节点和枝节点对象在抽象层次的区别，它们具有完全一致的接口
    public function add(Component $c)
    {
        echo "can not add to a leaf\n";
    }
    public function remove(Component $c)
    {
        echo "can not remove to a leaf\n";
    }
    // 叶节点的具体方法，此处是显示其名称和级别
    public function display($depth)
    {
        echo str_repeat('-', $depth).$this->name."\n";
    }
}
//composite定义有枝节点行为，用来存储子部件，在Component接口中实现与子部件有关的操作，比如增加add和删除remove.
class Composite extends Component
{
    //一个子对象集合用来存储其下属的枝节点和叶节点。
    private $childern = [];
    public function add(Component $c)
    {
        array_push($this->childern, $c);
    }
    public function remove(Component $c)
    {
        foreach ($this->childern as $key => $value) {
            if ($c === $value) {
                unset($this->childern[$key]);
            }
        }
    }
    // 显示其枝节点名称，并对其下级进行遍历
    public function display($depth)
    {
        echo str_repeat('-', $depth).$this->name."\n";
        foreach ($this->childern as $component) {
            $component->display($depth + 2);
        }
    }
}
//客户端代码
$root = new Composite('root');
$root->add(new Leaf("Leaf A"));
$root->add(new Leaf("Leaf B"));
$comp = new Composite("Composite X");
$comp->add(new Leaf("Leaf XA"));
$comp->add(new Leaf("Leaf XB"));
$root->add($comp);
$comp2 = new Composite("Composite X");
$comp2->add(new Leaf("Leaf XA"));
$comp2->add(new Leaf("Leaf XB"));
$comp->add($comp2);
$root->add(new  Leaf("Leaf C"));
$leaf = new Leaf("Leaf D");
$root->add($leaf);
$root->remove($leaf);
$root->display(1);
```

## 总结
> 组合模式，将对象组合成树形结构以表示‘部分与整体’的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。
> 
> 透明方式，子类的所有接口一致，虽然有些接口没有用。
> 
> 安全方式，子类接口不一致，只实现特定的接口，但是这样就要做相应的判断，带来了不便。
> 
> 需求中是体现部分与整体层次的结构时，或希望用户可以忽略组合对象与单个对象的不同，统一地使用组合结构中的所有对象时，就应该考虑用组合模式了。
> 
> 组合模式可以让客户一致地使用组合结构和单个对象。


# 18.【迭代器模式】想走？可以！先买票
```php
<?php 
//迭代器抽象类
abstract class IteratorClass
{
    abstract public function first();
    abstract public function next();
    abstract public function isDone();
    abstract public function currentItem();
}
// 聚集抽象类
abstract class Aggregate
{
    abstract function createIterator();
}
class ConcreteIterator extends IteratorClass
{
    private $aggregate;
    private $current = 0;
    function __construct($aggregate)
    {
        $this->aggregate = $aggregate;
    }
    public function first()
    {
        return $this->aggregate[0];
    }
    public function next()
    {
        $ret = null;
        $this->current++;
        if ($this->current < count($this->aggregate))
        {
            $ret = $this->aggregate[$this->current];
        }
        return $ret;
    }
    public function isDone()
    {
        return $this->current >= count($this->aggregate);
    }
    public function currentItem()
    {
        return $this->aggregate[$this->current];
    }
}
//这个类的代码感觉不符合书上的写法，但我感觉书上的不对，可能我知识面太单薄，没读懂，可自行参阅原著?。
class ConcreteAggregate extends Aggregate
{
    private $items = [];
    public function createIterator()
    {
        return new ConcreteIterator($this);
    }
    public function count()
    {
        return count($this->items);
    }
    public function add($item)
    {
        array_push($this->items, $item);
    }
    public function items()
    {
        return $this->items;
    }
}
//客户端代码
$a = new ConcreteAggregate();
$a->add("大鸟");
$a->add("小菜");
$a->add("行李");
$a->add("老外");
$a->add("公交内部员工");
$a->add("小偷");
$i = new ConcreteIterator($a->items());
while (!$i->isDone()) 
{
    echo $i->currentItem()." 请买票\n";
    $i->next();
}
```

## 总结
> 当你需要对聚集有多种方式遍历时，可以考虑用迭代器模式。
> 
> 当你需要访问一个聚集对象，而且不管这些对象是什么都需要遍历的时候，你就应该考虑用迭代器模式。
> 
> 为遍历不同的聚集结构提供如开始、下一个、是否结束、当前哪一项等统一的接口。
> 
> 迭代器模式就是分离了集合对象的遍历行为，抽象出一个迭代器类来负责，这样既可以做到不暴露集合内部的结构，又可让外部代码透明地访问集合内部的数据。


# 19.【单例模式】有些类也需要计划生育

```php
<?php 
class Singleton
{
    private static $instance;
    private function __construct(){}
    public static function getInstance()
    {
        if (static::$instance == null) 
        {
            static::$instance = new Singleton();
        }
        return static::$instance;
    }
}
//客户端代码
$s1 = Singleton::getInstance();
$s2 = Singleton::getInstance();
if ($s1 == $s2) 
{
    echo "same class";
}
```

## 总结
> 单例模式，保证一个类仅有一个实例，并提供一个访问它的全局访问点。
> 
> 单例模式因为Singleton类封装它的唯一实例，这样它可以严格地控制客户怎样访问以及何时访问它。简单地说就是对唯一实例的受控访问。



# 20.【桥接模式】手机软件何时统一
```php
<?php 
//手机软件
abstract class HandsetSoft
{
    abstract public function run();
}
//游戏、通讯录等具体类
//手机游戏
class HandsetGame extends HandsetSoft
{
    public function run()
    {
        echo "运行手机游戏\n";
    }
}
//手机通讯录
class HandsetAddressList extends HandsetSoft
{
    public function run()
    {
        echo "运行手机通讯录\n";
    }
}
//手机品牌类
abstract class HandsetBrand
{
    protected $soft;
    //设置手机软件
    public function setHandsetSoft(HandsetSoft $soft)
    {
        $this->soft = $soft;
    }
    //运行
    abstract public function run();
}
// 手机品牌n
class HandsetBrandN extends HandsetBrand
{
    public function run()
    {
        $this->soft->run();
    }
}
// 手机品牌m
class HandsetBrandM extends HandsetBrand
{
    public function run()
    {
        $this->soft->run();
    }
}
//客户端调用代码
$ab = new HandsetBrandN();
$ab->setHandsetSoft(new HandsetGame());
$ab->run();
$ab->setHandsetSoft(new HandsetAddressList());
$ab->run();
$ab = new HandsetBrandM();
$ab->setHandsetSoft(new HandsetGame());
$ab->run();
$ab->setHandsetSoft(new HandsetAddressList());
$ab->run();
```
## 总结
> 桥接模式，将抽象部分与它的实现部分分离，使它们都可以独立地变化。
> 
> 合成/聚合复用原则，尽量使用合成/聚合，尽量不要使用类继承。
> 
> 聚合表示弱的‘拥有’关系，体现的是A对象可以包含B对象，但B对象不是A对象的一部分；合成则是一种强的‘拥有’关系，体现了严格的部分和整体的关系，部分和整体的生命周期一样。
> 
> 对象的继承关系是在编译时就定义好了，所以无法在运行时改变从父类继承的实现。子类的实现与它的父类有非常紧密的依赖关系，以至于父类实现中的任何变化必然会导致子类发生变化。当你需要复用子类时，如果继承下来的实现不适合解决新的问题，则父类必须重写或被其他更合适的类替换。这种依赖关系限制了灵活性并最终限制了复用性。
> 
> 优先使用对象的合成/聚合将有助于你保持每个类被封装，并被集中在单个任务上。这样类和类继承层次会保持较小规模，并且不太可能增长为不可控制的庞然大物。
> 
> 什么叫抽象与它的实现分离，这并不是说，让抽象类与其派生类分离，因为这没有任何意义。实现指的是抽象类和它的派生类用来实现自己的对象。
> 
> 实现系统可能有多角度分类，每一种分类都有可能变化，那么就把这种多角度分离出来让它们独立变化，减少它们之间的耦合。
> 
> 只要真正深入地理解了设计原则，很多设计模式其实就是原则的应用而已，或许在不知不觉中就在使用设计模式了

# 21.【命令模式】烤羊肉串引来的思考
```php
<?php 
//烤串
class Barbecuer
{
    public function bakeMutton()
    {
        echo "烤羊肉串\n";
    }
    public function bakeChickenWing()
    {
        echo "烤鸡翅\n";
    }
}
// 抽象命令
abstract  class  Command
{
    protected $receiver;
    function __construct(Barbecuer $receiver)
    {
        $this->receiver = $receiver;
    }
    abstract public function excuteCommand();
}
//烤羊肉
class BakeMuttonCommand extends Command
{
    public function excuteCommand()
    {
        $this->receiver->bakeMutton();
    }
}
//烤鸡翅
class BakeChickenWingCommand extends Command
{
    public function excuteCommand()
    {
        $this->receiver->bakeChickenWing();
    }
}
//服务员
class Waiter
{
    private $commands = [];
    //设置订单
    public function setOrder(Command $command)
    {
        if ($command instanceof BakeChickenWingCommand)
        {
            echo "服务员： 鸡翅没有了，请点别的烧烤\n";
        } else {
            echo "增加订单\n";
            array_push($this->commands, $command);
        }
    }
    //取消订单
    public function cancelOrder(Command $command){}
    //通知执行
    public function notify()
    {
        foreach ($this->commands as $value) {
            $value->excuteCommand();
        }
    }
}
//客户端代码
//开店前准备
$boy = new Barbecuer();
$bakeMuttonCommand1 = new BakeMuttonCommand($boy);
$bakeMuttonCommand2 = new BakeMuttonCommand($boy);
$bakeChickenWingCommand1 = new BakeChickenWingCommand($boy);
$girl = new Waiter();
//开门营业
$girl->setOrder($bakeMuttonCommand1);
$girl->setOrder($bakeMuttonCommand2);
$girl->setOrder($bakeChickenWingCommand1);
$girl->notify();
```

## 总结

> 命令模式，将 一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤销的操作。
> 
> 对请求派对或记录请求日志，以及日志可撤销的操作。
> 
> 优点：第一，能较容易地设计一个命令队列；第二，在需要的情况下，可以较容易地将命令记入日志；第三，允许请求的一方决定是否要否决请求；第四，可以容易地实现对请求的撤销和重做；第五，由于加进新的具体命令类不影响其他的类，因此增加新的具体命令类很容易；最重要的是该 模式把请求一个操作的对象与知道怎么执行一个操作的对象分隔开。
> 
> 敏捷开发原则告诉我们，不要为代码添加基于猜测的、实际不需要的功能。如果不清楚一个系统是否需要命令模式，一般就不要急着去实现它，事实上，在需要的时候通过重构实现这个模式并不难，只有在真正需要如撤销/恢复操作等功能时，把原来的代码重构为命令模式才有意义。


# 22.【职责链模式】加薪非要老总批
```php
abstract class Handler
{
    protected $successor;
    //设置继承者
    public function setSuccessor(Handler $successor)
    {
        $this->successor = $successor;
    }
    //处理请求的抽象方法
    abstract function handleRequest(int $request);
}
//如果可以处理请求，就处理之，否者转发给它的后继者
class ConcreteHandler1 extends Handler
{
    public function handleRequest(int $request)
    {
        if ($request >=0 && $request < 10)
        {
            echo "ConcreteHandler1 handle it\n";
        } else if ($this->successor != null) {
             // 转移
            $this->successor->handleRequest($request);
        }
    }
}
class ConcreteHandler2 extends Handler
{
    public function handleRequest(int $request)
    {
        if ($request >=10 && $request < 20)
        {
            echo "ConcreteHandler2 handle it\n";
        } else if ($this->successor != null) {
            $this->successor->handleRequest($request);
        }
    }
}
// client
$h1 = new ConcreteHandler1();
$h2 = new ConcreteHandler2();
设置职责链上下家
$h1->setSuccessor($h2);
$requests = [1,5,7,16,25];
循环给最小处理者提交请求，不同的数额，由不同权限处理者处理
foreach ($requests as $value) {
    $h1->handleRequest($value);
}
```
## 总结

> 职责链模式， 使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系。将这个对象连成一条链，并沿着这条链传递该请求，直到有一个对像处理它为止。
> 
> 当用户提交一个请求时，请求是沿着链传递直至有一个对象负责处理它。
> 
> 接受者和发送者都没有对方的明确信息，且链中的对象自己也并不知道链的结构。结果是职责链可简化对象的相互连接，它们仅需要保持一个向其后继者的引用，而不需要保持它所有的候选者的引用。
> 
> 随时地增加或修改处理一个请求的结构。增强了给对象指派职责的灵活性。
> 
> 一个请求极有可能到了链的末端都得不到处理，或者因为没有正确配置而得不到处理。


# 23.【中介者模式】世界需要和平
```php
<?php
abstract class Mediator
{
    abstract public function send($message, Colleague $colleague);
}
abstract class Colleague
{
    protected $mediator;
    function __construct(Mediator $mediator)
    {
        $this->mediator = $mediator;
    }
}
class ConcreteMediator extends Mediator
{
    private $colleague1;
    private $colleague2;
    public function setColleague1(Colleague $colleague)
    {
        $this->colleague1 = $colleague;
    }
    public function setColleague2(Colleague $colleague)
    {
        $this->colleague2 = $colleague;
    }
    public function send($message, Colleague $colleague)
    {
        if($this->colleague1 == $colleague)
        {
            $this->colleague2->notify($message);
        } else {
            $this->colleague1->notify($message);
        }
    }
}
class ConcreteColleague1 extends Colleague
{
    public function send($message)
    {
        $this->mediator->send($message, $this);
    }
    public function notify($message)
    {
        echo "ConcreteColleague1 ".$message."\n";
    }
}
class ConcreteColleague2 extends Colleague
{
    public function send($message)
    {
        $this->mediator->send($message, $this);
    }
    public function notify($message)
    {
        echo "ConcreteColleague2 ".$message."\n";;
    }
}
//client
$mediator = new ConcreteMediator();
$c1 = new ConcreteColleague1($mediator);
$c2 = new ConcreteColleague2($mediator);
$mediator->setColleague1($c1);
$mediator->setColleague2($c2);
$c1->send('do you eat?');
$c2->send('no, do you want to invite me to dinner?');
```

## 总结
> 中介者模式，用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显示地相互交互，从而使其耦合松散，而且可以独立地改变它们之间的交互。
> 
> 尽管将一个系统分割成许多对象通常可以增加其可复用性，但是对象间的交互连接的激增又会降低其可复用性了。
> 
> 大量的连接使得一个对象不大可能在没有其他对象的支持下工作，系统表现为一个不可分割的整体，所以，对系统的行为进行任何较大的改动就十分困难了。
> 
> 中介者模式很容易在系统中应用，也很容易在系统中误用。当系统出现了‘多对多’交互复杂的对象群时，不要急于使用中介者模式，而要反思你的系统设计上是不是合理。
> 
> 由于把对象如何协作进行了抽象，将中介作为一个独立的概念并将其封装在一个对象中，这样关注的对象就从对象各自本身的行为转移到它们之间的交互上来，也就是站在一个更宏观的角度去看待系统。
> 
> 中介者模式一般应用于一组对象以定义良好但是复杂的方式进行通信的结合，以及想定制一个分布在多个类中的行为，而又不想生成太多的子类的场合。

# 24.【享元模式】项目多也别做傻事
```php
class User
{
    private $name;
    function __construct($name)
    {
        $this->name = $name;
    }
    public function getName()
    {
        return $this->name;
    }
}
abstract class WebSite
{
    abstract public function use(User $user);
}
// 具体网站类
class ConcreteWebSite extends WebSite
{
    private $name = '';
    function __construct($name)
    {
        $this->name = $name;
    }
    public function use(User $user)
    {
        echo "网站分类: ".$this->name."用户:".$user->getName()."\n";
    }
}
//网站工厂
class WebSiteFactory
{
    private $flyweights = [];
    public function getWebSiteGategory($key)
    {
        if (empty($this->flyweights[$key])) {
            $this->flyweights[$key] = new ConcreteWebSite($key);
        }
        return $this->flyweights[$key];
    }
    public function getWebSiteCount()
    {
        return count($this->flyweights);
    }
}
$f = new WebSiteFactory();
$fx = $f->getWebSiteGategory('产品展示');
$fx->use(new User('张伟'));
$fy = $f->getWebSiteGategory('产品展示');
$fy->use(new User('王伟'));
$fz = $f->getWebSiteGategory('产品展示');
$fz->use(new User('王芳'));
$fl = $f->getWebSiteGategory('博客');
$fl->use(new User('李伟'));
$fm = $f->getWebSiteGategory('博客');
$fm->use(new User('王秀英'));
$fn = $f->getWebSiteGategory('博客');
$fn->use(new User('李秀英'));
echo "网站分类总数:".$f->getWebSiteCount()."\n";
```

## 总结
> 享原模式 运用共享技术有效地支持大量细粒度的对象
> 
> 享原模式可以避免大量非常类似类的开销。在程序设计中，有时需要生成大量细粒度的类实例来表示数据。如果能发现这些实例除了几个参数外基本上都是相同的，有时就能够受大幅度地减少需要实例化的类的数量。如果能把参数移到类实例的外面，在方法调用时将它们传递进来，就可以通过共享大幅度地减少单个实例的数目。
> 
> 如果一个应用程序使用了大量的对象，而大量的这些对象造成了很大的存储开销时就应该考虑使用；还有就是对象的大多数状态可以外部状态，如果删除对象的外部状态，那么可以用相对较少的共享对象取代很多组对象，此时可以考虑使用享原模式。


# 25.【解释器模式】其实你不懂老板的心
```php
<?php
interface AbstractExpression
{
    public function interpret(Context $context);
}
class TerminalExpression implements AbstractExpression
{
    public function interpret(Context $context)
    {
        echo "终端解释器\n";
    }
}
class NonTerminalExpression implements AbstractExpression
{
    public function interpret(Context $context)
    {
        echo "非终端解释器\n";
    }
}
class Context
{
    private $input;
    public function setInput($input)
    {
        $this->input = $input;
    }
    public function getInput()
    {
        return $this->input;
    }
    private $output;
    public function setOutput($output)
    {
        $this->output = $output;
    }
    public function getOutput()
    {
        return $this->output;
    }
}
$context = new Context();
$syntax = [];
array_push($syntax, new TerminalExpression());
array_push($syntax, new NonTerminalExpression());
array_push($syntax, new TerminalExpression());
array_push($syntax, new TerminalExpression());
foreach ($syntax as $value) {
    $value->interpret($context);
}
```

## 总结
> 解释器模式，给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言的句子。
> 
> 如果一种特定语言类型的问题发生的频率足够高，那么可能就值得将该问题的各个实例表述为一个简单语言的句子。这样就可以构建一个解释器，该解释器通过解释这些句子来解决该问题。
> 
> 通常当有一个语言需要解释执行，并且你可将该语言的句子表示为一个抽象语法树时，可使用解释器模式。
> 
> 解释器模式容易修改和扩展文法，因为解释器模式使用类来表示文法规则，你可使用继承来改变或扩展该文法。也比较容易实现文法，因为定义抽象语法树中各个节点的类的实现大体类似，这些类都易于直接编写。
> 
> 解释器模式不足的是要为文法中的每一条规则至少定义了一个类，因此包含许多规则的文法可能难以管理和维护。建议当文法非常复杂时，使用其他的技术如语法分析程序或编译器生成器来处理。


# 26.【访问者模式】男人和女人
```php
<?php 
abstract class Action
{
    abstract public function getManConclusion(Man $concreteElementA);
    abstract public function getWomanConclusion(Woman $concreteElementB);
}
abstract class Person
{
    abstract public function accept(Action $visitor);
}
class Success extends Action
{
    public function getManConclusion(Man $concreteElementA)
    {
        echo "背后多半有一个伟大的女人\n";
    }
    public function getWomanConclusion(Woman $concreteElementB)
    {
        echo "背后多有一个不成功的男人\n";
    }
}
class Failing extends Action
{
    public function getManConclusion(Man $concreteElementA)
    {
        echo "男人失败时，闷头喝酒，谁也不用劝\n";
    }
    public function getWomanConclusion(Woman $concreteElementB)
    {
        echo "女人失败时，眼泪汪汪，谁也劝不了\n";
    }
}
class Amativeness extends Action
{
    public function getManConclusion(Man $concreteElementA)
    {
        echo "男人恋爱时，凡事不懂也要装懂\n";
    }
    public function getWomanConclusion(Woman $concreteElementB)
    {
        echo "女人恋爱时，遇事懂也装作不懂\n";
    }
}
class Man extends Person
{
    public function accept(Action $visitor)
    {
        $visitor->getManConclusion($this);
    }
}
class Woman extends Person
{
    public function accept(Action $visitor)
    {
        $visitor->getWomanConclusion($this);
    }
}
class ObjectStructure
{
    private $person = [];
    public function acctch(Person $person)
    {
        array_push($this->person, $person);
    }
    public function display(Action $visitor)
    {
        foreach ($this->person as $person) {
            $person->accept($visitor);
        }
    }
}
$o = new ObjectStructure();
$o->acctch(new Man());
$o->acctch(new Woman());
// 成功时的反应
$v1 = new Success();
$o->display($v1);
$v2 = new Failing();
$o->display($v2);
$v3 = new Amativeness();
$o->display($v3);
```

## 总结
> 访问者模式，表示一个作用于某对象结构中的各个元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。
> 
> 访问者模式适合用于数据结构相对稳定的系统。它把数据结构和作用于结构上的操作之间的耦合解脱开，使得操作集合可以相对自由地演化。
> 
> 访问者的目的是要把处理从数据结构分离出来。这样系统有比较稳定的数据结构，又有易于变化的算法的话，使用访问者模式就是比较合适的，因为访问者模式使得算法操作的增加变得容易。
> 
> 增加新的操作容易，因为增加新的操作就意味着增加一个新的访问者。访问者将有关行为集中到一个访问者对象中。
> 
> 访问者模式使增加新的数据结构变得困难了。























