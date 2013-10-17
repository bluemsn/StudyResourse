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


