---
layout: post
title: "细说oc中的load和initialize方法"
date: 2018-03-11
description: "细说oc中的load和initialize方法"
tag: iOS
---

## 细说oc中的load和initialize方法[参考博文](http://blog.csdn.net/lqq200912408/article/details/50779249)

oc中有两个特殊的类方法。这里细说一下两个方法的区别和联系。

### load

* load方法在这个文件被装载的时候调用，只要在你的build Phases->Complie Sources 中出现的文件总会被装载，这与这个类是否呗用到无关，因此load方法总是在main函数之前调用。

* 如果这个类实现了load方法，在调用前会首先调用父类的load方法，这个过程是自动完成，并不需要我们手动实现：

	>ps: 如果一个类没有实现load方法，那么就不会调用他的父类load方法，这一点与正常的类继承和方法调用不一样，需要额外注意一下。

* load方法调用时，系统处于脆弱状态，如果调用别的类方法，且该方法依赖于这个类的load方法，进行初始化设置，那么必须确保这个类的load方法已经调用了，

* 在compile source 中，文件的排放顺序就是其转载的顺序，自然也就是load方法的调用顺序，这一点证明了load方法中会自动调用父类的方法。
	>ps: 永远不要依赖这个顺序来实现你的代码逻辑，一方面：这个在后期的开发中极其容易导致错误，另一方面：你实际上并不需要这么做。所以！一般来说，除了Method Swizzle ，其他的逻辑不应该放在load方法中实现。
	
### initialize
* 这个方法在第一次给某个类发送消息时调用（比如实例化一个对象），并且只会调用一次。initialize方法实际上时一种惰性调用，也就是说如果一个类没有呗用到，那么它的initialize方法也不会被调用，这一点可以有效利用资源。

* 	与load方法类似的是，在initialize方法内部也会调用父类的方法，而且不需要我们显示的写出来。与load方法不同之处在于，即使子类没有实现initialize方法，也会调用父类的方法，这会导致一个很严重的问题：

*  在创建子类对象时，首先要创建父类对象，所以会调用一次父类的initialize方法，然后创建子类时，尽管自己没有实现initialize方法，但还是会调用到父类的方法。正确使用initialize方法的姿势如下：

```
+ (void)initialize {  
    if (self == [Parent class]) {  
        NSLog(@"Initialize Parent, caller Class %@", [self class]);  
    }  
} 
```

### 总结
* load和initialize方法都会在实例化对象之前调用，以main函数为分水岭，前者在main函数之前调用，后者在之后调用。这两个方法会被自动调用，不能手动调用它们。

* load和initialize方法都不用显示的调用父类的方法而是自动调用，即使子类没有initialize方法也会调用父类的方法，而load方法则不会调用父类。

* load方法通常用来进行Method Swizzle，initialize方法一般用于初始化全局变量或静态变量。

* load和initialize方法内部使用了锁，因此它们是线程安全的。实现时要尽可能保持简单，避免阻塞线程，不要再使用锁。


	

