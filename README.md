#PythonReview
>该文档整理Python的一些难点和易错点,以方便以后复习查找

>开始于2015年11月4日,计划2016年1月1日前完成

##1. Python的函数传参
---

    所有变量都可以理解为对内存中的一个对象的"引用".类型不是属于变量的,而是属于对象的.对象有两种:

> ####一种是不可变对象:
    如字符串,元组,数值
    
> ####一种是可变对象:
    如列表,字典
    Python函数传递参数时,函数自动复制一份引用,函数作用域外的引用便与复制后的引用没有关系了.当传递的是不可变对象的引用时,函数内部对它的操作不会影响内存中对象的值;但当传递的是可变对象的引用时,函数内部对它的操作就和定了位的指针一样,即在内存中修改了它的值.

##2. Python的三类方法
    Python有三类方法,即静态方法,类方法,实例方法
    
    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | col 3 is      | right-aligned | $1600 |
    | col 2 is      | centered      |   $12 |
    | zebra stripes | are neat      |    $1 |