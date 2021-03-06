Python
==========================

.. contents::
  :local:
  :backlinks: top

面向对象
-------------------------

Python 既可以面向过程编程, 也可以面向对象编程. 面向对象编程可以实现面向过程无法实现的功能.
所以应该偏向于利用面向对象的思想. 

Python 中有两种类, **经典类** 与 **新式类**. 新式类比经典类的属性和用法都更丰富. **新式类** 在 Python2.2 版本引入.

.. attention:: Python2 中, 显式继承 ``object`` 的都是新式类, 否则是经典类. Python3 中全部是新式类.

封装
'''''''''''''''''''''

将属性或方法封装在类或着对象中, 通过类或对象访问.

.. image:: https://s2.ax1x.com/2019/05/10/EW3DGq.md.png
  :scale: 30 %
  :alt: 类与对象

字段 Attribute
""""""""""""""""""""""

普通 Attr
  属于对象; 只能由对象访问. 定义时使用 ``self`` 在方法内定义.

.. code-block:: python

  class Foo:
    def __init__(self)
      self.name = 'Foo'
  
静态 Attr
  属于类; 静态 Attr 可以由类访问, 也可以由对象访问. 在方法外定义.

.. code-block:: python

  class Foo:
    name = 'Foo'

.. hint:: 普通 Attr 在每个对象中保存一份, 静态 Attr 只有在类中保存一份.

方法
""""""""""""""""""""""

普通方法
  只能通过对象调用, 至少有一个 ``self`` 参数. 调用普通方法时, 自动将调用该方法的对象赋值给 ``self`` 参数.

.. code-block:: python

  def func(self, paramlist):
    pass

类方法
  可以通过类或对象调用, 至少有一个 ``cls`` 参数, 调用类方法时, 自动将调用该方法的类赋值给 ``cls`` 参数.

.. code-block:: python

  @classmethod
  def func(cls, paramlist):
    pass

静态方法
  通过类或对象调用, 无必须参数.

.. code-block:: python

  @staticmethod
  def funcname(parameter_list):
    pass

.. hint:: 所有的方法都只在内存中保存一份, 只不过根据调用的对象不同, 传入的参数不同.

属性
""""""""""""""""""""""

属性类似方法的变种, 在定义时通过 **方法** 定义, 调用时像 **字段** 一样调用.

定义属性有两种方法:

1. 通过装饰器.
2. 通过静态 Attr 定义 property 对象.

通过装饰器, 经典类中有一个装饰器::

  @property
  def prop(self):
    return self.__prop

调用时, 自动执行对应方法, 并返回值::

  res = obj.prop

新式类中增加了两个装饰器, 分别在对属性赋值和删除时::

  @prop.setter
  def prop(self, v):
    pass
  
  @prop.deleter
  def prop(self):
    pass

.. hint:: 赋值时会将值传递给 @prop.setter 修饰的方法的参数.


通过静态 Attr 初始化property对象. property的构造方法中有个四个参数:

- 方法, 调用时触发; 对应 @property
- 方法, 赋值时触发; 对应 @prop.setter
- 方法, 删除时触发; 对应 @prop.deleter
- 字符串, 设置 ``obj.prop.__doc__`` , 对应属性描述

访问控制
"""""""""""""""""""""

普通 Attr
  可以在外部随意访问或着修改.

保护 Attr
  一个 ``_`` 开头的 ``_Attr``; 保护类型, 约定俗成最好不要从外部访问, 对象内部和子类可以访问. 
  (无法通过 ``from some import *`` 导入)

私有 Attr
  两个 ``_`` 开头的 ``__Attr``; 私有类型, 只有对象内部可以访问. 
  (可以通过 ``obj._cls__attr`` 强制访问)

.. hint:: 访问或修改私有 Attr 可以通过设置 property 的方式提供接口.

特殊 Attr
  特殊 ``__Attr__`` 可以从外部访问. Python 内置属性, 特殊用途.

继承
''''''''''''''''''''''

.. sidebar:: 名称

  父类与子类也叫基类与派生类.

通过继承可以使得子类继承父类的功能与属性, 使得代码易于管理与扩展.

Python 可以实现多继承, 即继承多个类. 当继承多个类时, 调用时有两种搜索方式, 一种时 **深度优先** , 一种是 **广度优先** .

- 当类是经典类时, 按照深度优先.
- 当类是新式类时, 按照广度优先.

.. attention:: Python3 总是建立新式类, 所以总是采用 **广度优先**.

多态
'''''''''''''''''''''

强类型
"""""""""""""""""""""

在强类型语言中, 如 JAVA C++ C# 等, 通过子类实现 **覆盖** 父类已有的方法; 
调用时, **通过父类** 对象可以实现一个通用方法, 当参数是不同的子类时实现不同的功能.

所以, 强类型语言是通过继承与覆盖来实现多态的. 也可以不使用覆盖, 例如在各个子类中定义父类没有的相同方法.

强类型语言: `c++ 多态`_

.. _c++ 多态: https://znote.readthedocs.io/zh/latest/program/cpp/basic_cpp.html#id1

弱类型
"""""""""""""""""""""

Python 是弱类型语言, 也叫做 "鸭子类型". 它并不要求严格的继承体系, 一个对象只要 "看起来像鸭子,走起路来像鸭子" 那它就可以被看做是鸭子.

  Python的 "file-like object" 就是一种鸭子类型. 对真正的文件对象, 它有一个 ``read()`` 方法, 返回其内容.
  但是, 许多对象, 只要有read()方法, 都被视为 "file-like object". 许多函数接收的参数就是 "file-like object",
  你不一定要传入真正的文件对象, 完全可以传入任何实现了 ``read()`` 方法的对象.

.. hint:: Python这种弱类型动态语言, 定义方法时不需要指定参数类型, 即使不通过父类也可以实现应用于多种类的方法. 即不通过继承, 也可以实现多态.

元类
'''''

动态语言和静态语言最大的不同, 就是函数和类的定义, 不是编译时定义的, 而是运行时动态创建的.

比方说我们要定义一个Hello的class, 就写一个hello.py模块::

  class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)

当Python解释器载入hello模块时, 就会依次执行该模块的所有语句, 执行结果就是动态创建出一个Hello的class对象.

``type()`` 函数除了返回对象的类型, 还可以创建新的类. 例如通过 ``type()`` 函数创建 ``Hello`` 类::

  def fn(self, name='world'): # 先定义函数
    print('Hello, %s.' % name)
  
  Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
  h = Hello()
  h.hello()

除了使用 ``type()`` 动态创建类以外, 要控制类的创建行为, 还可以使用 ``metaclass``.

MetaClass 元类
  类相当于元类的对象, 先创建元类, 再根据元类实例化类.

按照默认习惯, ``metaclass`` 的类名总是以 ``Metaclass`` 结尾, 以便清楚地表示这是一个 ``metaclass``::

  # metaclass是类的模板，所以必须从`type`类型派生：
  class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
      attrs['add'] = lambda self, value: self.append(value)
      return type.__new__(cls, name, bases, attrs)

有了 ``ListMetaclass``, 我们在定义类的时候还要指示使用 ``ListMetaclass`` 来定制类, 传入关键字参数 ``metaclass``::

  class MyList(list, metaclass=ListMetaclass):
    pass

``__new__()`` 方法接收到的参数依次是:

- 当前准备创建的类的对象;
- 类的名字;
- 类继承的父类集合;
- 类的方法集合;

特殊成员与方法
''''''''''''''''''''''

Python 的类有一些内置的特殊成员与方法, 也叫 **魔术方法** .

特殊成员 Attr
"""""""

- 对于函数对象:

+-----------------+------+--------------------------------------+
| 特殊成员        | 权限 | 解释                                 |
+=================+======+======================================+
| __doc__         | 读写 | 函数的描述                           |
+-----------------+------+--------------------------------------+
| __name__        | 读写 | 函数的名称                           |
+-----------------+------+--------------------------------------+
| __qualname__    | 读写 | 函数的全名 (`New in 3.3`_)           |
+-----------------+------+--------------------------------------+
| __module__      | 读写 | 函数所属的 module                    |
+-----------------+------+--------------------------------------+
| __defaults__    | 读写 | 一个包含所有默认参数值的 tuple       |
+-----------------+------+--------------------------------------+
| __code__        | 读写 | 已编译函数体的 code 对象             |
+-----------------+------+--------------------------------------+
| __globals__     | 只读 | 函数可用的所有全局变量的字典         |
+-----------------+------+--------------------------------------+
| __dict__        | 读写 | 自定义函数 Attr 的字典 `PEP 232`_    |
+-----------------+------+--------------------------------------+
| __closure__     | 只读 | 闭包, 返回外层函数的变量cell         |
+-----------------+------+--------------------------------------+
| __annotations__ | 读写 | 包含函数参数的注解的字典 `PEP 3107`_ |
+-----------------+------+--------------------------------------+
| __kwdefaults__  | 读写 | 包含关键字默认参数的字典             |
+-----------------+------+--------------------------------------+

.. attention:: Python函数也是对象, 类也是对象, Python中的一切都是对象.

.. _PEP 232: https://www.python.org/dev/peps/pep-0232/
.. _PEP 3107: https://www.python.org/dev/peps/pep-3107/
.. _New in 3.3: https://docs.python.org/3/glossary.html#term-qualified-name

- 对于类

+---------------------+---------------------------------+
| 特殊成员            | 解释                            |
+---------------------+---------------------------------+
| __doc__             | 类的描述                        |
+---------------------+---------------------------------+
| __name__            | 类的名称                        |
+---------------------+---------------------------------+
| __qualname__        | 类的全名 (`New in 3.3`_)        |
+---------------------+---------------------------------+
| __module__          | 类所属的 module                 |
+---------------------+---------------------------------+
| __base__            | 类的父类                        |
+---------------------+---------------------------------+
| __bases__           | 类的所有父类的 tuple            |
+---------------------+---------------------------------+
| __mro__ [1]_        | 方法的调用搜索路径 tuple        |
+---------------------+---------------------------------+
| __abstractmethods__ | 一个包含抽象方法的set(仅抽象类) |
+---------------------+---------------------------------+
| __class__           | 所属的类, 一般类属于type        |
+---------------------+---------------------------------+
| __dict__            | 返回类的静态Attr 及实现的方法   |
+---------------------+---------------------------------+

.. [1] 对应的还有 ``mro()`` 方法, 返回的是 list 类型.

.. tip:: 可以使用 ``vars(obj)`` 方法获取一个对象的 ``__dict__``.

- 对于对象

+------------+--------------------------+
| 特殊成员   | 解释                     |
+------------+--------------------------+
| __doc__    | 类的描述                 |
+------------+--------------------------+
| __module__ | 类所属的 module          |
+------------+--------------------------+
| __class__  | 所属的类, 一般类属于type |
+------------+--------------------------+
| __dict__   | 对象的所有Attr 的字典    |
+------------+--------------------------+

特殊方法
"""""""

参考

`Python 魔术方法指南`_

`Python 官方文档`_

`一篇文章搞懂Python中的面向对象编程`_

`Python 面向对象(进阶篇)`_

.. _Python 魔术方法指南: https://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html#python
.. _Python 官方文档: https://docs.python.org/3/reference/datamodel.html#basic-customization
.. _一篇文章搞懂Python中的面向对象编程: http://yangcongchufang.com/%E9%AB%98%E7%BA%A7python%E7%BC%96%E7%A8%8B%E5%9F%BA%E7%A1%80/python-object-class.html#dir5
.. _Python 面向对象(进阶篇): https://www.imooc.com/article/3066#

+---------------------------------+-----------------------------------+---------------------------------+
| 特殊方法                        | 调用方式                          | 解释                            |
+=================================+===================================+=================================+
| __new__(cls [,...])             | instance = MyClass(arg1, arg2)    | __new__ 在创建实例的时候被调用  |
+---------------------------------+-----------------------------------+---------------------------------+
| __init__(self [,...])           | instance = MyClass(arg1, arg2)    | __init__ 在创建实例的时候被调用 |
+---------------------------------+-----------------------------------+---------------------------------+
| __cmp__(self, other)            | self == other, self > other, 等。 | 在比较的时候调用                |
+---------------------------------+-----------------------------------+---------------------------------+
| __pos__(self)                   | +self                             | 一元加运算符                    |
+---------------------------------+-----------------------------------+---------------------------------+
| __neg__(self)                   | -self                             | 一元减运算符                    |
+---------------------------------+-----------------------------------+---------------------------------+
| __invert__(self)                | ~self                             | 取反运算符                      |
+---------------------------------+-----------------------------------+---------------------------------+
| __index__(self)                 | x[self]                           | 对象被作为索引使用的时候        |
+---------------------------------+-----------------------------------+---------------------------------+
| __nonzero__(self)               | bool(self)                        | 对象的布尔值                    |
+---------------------------------+-----------------------------------+---------------------------------+
| __getattr__(self, name)         | self.name # name 不存在           | 访问一个不存在的属性时          |
+---------------------------------+-----------------------------------+---------------------------------+
| __setattr__(self, name, val)    | self.name = val                   | 对一个属性赋值时                |
+---------------------------------+-----------------------------------+---------------------------------+
| __delattr__(self, name)         | del self.name                     | 删除一个属性时                  |
+---------------------------------+-----------------------------------+---------------------------------+
| __getattribute(self, name)      | self.name                         | 访问任何属性时                  |
+---------------------------------+-----------------------------------+---------------------------------+
| __getitem__(self, key)          | self[key]                         | 使用索引访问元素时              |
+---------------------------------+-----------------------------------+---------------------------------+
| __setitem__(self, key, val)     | self[key] = val                   | 对某个索引值赋值时              |
+---------------------------------+-----------------------------------+---------------------------------+
| __delitem__(self, key)          | del self[key]                     | 删除某个索引值时                |
+---------------------------------+-----------------------------------+---------------------------------+
| __iter__(self)                  | for x in self                     | 迭代时                          |
+---------------------------------+-----------------------------------+---------------------------------+
| __contains__(self, value)       | value in self, value not in self  | 使用 in 操作测试关系时          |
+---------------------------------+-----------------------------------+---------------------------------+
| __concat__(self, value)         | self + other                      | 连接两个对象时                  |
+---------------------------------+-----------------------------------+---------------------------------+
| __call__(self [,...])           | self(args)                        | “调用”对象时                    |
+---------------------------------+-----------------------------------+---------------------------------+
| __enter__(self)                 | with self as x:                   | with 语句环境管理               |
+---------------------------------+-----------------------------------+---------------------------------+
| __exit__(self, exc, val, trace) | with self as x:                   | with 语句环境管理               |
+---------------------------------+-----------------------------------+---------------------------------+
| __getstate__(self)              | pickle.dump(pkl_file, self)       | 序列化                          |
+---------------------------------+-----------------------------------+---------------------------------+
| __setstate__(self)              | data = pickle.load(pkl_file)      | 序列化                          |
+---------------------------------+-----------------------------------+---------------------------------+


文件操作
-------------------------

``Python`` 通过 ``open(filname, flag)`` 函数打开一个文件, 返回一个文件对象, 并通过对象对文件进行操作. 

:filename:  要打开的文件
:flag:      标志, 控制对文件的权限

标志包括 ``a, w, r, b, t, +, x``.

``a``: 添加; 打开文件, 并将指针置于文件末尾.

``w``: 写入; 打开文件, 并将指针置于文件开头. 不存则会创建文件.

``r``: 读取; 打开文件, 并将指针置于文件开头

``b``: 是否以二进制的方式读写.

``t``: 以 ``str`` 的方式读写, 默认.

``+``: 将权限提高为读写.

``x``: 创建文件, 如果文件存在则报错. 有写权限.

读取方法
''''''''''''''''''''''''''

从文件中读取:

file.read(size)
  如果 ``size`` 未指定, 返回读取整个文件. ``size`` 包含 ``\n`` .

file.readline()
  返回一行数据, 包含 ``\n`` .

file.readlines(size)
  返回在 ``size`` 内行的列表,  如果未指定, 返回包含全部行的列表, 每个元素包含 ``\n`` . ``size`` 不包含 ``\n`` .

.. attention:: 

  注意, ``size`` 指的是字符数.

此外还可以将文件对象当作迭代器读取::

  with open(filename, 'r') as f:
    for line in f:
      ...

写入方法
'''''''''''''''''''''''''''

往文件写入:

file.write(content)
  ``content`` 必须是字符串.

file.writelines(content)
  ``content`` 可以是字符串或着列表.

其他
''''''''''''''''''''''''''

除了写入和读取方法, 还有对指针的操作方法:

file.seek(offset, position=1)
  移动文件指针.

:offset: 偏移量; 单位为字符, 可正可负数.
:position: 相对位置; ``0`` - 文件开头; ``1`` - 当前位置; ``2`` - 文件结尾.

.. important:: 只有以二进制方式打开时, 才可以使用负偏移量.

file.tell()
  返回当前指针相对于文件头的位置, 即第几个字符. 例如从开头读取 ``6`` 个字符, 则返回 ``7``.

file.close()
  清空 ``buffer``, 关闭文件.

除了方法, 文件对象还有属性:

file.mode
  返回打开文件的标记.

Utils
--------------------------

Assertion
''''''''''''''''''''''''''

断言可以用来判断, 确保程序运行正确.

断言二维列表::

  assert isinstance(input_data[0], list)

Python2 与 Python3 的不同
---------------------------

``Python2`` 与 ``Python3`` 主要不同之处.

.. hint:: ``Python2`` 将在 2020 年全面弃用并不再提供支持.


除法
''''''''''''''''''''''''

普通除法
"""""""""""""""""""""""""

``Python2`` 中的普通除法:

  - 如果两边都为整数, 则结果为整数.

  >>> 2 / 3
  1

  - 如果有一边为浮点数, 则结果为浮点数.

  >>> 3 / 2
  1.5

``Python3`` 中的普通除法:

  - 无论两边是什么类型, 结果都为浮点数.

  >>> 3 / 2
  1.5
  >>> 3 / 3
  1.0

地板除法
"""""""""""""""""""""""""""

.. sidebar:: 地板除法

  地板除法指的是操作符 ``//``. 

地板除法是后来添加在 ``Python2`` 中的, 与 ``Python3`` 中的效果相同. 

首先无论如何都只保留整数部分. 如果两边都为整型, 则结果为整型. 若有浮点型, 则结果为浮点型.

>>> 3 // 2
1
>>> 3 // 2.0
1.5

总结
"""""""""""""""""""""""""""

1. **普通除法**, ``Python2`` 根据两边类型决定结果类型, ``Python3`` 全部为浮点型.
2. **地板除法**, ``Python2`` 与 ``Python3`` 相同, 只取整数部分, 结果类型与表达式类型有关.

类的不同
''''''''''''

Python2 中有经典类与新式类, Python3 中所有的都是新式类. 详见 `面向对象`_

交互式CLI
-------------

历史结果
  交互式CLI 里, 像 ``shell`` 的上次命令是 ``!!`` 一样, ``_`` 代表上次的输出. 
  更进一步的 ``__`` 代表倒数第二次的输出, ``___`` 代表倒数第三次的输出.

Ipython 交互
''''''''''''''''

运行文件::

  %run file
