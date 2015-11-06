#PythonReview
>* 该文档整理Python的一些难点和易错点,以方便以后复习查找
>* 开始于2015年11月4日,计划2016年1月1日前完成

# Python的函数传参

    所有变量都可以理解为对内存中的一个对象的"引用".类型不是属于变量的,而是属于对象的.对象有两种:
#### 一种是不可变对象:
    如字符串,元组,数值
#### 一种是可变对象:
    如列表,字典
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
    __name__:一种约定,Python内部命名,用以区别用户命名
    _sex:一种约定,用来指定私有变量的方式
    __foo:这种写法的实际意义在于,解释器会用_classname__foo来代替这个名字,以防止它覆盖了其他类里相同的命名
    
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
    
# 面向切面编程和装饰器
#### AOP面向切面编程
> 即将众多方法中的所有共有代码全部抽取出来,放到某个地方集中管理,然后在具体运行时,再由容器动态织入这些共有代码.
> AOP有两个好处,首先程序员在编写时只需关心核心业务逻辑;其次,日后需求变动或维护代码时更加简单轻松.
> 这个帖子分析的不错.[面向切面编程](http://blog.csdn.net/Intlgj/article/details/5671248).
    