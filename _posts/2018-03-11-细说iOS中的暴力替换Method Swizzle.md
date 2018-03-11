---
layout: post
title: "细说iOS中的暴力方法替换，Method Swizzle"
date: 2018-03-11
description: "细说iOS中的暴力方法替换，Method Swizzle"
tag: iOS
---


## 细说iOS中的暴力方法替换，Method Swizzle[参考博文](http://www.cocoachina.com/ios/20160121/15076.html)

今天突然遇到了一个问题，在开发的过程中，如果遇到有些实现逻辑并不是我想要的怎么办？这个时候第一想到的有以下几种做法。

* 手动添加

	直接简单粗暴的在每个控制器中加入统计，复制、粘贴、复制、粘贴...
	上面这种方法太Low了，消耗时间而且以后非常难以维护，会让后面的开发人员骂死的。

* 继承

	我们可以使用OOP的特性之一，继承的方式来解决这个问题。创建一个基类，在这个基类中添加统计方法，其他类都继承自这个基类。

	然而，这种方式修改还是很大，而且定制性很差。以后有新人加入之后，都要嘱咐其继承自这个基类，所以这种方式并不可取。

* Category

	我们可以为UIViewController建一个Category，然后在所有控制器中引入这个Category。当然我们也可以添加一个PCH文件，然后将这个Category添加到PCH文件中。

	我们创建一个Category来覆盖系统方法，系统会优先调用Category中的代码，然后在调用原类中的代码。
	
### Method Swizzling
但是上面这些方法都有点麻烦，这个时候就有了runtime中的黑魔法Method Swizzling，Method Swizzling本质上就是对IMP和SEL进行交换。

* 核心
在实现 Method Swizzling 时，核心代码主要就是runtime的c语言api：

	```
	OBJC_EXPORT void method_exchangeImplementations(Method m1, Method m2) 
	 __OSX_AVAILABLE_STARTING(__MAC_10_5, __IPHONE_2_0);
	```
* 实现思路
	我们先给UIViewController添加一个Category，然后在Category中的+(void)load方法中添加Method Swizzling方法，我们用来替换的方法也写在这个Category中。由于load类方法是程序运行时这个类被加载到内存中就调用的一个方法，执行比较早，并且不需要我们手动调用。而且这个方法具有唯一性，也就是只会被调用一次，不用担心资源抢夺的问题。

	定义Method Swizzling中我们自定义的方法时，需要注意尽量加前缀，以防止和其他地方命名冲突，Method Swizzling的替换方法命名一定要是唯一的，至少在被替换的类中必须是唯一的。




