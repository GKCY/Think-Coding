假如我们现在要判断一个列表listA是否为空

```Python
if listA is not None:
    Do something
else:
    Do some other thing
```

这样写是有问题的，很容易可以得到验证。

```Python
>>> a = []
>>> a is not None
True
```

那么为什么呢？

首先我们来了解下Python中哪些形式的数据为空，有以下这些。

- 常量None
- 常量False
- 任何形式的数值类型零，如0、0L、0.0、0j
- 空的序列如 ''、()、[]
- 空的字典，如{}
- 类中定义了nonzero()方法和len()方法，并且该方法返回整数值0或者布尔值False的时候

None的特殊之处在于他既不是0、False也不是空字符串空序列等，调用`type(None)`我们可以发现，其数据类型为`NoneType`。官方文档对None的解释是这样的：

> ###### None
>
> The sole value of the type `NoneType`. `None` is frequently used to represent the absence of a value, as when default arguments are not passed to a function. Assignments to `None` are illegal and raise a `SyntaxError`

None遵循单例模式，是唯一的，所有赋值为None的变量都相等。None与非None对象的比较全都为False。

```Python
>>> id(None)
4542996072
>>> None == 0
False
>>> None == ''
False
>>> None == False
False
>>> None == []
False
>>> None == {}
False
>>> None == ()
False
```

正确的做法应该是：

```Python
if listA:
    Do something
else:
    Do some other thing
```

这么写实际上发生了什么？

实际上在执行过程中会调用`__nonzero__()`方法来判断listA是否为空。此方法用于对自身对象进行空值测试，返回0/1或者True/False。如果一个类没有定义该方法，Python则获取`__len__()`方法调用的结果进行判断。`__len__()`返回值为0表示为空。如果一个类中上述两种方法都没有定义，该类的实例用if判断的结果都为True。