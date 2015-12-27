title: PythonReview
date: 2015-11-05 00:08:07
categories: web开发
tags: Python
---

>* 该文档整理Python的一些难点和易错点,以方便以后复习查找
>* 开始于2015年11月4日,计划2016年1月1日前完成

# Python的函数传参
所有变量都可以理解为对内存中的一个对象的"引用".类型不是属于变量的,而是属于对象的.对象有两种:
#### 一种是不可变对象:
> 如字符串,元组,数值

#### 一种是可变对象:
> 如列表,字典

Python函数传递参数时,函数自动复制一份引用,函数作用域外的引用便与复制后的引用没有关系了.当传递的是不可变对象的引用时,函数内部对它的操作不会影响内存中对象的值;但当传递的是可变对象的引用时,函数内部对它的操作就和定了位的指针一样,即在内存中修改了它的值.

# Python的三类方法
#### Python有三类方法,即静态方法,类方法,实例方法
    |    \   |   实例调用   |    类调用    |  
    |静态方法--|   yes      |     yes     |
    |类方法   |    yes      |    yes      |
    |实例方法  |   yes      |     no      |

# Python自省
非常强大的特性,即面向对象语言所写程序在运行时,检查对象的能力;

常用的诸如 dir(), getattr(), setattr(), hasattr(), isinstance()等

# Python列表,集合,字典推导式
    了解的比较多的是列表推导式,但其实从2.7开始,就可以用同样的语法创建集合和字典了
        >>> my_list = [1,2,3,2,4,6,3,7]
        >>> my_set = {x for x in my_list if x%2 == 0}
        >>> my_set
        set([2,4,6])
        
        >>> my_dict = {x:x%2 == 0 for x in range(1, 10) }
        >>> my_dict
        {1: False, 2: True, 3: False, 4: True, 5: False, 6: True, 7: False, 8: True, 9: False}

# Python中的单下划线与双下划线
\__name__:一种约定,Python内部命名,用以区别用户命名

\_sex:一种约定,用来指定私有变量的方式

\__foo":这种写法的实际意义在于,解释器会用\_classname__foo来代替这个名字,以防止它覆盖了其他类里相同的命名
    
# Python字符串格式化: %和format
#### %够用.但不能同时传递变量和元组
    >>> "kkkkk%s" % (1,2)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: not all arguments converted during string formatting
    解决办法:
    >>> "kkkkk%s" % ((1,2),)
    'kkkkk(1, 2)'
#### 更好的选择,format
    "kkkkk{0}".format((1,2))
    'kkkkk(1, 2)'

# 生成器与迭代器
#### 首先,迭代器对象.
        >>> myiterator = [x*x for x in range(1,4)]
        >>> for i in myiterator:
        ...     print i
        ... 
        1
        4
        9
可迭代对象必须先放到内存中,然后才可随意调用.但如果它们数据量过大,就会消耗太多的内存.
#### 生成器对象是一种特殊的迭代器对象.
生成器是一个带yield关键字的函数,这里说的是生成器对象.
创建生成器对象时用()代替[],

        >>> mygenerator = (x*x for x in range(1,4))
        >>> for i in mygenerator:
        ...     print i
        ... 
        1
        4
        9
    其次,生成器对象只可迭代一次,不可重复调用
#### yield关键字
    其用法和return类似
        >>> def createGenerator():
        ...     my_list = range(3)
        ...     for i in my_list:
        ...         yield i*i
        ... 
        >>> myGenerator = createGenerator()
        >>> print myGenerator
        <generator object createGenerator at 0x7fec5da8dbe0>
        >>> for i in myGenerator:
        ...     print i
        ... 
        0
        1
        4

调用createGenerator函数时,函数内部的代码并没有运行,函数仅仅返回了一个生成器对象.

当for循环第一次调用生成器对象myGenerator时,createGenerator函数里的代码开始执行,直到遇到yield关键字,返回本次循环的返回值.

以后,每次循环返回一个该循环的返回值,直到没有返回值.

更多参考:[Python yield 使用浅析](http://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/)
    
# 面向切面编程和装饰器
#### AOP面向切面编程
即将众多方法中的所有共有代码全部抽取出来,放到某个地方集中管理,然后在具体运行时,再由容器动态织入这些共有代码.
AOP有两个好处,首先程序员在编写时只需关心核心业务逻辑;其次,日后需求变动或维护代码时更加简单轻松.
这个[帖子](http://blog.csdn.net/Intlgj/article/details/5671248)分析的不错.

#### 装饰器
装饰器是一个非常著名的设计模式,经常被用到有切面需求的场景,常见的有插入日志,性能测试,事务处理等.

装饰器是解决这类问题的绝佳设计,有了装饰器,就可以抽离出大量函数中与函数核心功能无关的公共代码以实现重用.

总结来说,装饰器就是把其他函数作为参数的函数,作用就是**为已经存在的对象添加额外的功能**.
    
以下代码是对装饰器的简单应用,包括把函数的参数传递给装饰器&&装饰器传递自己的参数

    login = True
    
    def checkLogin(gender):
    	def my_decorator(func):
    		print "the para of the decorator is " + gender
    		def wrapper(para):
    			print "the para of the function is " + para
    			if login == True:
    				return func(para)
    			else:
    				print "you haven't login"
    		return wrapper
    	return my_decorator
    
    @checkLogin("man")
    def getMessage(para):
    	print para
    
    getMessage("yangbo")
    
另外,Python有几个自带的装饰器,比如property, staticmethod, classmethod等.更多关于Python装饰器,可以参考[这里](http://stackoverflow.com/questions/739654/how-can-i-make-a-chain-of-function-decorators-in-python)

# Duck Typing, 鸭子类型
> 如果行为像鸭子,那么不管它是什么,就把它看做鸭子.根本不考虑一个对象属于什么类,只关心它有什么样的行为(它有哪些方法),这就是Duck Typing.
鸭子类型是动态语言的一种风格.提出这个概念的是大名鼎鼎的专家程序员Dave Thomas.  --*松本行弘的程序世界*

这篇博文是对鸭子类型的思考,可以参考[鸭子类型:一切都是为了复用](http://blog.csdn.net/xiammy/article/details/1457135)

# Python函数重载
函数重载主要解决的两个问题.

1. 可变参数类型.
2. 可变参数个数.

另外,有一个基本的设计原则,仅仅当两个函数除了参数类型和参数个数不同以外,其功能完全相同时,才使用函数重载,如果函数功能不同,则不应该使用重载,而是写一个新的函数.

对于情况 1, 参数类型不同.由于Python的函数可以接受任何类型的参数,如果函数的功能相同,不同参数类型在Python中很可能对应相同的代码,所以没有必要区别对待.

对于情况 2, 参数个数不同.处理方法是 默认参数,假设函数功能相同,默认的参数一定会用到.

综上,Python不需要函数重载.

# 新式类与旧式类
Python2.2引入descriptor,基于这个功能实现了新式类的对象模型,同时解决了经典类系统中出现的多重继承的MRO(Method Resolution Order)的问题.

更多关于MRO.[MRO&super](http://blog.csdn.net/seizef/article/details/5310107)

# \__new__和\__init__的区别

1. \__new__是一个静态方法, 而\__init__是一个实例方法.
2. \__new__会返回一个创建的实例,而\__init__什么都不返回.
3. 只有在\__new__返回一个cls的实例时,后面的\__init__才能被调用.
4. 当创建一个新实例时调用\__new__方法, 初始化实例时调用\__init__.

# 元类
> “元类就是深度的魔法，99%的用户应该根本不必为此操心。如果你想搞清楚究竟是否需要用到元类，那么你就不需要它。那些实际用到元类的人都非常清楚地知道他们需要做什么，而且根本不需要解释为什么要用元类。”  —— Python界的领袖 Tim Peters

元类很复杂,但还是先做一点简单的了解.
在Python中,可以用type函数动态创建类,其实,所有类的创建,都是通过扫描class关键字下的语法,背后的都是由type函数实现的.
       
    def getName(self):
        print self.name
        
    My_class = type('My_class', (), {'name':'yangbo', 'getName':getName})
    
    my_instance = My_class()
    my_instance.getName()   #yangbo
    
Python中,一切都是对象,类也是对象,元类就是用来创建类的"类",type就是Python的内建元类.

#### \__metaclass__属性
可以在写一个类时添加一个\__metaclass__属性,
    
    class My_class():
        __metaclass__ = something...

此时,类对象My_class还没有在内存中创建,运行这段代码时的过程时,Python解释器会在类的定义中寻找\_\_metaclass__属性,
如果找到了,Python解释器就会用它来创建类My_class,如果没有找到,它会继续在父类中寻找,并尝试之前的操作,如果在所有父类中都无法找到\__metaclass__属性,
它就会在模块层次中去寻找,并尝试同样的操作,如果还是找不到,它就会用内建元类type来创建这个类对象.

所以,我们在\__metaclass__中就可以放置能创建一个类的东西.什么是可以创建一个类的东西?type,或者任何用到type或者子类化type的东西,都可以.

使用到元类的代码比较复杂,但就元类本身而言,其实是很简单的:
    
    1. 拦截类的创建.
    2. 修改类.
    3. 返回修改之后的类.
    
参考:

1. [stack overflow](http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python)

2. [中文版](http://blog.jobbole.com/21351/)

# 单例模式
##### 使用\__new__方法

    class Singleton(object):
    	def __new__(cls, *args, **kw):
    		if not '_instance' in cls.__dict__:
    			cls._instance = object.__new__(cls)
    		return cls._instance
    
    class My_class(Singleton):
    	a = 1
    
    
    one = My_class()
    two = My_class()
    
    two.a = 9
    print one.a #9
    print one is two #True

通过\__new__方法,将一个类的实例绑定到\_instance"属性.
如果cls.\_instance为None,说明类还未实例化,故实例化并将其绑定到cls.\_instance,
以后每次实例化的时候都返回第一次实例化创建的实例.注:从Singleton派生子类时不要重载\__new__方法.

##### 共享属性
所谓单例就是所有引用(实例,对象)拥有相同的状态(属性)和行为(方法).

同一个类的所有实例必然拥有相同的行为,故只需保证它们拥有相同的状态(属性).

所有实例共享属性的最简单最直接的方法就是\__dict__属性指向同一个字典
    
    class Borg(object):
        	__state = {}
        	def __init__(self):
        	    self.__dict__ = self.__state
    
    one = Borg()
    two = Borg()
    
    print one is two #False
    print one.__dict__ is two.__dict__ #True

参考http://code.activestate.com/recipes/66531/

##### 使用装饰器(decorator)
这是一种更Pythonic,更elegant的方法

单例类不知道自己是单例的,因为它本身并不是单例的

    def Singleton(cls, *args, **kw):
    	_instances = {}
    	def _singleton():
    		if cls not in _instances:
    			_instances[cls] = cls(*args, **kw)
    		return _instances[cls]
    	return _singleton
    
    @Singleton
    class My_class(object):
    	a = 1
    	def __init__(self, x=0):
    		self.x = x
    
    one = My_class()
    two = My_class()
    
    one.a = 2
    print two.a #2

##### import方法
Python中的模块module在程序中只被加载一次,它是天然的单例模式.所以可以直接写一个模块,
将需要的方法和属性写在模块中当做函数和模块作用域的全局变量即可.

我们在写[未来网](http://www.future.org.cn),[珞青网](http://young.future.org.cn)以及[E刊网](http://ebook.future.org.cn)时,都是这么做的.所有的配置信息写在config.py中,在使用的模块中导入它即可.

参考:http://blog.csdn.net/ghostfromheaven/article/details/7671853

# Python中的作用域
Python是静态作用域语言，尽管它自身是一个动态语言。也就是说，在Python中,变量的作用域是由它在源代码中的位置决定的.
实际上,只有模块，类以及函数才会引入新的作用域，其它的代码块是不会引入新的作用域的。

Python2.0及以前版本只有三中作用域,在2.1中引入了嵌套作用域,相应的变量查找顺序变为LEGB(即Local, Enclosing, Global, Built-in)

参考:http://www.cnblogs.com/frydsh/archive/2012/08/12/2602100.html

# GIL全局解释器锁
Global Interpreter Lock, 即Python为了保证线程安全而采取的独立线程运行的限制,也就是说一个CPU核心同一时间只能运行一个线程.
关于GIL的详细介绍,戳[Python 最难的问题](http://www.jeffknupp.com/blog/2012/03/31/pythons-hardest-problem/)

[中文版](http://www.oschina.net/translate/pythons-hardest-problem)

详细请参考[这里](http://blog.csdn.net/I2Cbus/article/category/2224167)

# 协程(Coroutine)
参考:[廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868328689835ecd883d910145dfa8227b539725e5ed000)

并发编程目前有四种方式(多进程,多线程,异步,协程)

协程的特点在于是一个线程执行，那和多线程比，协程有何优势？

最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。

第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。

因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。

Python对协程的支持还非常有限，用在generator中的yield可以一定程度上实现协程。虽然支持不完全，但已经可以发挥相当大的威力了。
传统的生产者-消费者模型是一个线程写消息，一个线程取消息，通过锁机制控制队列和等待，但一不小心就可能死锁。

    import time
    
    def consumer():
        r = ''
        while True:
            n = yield r
            if not n:
                return
            print('[CONSUMER] Consuming %s...' % n)
            time.sleep(1)
            r = '200 OK'
    
    def produce(c):
        c.next()
        n = 0
        while n < 5:
            n = n + 1
            print('[PRODUCER] Producing %s...' % n)
            r = c.send(n)
            print('[PRODUCER] Consumer return: %s' % r)
        c.close()
    
    if __name__=='__main__':
        c = consumer()
        produce(c)

注意到consumer函数是一个generator（生成器），把一个consumer传入produce后：
      
首先调用c.next()启动生成器；
      
然后，一旦生产了东西，通过c.send(n)切换到consumer执行；
      
consumer通过yield拿到消息，处理，又通过yield把结果传回；
      
produce拿到consumer处理的结果，继续生产下一条消息；
      
produce决定不生产了，通过c.close()关闭consumer，整个过程结束。
      
整个流程无锁，由一个线程执行，produce和consumer协作完成任务，所以称为“协程”，而非线程的抢占式多任务。
      
最后套用Donald Knuth的一句话总结协程的特点: *“子程序就是协程的一种特例。”*

# 闭包
```
def lazy_sum(*args):
    def sum():
        total = 0
        for i in args:
            total = total + i
        return total
    return sum

>>> f = lazy_sum(1, 2, 3, 4)
>>> f
<function sum at 0x7f3ea07c95f0>
>>> f()
10
```
在函数lazy_sum中定义了函数sum，并且，内部函数sum可以引用外部函数lazy_sum的参数和局部变量，调用函数lazy_sum时，返回函数sum，相关参数和变量都保存在返回的函数中，调用f时，函数才真正执行。

这样的定义在lazy_sum中的sum函数就是闭包(Closure)。

# lambda函数
就相当于单行、没有return的匿名函数，常在需要一个函数，而又不想去命名一个函数的场合下使用。

# Python函数式编程
函数式编程的三大特性：
    
    1.不可变数据，像闭包一样，默认上变量不可变，若要修改，需要把变量copy出去修改（不是很明白这句话）
    2.first class functions，函数可以当成变量使用
    3.尾递归优化，每次递归都会重用stack（貌似Python不支持）
    
结合上一条的lambda表达式
```py
map(lambda a:a*a, [a for a in range(5)]) 
# [0, 1, 4, 9, 16]

filter(lambda a:'f' in a, ['fd', 'ff', 'gg'])
# ['fd', 'ff'] 遍历调用函数，返回使函数返回值为True的项

reduce(lambda a, b:a*b, range(1,5)) 
# 24 对每一个项迭代调用函数

sorted(['sd','ass','mss'], cmp_ig_small_case)
# 传入自定义函数进行排序
def cmp_ig_small_case(x, y):
    x1 = x.upper()
    y1 = y.upper()
    if x1>y1:
        return 1
    if x1<y1:
        return -1
    return 0
```
# Python里的拷贝
对象赋值只是简单的对象引用，当创建了一个对象，并将它赋值给另外一个变量时，并没有进行对象的拷贝，只是拷贝了这个对象的引用。

而浅拷贝则是新建了一个类型跟原对象相同的对象，其内容是对原对象元素的引用，对于不可变对象，如字符串，则是显式拷贝，并新建一个字符串对象。

浅拷贝的方式：

    1.完全切片[:]
    2.利用list(),dict()等工厂函数
    3.使用copy模块的copy函数

所谓深拷贝，即为创建一个新的容器对象，包含原对象元素全新拷贝的引用。使用copy模块的deepcopy函数。

# Python垃圾回收机制
