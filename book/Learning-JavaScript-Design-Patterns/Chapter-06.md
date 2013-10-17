####（Revealing Module Pattern）暴露式模块模式

既然我们对模块模式已经有一些了解了，让我们看一下改进版本 - Christian Heilmann 的启发式模块模式。

启发式模块模式来自于，当Heilmann对这样一个现状的不满，即当我们想要在一个公有方法中调用另外一个公有方法，或者访问公有变量的时候，
我们不得不重复主对象的名称。他也不喜欢模块模式中，当想要将某个成员变成公共成员时，修改文字标记的做法。

因此他工作的结果就是一个更新的模式，在这个模式中，我们可以简单地在私有域中定义我们所有的函数和变量，并且返回一个匿名对象，
这个对象包含有一些指针，这些指针指向我们想要暴露出来的私有成员，使这些私有成员公有化。

```javascript
var myRevealingModule = function () {
        var privateVar = "Ben Cherry",
            publicVar  = "Hey there!";

        function privateFunction() {
            console.log( "Name:" + privateVar );
        }

        function publicSetName( strName ) {
            privateVar = strName;
        }

        function publicGetName() {
            privateFunction();
        }


        // Reveal public pointers to
        // private functions and properties

        return {
            setName: publicSetName,
            greeting: publicVar,
            getName: publicGetName
        };

}();

myRevealingModule.setName( "Paul Kinlan" );
```
这个模式可以用于将私有函数和属性以更加规范的命名方式展现出来。


```javascript
var myRevealingModule = function () {

        var privateCounter = 0;

        function privateFunction() {
            privateCounter++;
        }

        function publicFunction() {
            publicIncrement();
        }

        function publicIncrement() {
            privateFunction();
        }

        function publicGetCount(){
          return privateCounter;
        }

        // Reveal public pointers to
        // private functions and properties

       return {
            start: publicFunction,
            increment: publicIncrement,
            count: publicGetCount
        };

    }();

myRevealingModule.start();
```

####优势

这个模式是我们脚本的语法更加一致。同样在模块的最后关于那些函数和变量可以被公共访问也变得更加清晰，增强了可读性

####缺点

这个模式的一个缺点是如果私有函数需要使用公有函数，那么这个公有函数在需要打补丁的时候就不能被重载。因为私有函数仍然使用的是私有的实现，并且这个模式不能用于公有成员，只用于函数。
公有成员使用私有成员也遵循上面不能打补丁的规则。
 因为上面的原因，使用暴露式模块模式创建的模块相对于原始的模块模式更容易出问题，因此在使用的时候需要小心。


###单例模式

 单例模式之所以这么叫，是因为它限制一个类只能有一个实例化对象。经典的实现方式是，创建一个类，这个类包含一个方法，这个方法在没有对象存在的情况下，将会创建一个新的实例对象。
 如果对象存在，这个方法只是返回这个对象的引用。

 单例和静态类不同，因为我们可以退出单例的初始化时间。通常这样做是因为，在初始化的时候需要一些额外的信息，而这些信息在声明的时候无法得知。
 对于并不知晓对单例模式引用的代码来讲，单例模式没有为它们提供一种方式可以简单的获取单例模式。
 这是因为，单例模式既不返回对象也不返回类，它只返回一种结构。可以类比闭包中的变量不是闭包-提供闭包的函数域是闭包（绕进去了）。

 在JavaScript语言中, 单例服务作为一个从全局空间的代码实现中隔离出来共享的资源空间是为了提供一个单独的函数访问指针。

 我们能像这样实现一个单例:

```javascript
var mySingleton = (function () {
  // Instance stores a reference to the Singleton
  var instance;

  function init() {

    // 单例

    // 私有方法和变量
    function privateMethod(){
        console.log( "I am private" );
    }

    var privateVariable = "Im also private";

    var privateRandomNumber = Math.random();

    return {

      // 共有方法和变量
      publicMethod: function () {
        console.log( "The public can see me!" );
      },

      publicProperty: "I am also public",

      getRandomNumber: function() {
        return privateRandomNumber;
      }

    };

  };

  return {
    // 如果存在获取此单例实例，如果不存在创建一个单例实例
    getInstance: function () {
      if ( !instance ) {
        instance = init();
      }

      return instance;
    }

  };

})();

var myBadSingleton = (function () {
  // 存储单例实例的引用
  var instance;
  function init() {
    // 单例
    var privateRandomNumber = Math.random();
    return {
      getRandomNumber: function() {
        return privateRandomNumber;
      }
    };
  };

  return {

    // 总是创建一个新的实例
    getInstance: function () {
      instance = init();
      return instance;
    }

  };

})();


// 使用:

var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
console.log( singleA.getRandomNumber() === singleB.getRandomNumber() ); // true

var badSingleA = myBadSingleton.getInstance();
var badSingleB = myBadSingleton.getInstance();
console.log( badSingleA.getRandomNumber() !== badSingleB.getRandomNumber() ); // true
```
创建一个全局访问的单例实例 (通常通过 MySingleton.getInstance()) 因为我们不能(至少在静态语言中) 直接调用 new MySingleton() 创建实例. 这在JavaScript语言中是不可能的.



在四人帮(GoF)的书里面，单例模式的应用描述如下：

每个类只有一个实例，这个实例必须通过一个广为人知的接口，来被客户访问。
子类如果要扩展这个唯一的实例，客户可以不用修改代码就能使用这个扩展后的实例。
关于第二点，可以参考如下的实例，我们需要这样编码:
```javascript
mySingleton.getInstance = function(){
  if ( this._instance == null ) {
    if ( isFoo() ) {
       this._instance = new FooSingleton();
    } else {
       this._instance = new BasicSingleton();
    }
  }
  return this._instance;
};
```
在这里，getInstance 有点类似于工厂方法，我们不需要去更新每个访问单例的代码。FooSingleton可以是BasicSinglton的子类，并且实现了相同的接口。

为什么对于单例模式来讲，延迟执行执行这么重要？

在c++代码中，单例模式将不可预知的动态初始化顺序问题隔离掉，将控制权返回给程序员。
区分类的静态实例和单例模式很重要：尽管单例模式可以被实现成一个静态实例，但是单例可以懒构造，在真正用到之前，单例模式不需要分配资源或者内存。

如果我们有个静态对象可以被直接初始化，我们需要保证代码总是以同样的顺序执行（例如 汽车需要轮胎先初始化）当你有很多源文件的时候，这种方式没有可扩展性。


单例模式和静态对象都很有用，但是不能滥用-同样的我们也不能滥用其它模式。

在实践中，当一个对象需要和另外的对象进行跨系统协作的时候，单例模式很有用。下面是一个单例模式在这种情况下使用的例子：
```javascript
var SingletonTester = (function () {

  // options: an object containing configuration options for the singleton
  // e.g var options = { name: "test", pointX: 5};
  function Singleton( options )  {

    // set options to the options supplied
    // or an empty object if none are provided
    options = options || {};

    // set some properties for our singleton
    this.name = "SingletonTester";

    this.pointX = options.pointX || 6;

    this.pointY = options.pointY || 10;

  }

  // our instance holder
  var instance;

  // an emulation of static variables and methods
  var _static  = {

    name:  "SingletonTester",

    // Method for getting an instance. It returns
    // a singleton instance of a singleton object
    getInstance:  function( options ) {
      if( instance  ===  undefined )  {
        instance = new Singleton( options );
      }

      return  instance;

    }
  };

  return  _static;

})();

var singletonTest  =  SingletonTester.getInstance({
  pointX:  5
});

// Log the output of pointX just to verify it is correct
// Outputs: 5
console.log( singletonTest.pointX );

```
尽管单例模式有着合理的使用需求，但是通常当我们发现自己需要在javascript使用它的时候，这是一种信号，表明我们可能需要去重新评估自己的设计。

这通常表明系统中的模块要么紧耦合要么逻辑过于分散在代码库的多个部分。单例模式更难测试，因为可能有多种多样的问题出现，例如隐藏的依赖关系，很难去创建多个实例，很难清理依赖关系，等等。

要想进一步了解关于单例的信息，可以读读Miller Medeiros 推荐的这篇非常棒的关于单例模式以及单例模式各种各样问题的
[文章](http://www.ibm.com/developerworks/webservices/library/co-single/index.html)，
也可以看看[这篇文章](http://misko.hevery.com/2008/10/21/dependency-injection-myth-reference-passing/)的评论，
这些评论讨论了单例模式是怎样增加了模块间的紧耦合。
我很乐意去支持这些推荐，因为这两篇文章提出了很多关于单例模式重要的观点，而这些观点是很值得重视的。


###观察者模式

观察者模式是这样一种设计模式。一个被称作被观察者的对象，维护一组被称为观察者的对象，这些对象依赖于被观察者，被观察者自动将自身的状态的任何变化通知给它们。

当一个被观察者需要将一些变化通知给观察者的时候，它将采用广播的方式，这条广播可能包含特定于这条通知的一些数据。

当特定的观察者不再需要接受来自于它所注册的被观察者的通知的时候，被观察者可以将其从所维护的组中删除。

在这里提及一下设计模式现有的定义很有必要。这个定义是与所使用的语言无关的。通过这个定义，最终我们可以更深层次地了解到设计模式如何使用以及其优势。
在四人帮的《设计模式:可重用的面向对象软件的元素》这本书中，是这样定义观察者模式的:

"一个或者更多的观察者对一个被观察者的状态感兴趣，将自身的这种兴趣通过附着自身的方式注册在被观察者身上。
当被观察者发生变化，而这种便可也是观察者所关心的，就会产生一个通知，这个通知将会被送出去，
最后将会调用每个观察者的更新方法。当观察者不在对被观察者的状态感兴趣的时候，它们只需要简单的将自身剥离即可。“
