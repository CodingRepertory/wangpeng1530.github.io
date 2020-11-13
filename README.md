# 存储自己的一些想法

## 客户端热更实现

    首先需要有版本信息确定当前版本需要更新什么东西，所以需要几个文件来存储需要更新的内容

    1、首先需要一个基础db文件来存储基础版本的所有数据，以此为基准来获取有哪些文件变更。
    2、需要一个版本db文件来确定哪些文件需要被下载。
    3、需要一个version文件来控制是否有新的版本db文件需要下载

## 热更流程

    1、玩家启动游戏后，会请求一个web地址，获取到version文件后，对比本地的version文件中的信息，这里有一个操作，包体中也有一个version.json文件，当外部存储目录找不到version文件时，加载包体内的version文件，当本地版本号低于服务器请求到的version版本信息后，此时需要请求服务器获取版本db文件。
    2、请求到db文件之后，加载下载列表db文件，加载已下载db文件，对比md5之后，以文件路径做key，缓存到表中，注册下载回调，开始下载，下载下来的文件和内存中缓存的db数据进行对比，如果下载成功，则需要将成功的文件写入已下载db文件中，标记已下载完成。

## Python相关

    ①Python和lua类似，变量不需要声明具体的类型
    ②python注释#开头，当语句以:结尾时，缩进语句视为代码块,缩进4空格，(自己设置tab空格替换)大小写敏感
    ③python中pass语句为空语句，什么都不做，占位

    exit()退出命令行模式

    能不能像.exe文件那样直接运行.py文件呢？在Windows上是不行的，但是，在Mac和Linux上是可以的，方法是在.py文件的第一行加上一个特殊的注释：

    #!/usr/bin/env python3

    print('hello, world')
    然后，通过命令给hello.py以执行权限：

    $ chmod a+x hello.py


    pass语句什么都不做，那有什么用？实际上pass可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。

    pass还可以用在其他语句里，比如：

    if age >= 18:
        pass
    缺少了pass，代码运行就会有语法错误。

    1、Python 两种运行方式:
        ①：命令行方式，会自动输出输入代码的结果
        ②：文件执行方式，具体为 python xxx.py，一次执行完所有代码
    2、数据类型和变量
            整数
                和数学写法一模一样，负整数之类的全部支持
                Python除法有两种，一种/，一种//，后者只取结果的整数部分,前者精确除
            浮点数
                可使用科学计数法替代 1.23*10⑨，10可用e代替1.23e9
            字符串
                字符串是使用'或者"括起来的任意文本,支持转义符,
                如字符串中很多字符需要转义，Python允许使用r' xxx'表示xxx中不转义，
             如字符串中需要换很多行， '''...'''表示输入多行内容(…并不是特殊字符只是用来代表内容，不要误解)
            布尔值
                Python中布尔值只有True和False两种值，支持and or not运算
            空值
                None是一个特殊的值，并不能理解为0，0有意义，但是None是一个特殊空值。
            变量
                Python本身动态语言，定义变量时并不需要指定具体的数据类型。
            常量
                Python中多使用全大写变量名来表示常量
    3、字符串和编码
        目前计算机编码格式统一使用Unicode，Unicode中存储了所有字符的编码，但是因为要兼容所有的编码格式，所以编码需要多个字节来存储（英语本来一个字节就可以，为了兼容其他字符，所以需要多个字节），这样存储和传输成本就上升了，所以，后推出了UTF-8编码格式，会自己适配编码成多个字节。。而且UTF-8还可以兼容之前的ASCII码.
                  

            Python提供了ord()来获取字符的编码，chr()可以将编码转为字符
    4、使用list和tuple
         list=[pa1,pa2,pa3,...]

         Python内置数据类型 List
         List是一种有序的集合, len()可以获取list元素个数。，list索引从0开始。
         当索引超出了范围时，Python会报一个IndexError错误，所以，要确保索引不能越界， 最后一个元素的索引是len(classmates) - 1。
         要取最后一个元素，除了计算索引位置外，还可以用-1做索引，直接获取最后一个元素，以此类推，-2，-3...

        len(List) 取list长度
        List.append('Adam')  追加元素    List.insert(1, 'Jack') 插入元素    List.pop(Index) 删除元素,Index元素位置 ,为空删除末尾。

                跟lua一样可以，可以直接通过键值获取列表数值，然后重新赋值。
                List里面的元素也可以是一个List，和lua很相似。
                list数据结构类似于Lua中的表。
        extend方法，使用一个list去扩充另一个list
                
        Tuple 定义
        t=("1","2","3")
        Tuple一旦初始化就不能更改，获取元素的方法类似于list
                t = (1)
                定义的不是tuple，是1这个数！这是因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是1。
        所以，只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：
    5、条件判断
             
        if elif else
         非零数值、非空字符串、非空list等，就判断为True，否则为False
    6、循环
        for x in ...     迭代list或者tuple
           while 循环
            break
            continue

    7、使用dict和set
        Dict {}
        Set ()
        List []
         dict(也叫map)
             要避免key不存在的错误，有两种办法：
        一是通过in判断key是否存在：
            二是通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value：
        d.get('Thomas', -1)
        如果不存在key，就会返回-1，未设置返回None

        dict特点：
        查找和插入的速度极快，不会随着key的增加而变慢；
        需要占用大量的内存，内存浪费多。
        list特点：
        查找和插入的时间随着元素的增加而增加；
        占用空间小，浪费内存很少。
        
        dict的key必须是不可变对象。

        Set  本身不存储Value，只存储了Key，本身需要list对象进行初始化，set是数学意义上的无序和无重复元素的集合，因此两个set可以进行数学上的集合运算，交并之类的操作。


        set和dict的唯一区别仅在于没有存储对应的value

    8、调用函数
        函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：
    9、定义函数
        在Python中，定义一个函数要使用def语句，依次写出函数名、括号、括号中的参数和冒号:，然后，在缩进块中编写函数体，函数的返回值用return语句返回。
        如果没有return语句，函数执行完毕后也会返回结果，只是结果为None。return None可以简写为return。
        
        对参数类型做检查，只允许整数和浮点数类型的参数。数据类型检查可以用内置函数isinstance()实现：
        def my_abs(x):
            if not isinstance(x, (int, float)):
                raise TypeError('bad operand type')
        Python函数多个返回值是一个tuple！但是，在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个tuple，但写起来更方便
        
        定义函数时，需要确定函数名和参数个数；
        
        如果有必要，可以先对参数的数据类型做检查；
        
        函数体内部可以用return随时返回函数结果；
        
        函数执行完毕也没有return语句时，自动return None。
        
        函数可以同时返回多个值，但其实就是一个tuple
    10、函数的参数
        三种类型
        
        位置参数
        可变参数
        关键字参数
        
        1、位置参数
        def power(x, n=2):
            s = 1
            while n > 0:
                n = n - 1
                s = s * x
            return s
        n=2就是定义了函数默认值，未传值的情况下，n等于2
        参数未传情况下会按照参数顺序依次赋值，如果不按照顺序传参，则需要带上参数名
        当不按顺序提供部分默认参数时，需要把参数名写上。比如调用enroll('Adam', 'M', city='Tianjin')，意思是，city参数用传进去的值，其他默认参数继续使用默认值。
        关键字参数必须在位置参数后面，因为python在解析函数时，是按照顺序来的，位置参数必须先满足， 才考虑其他可变参数。
        
        注意
        定义默认参数要牢记一点：默认参数必须指向不变对象！
        
        Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，
        如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。
        定义默认参数要牢记一点：默认参数必须指向不变对象！
            
        可变参数，即函数的参数列表不固定，长短不一
        
        2、可变参数
        函数参数需要多个时，没必要定义很多个参数，可以在参数前加*，意思是将传进来的所有参数组成一个tuple，减少很多函数参数声明。
        
        需要将list或者tuple整个传进函数当参数，可以在调用的时候给变量前加*，这样的意思是，将整个list整体传入函数。
        
        3、关键字参数
        可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。
        而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。
        
        
        def person(name, age, **kw):
            print('name:', name, 'age:', age, 'other:', kw)
        
        >>> person('Michael', 30)
        name: Michael age: 30 other: {}
        
        
        >>> person('Bob', 35, city='Beijing')
        name: Bob age: 35 other: {'city': 'Beijing'}
        >>> person('Adam', 45, gender='M', job='Engineer')
        name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
        
        
        >>> extra = {'city': 'Beijing', 'job': 'Engineer'}
        >>> person('Jack', 24, city=extra['city'], job=extra['job'])
        name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
        
        
        >>> extra = {'city': 'Beijing', 'job': 'Engineer'}
        >>> person('Jack', 24, **extra)
        name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
        
        **操作符，传入dict的拷贝
        
        递归函数 
        尾调用，尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

    11、Python切片方法
        Python提供了切片（Slice）操作符
        L[0:3]表示，从索引0开始取，直到索引3为止，但不包括索引3。即索引0，1，2，正好是3个元素。
        
        L[X:Y:Z] 三个参数分别代表 x开始索引 y结束索引 z 每z个元素取一次
        
        x,y,z默认为0
        
        字符串也可以看成是一个list，所以字符串也可以使用切片操作，所以Python中没有字符串切割函数

    12、Python迭代
        只要是可迭代对象，都可以进行迭代。是否是可迭代对象通过collections模块的Iterable类型判断：
        
        >>> from collections import Iterable
        >>> isinstance('abc', Iterable) # str是否可迭代
        True
        >>> isinstance([1,2,3], Iterable) # list是否可迭代
        True
        >>> isinstance(123, Iterable) # 整数是否可迭代
        False

        isinstance(data,type)这个函数可以判断变量的类型

        
        for in循环
        默认情况下，dict迭代的是key。如果要迭代value，可以用for value in d.values()，如果要同时迭代key和value，可以用for k, v in d.items()。

        如果要对list实现类似Java那样的下标循环怎么办？Python内置的enumerate函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身：

        >>> for i, value in enumerate(['A', 'B', 'C']):
        ...     print(i, value)
        ...
        0 A
        1 B
        2 C

    13、列表生成式
        写列表生成式时，把要生成的元素x * x放到前面，后面跟for循环，就可以把list创建出来，十分有用。
        >>> [x * x for x in range(1, 11)]
        >>> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

        for循环后面还可以加上if判断，这样我们就可以筛选出仅偶数的平方：
        >>> [x * x for x in range(1, 11) if x % 2 == 0]
        [4, 16, 36, 64, 100]

        还可以使用两层循环，可以生成全排列：
        >>> [m + n for m in 'ABC' for n in 'XYZ']
        ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

        在一个列表生成式中，for前面的if ... else是表达式，而for后面的if是过滤条件，不能带else。
    14、生成器
        如果列表元素可以按照某种算法推算出来，这样就不必创建完整的list，从而节省大量的空间。
        在Python中，这种一边循环一边计算的机制，称为生成器：generator。
        >>> L = [x * x for x in range(10)]
        >>> L
        [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
        >>> g = (x * x for x in range(10))
        >>> g
        <generator object <genexpr> at 0x1022ef630>
        创建L和g的区别仅在于最外层的[]和()，L是一个list，而g是一个generator。
        
        from collections.abc import Iterable

        g = (x * x for x in range(10))
        print(type(g),isinstance(g,Iterable))

        <class 'generator'> True

        generator是可遍历的变量，也可以通过next(g)来获取下一个元素，直到没有元素后，抛出StopIteration错误。
    15、list、dict、str虽然是Iterable(可迭代对象)，却不是Iterator(迭代器)。
        把list、dict、str等Iterable(可迭代对象)变成Iterator(迭代器)可以使用iter()函数
    16、面向对象编程
        Python类关键字Class，拥有成员变量，默认构造函数__init__
        def __init__(self,args)
        Python和lua类似，可以直接点语法给对象添加成员变量，或者修改成员变量的值
        但如果给成员变量头部加上__就表明这个变量本身为私有变量，只能通过提供的接口进行修改和访问。

        >>> bart = Student('Bart Simpson', 59)
        >>> bart.get_name()
        'Bart Simpson'
        >>> bart.__name = 'New Name' # 设置__name变量！
        >>> bart.__name
        'New Name'

        这样看貌似是修改成功了，但是其实是给bart添加了一个__name变量，本身的__name变量已经被存储为了_XXX__name__。

        如果要获得一个对象的所有属性和方法，可以使用dir()函数，它返回一个包含字符串的list

        类似__xxx__的属性和方法在Python中都是有特殊用途的，比如__len__方法返回长度。在Python中，如果你调用len()函数试图获取一个对象的长度，实际上，在len()函数内部，它自动去调用该对象的__len__()方法，所以，下面的代码是等价的：

        >>> len('ABC')
        3
        >>> 'ABC'.__len__()
        3

        getattr()、setattr()以及hasattr()三个方法可以对对象进行修改

        getattr(obj,key) 
        
        ###类成员变量
        >>> class Student(object):
        ... name = 'Student'
        ...
        >>> s = Student() # 创建实例s
        >>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
        Student
        >>> print(Student.name) # 打印类的name属性
        Student
        >>> s.name = 'Michael' # 给实例绑定name属性
        >>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
        Michael
        >>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
        Student
        >>> del s.name # 如果删除实例的name属性
        >>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
        Student

    ##正则表达式
    一直没有彻底掌握正则表达式，通过这次，必须熟练掌握。
    \d 匹配一个数字
    \w 匹配一个字母或数字
    .可以匹配任意字符
    要匹配变长的字符，在正则表达式中，用*表示任意个字符（包括0个），用+表示至少一个字符，用?表示0个或1个字符，
    用{n}表示n个字符，用{n,m}表示n-m个字符：
