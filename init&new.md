### __init__ 和 __new__ 的区别

* __new__是构建某个类实例的方法，__init__则是初始化该实例的方法.
* __new__先于__init__调用

```python
class Person(object):

    def __new__(cls, name, age):
        print('called __new__')
        return super(Person, cls).__new__(cls)

    def __init__(self, name, age):
        print('called __init__')
        self.name = name
        self.age = age

p = Person('张三', 18)

```
上面的代码执行后会输出
```
called __new__
called __init__
```

直接调用__new__方法生成实例会报错
因为__init__方法并未被调用，name属性还未被初始化
```python
p1 = Person.__new__(Person,'张三', 18)
# 调用p1.name时会报错
'Person' object has no attribute 'name'
```

* 通常来说，新式类开始实例化时，__new__()方法会返回cls（cls指代当前类）的实例，然后该类的__init__()方法作为构造方法会接收这个实例（即self）作为自己的第一个参数，然后依次传入__new__()方法中接收的位置参数和命名参数。
注意：如果__new__()没有返回cls（即当前类）的实例，那么当前类的__init__()方法是不会被调用的。如果__new__()返回其他类（新式类或经典类均可）的实例，那么只会调用被返回的那个类的构造方法。

* new方法主要是当你继承一些不可变的class时(比如int, str, tuple)， 提供给你一个自定义这些类的实例化过程的途径。还有就是实现自定义的metaclass。
* 单例模式

```python
# 实例化一个单例
class Singleton(object):
    __instance = None

    def __new__(cls, age, name):
        #如果类数字__instance没有或者没有赋值
        #那么就创建一个对象，并且赋值为这个对象的引用，保证下次调用这个方法时
        #能够知道之前已经创建过对象了，这样就保证了只有1个对象
        if not cls.__instance:
            cls.__instance = object.__new__(cls)
        return cls.__instance
```
