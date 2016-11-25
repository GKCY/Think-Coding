据说GIL(全局解释器锁 Global Interpreter Lock)是Python中最难的问题，要想把它讲清楚着实有点难度。

那么GIL这个问题是怎么产生的呢？首先我们来了解下Python的垃圾回收制度。

相对于C++与Java，Python中的变量名实际上是对象的引用。Python使用引用计数器的方法来管理内存中的对象，使用`sys.getrefcount()`方法可以查看一个对象的引用次数（注意：使用此方法得到的实际次数会多1）。当一个对象的引用计数为0的时候，这个对象就会被垃圾收集器回收。

引用计数方法有一个致命的弱点，那就是对象的循环引用，举个例子。

```Python
class Leak(object):
    def __init__(self):
        print "object with id %d was born" %id(self)

while(True):
    A = Leak()
    B = Leak()
    A.b = B
    B.a = A
    A = None
    B = None
    #来源于《改善Python程序的91个建议》
```

运行上述程序，就会发现，Python进程的内存消耗会一直增长直到耗光，为什么会这样?

即使当所有引用都消失，由于两个对象之间相互引用，它们的引用计数也不会为0，当然对象自己引用自己也会出现这种问题。这样就造成了该对象并不会被垃圾收集器回收，该内存无法被释放。

为此，Python引用了其他垃圾收集机制来弥补这个缺陷：标记-清除法与分代回收制度。这里不再赘述。

这与GIL有什么关系？