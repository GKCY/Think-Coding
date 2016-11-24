
[官方文档](https://docs.python.org/3/library/functools.html)如下：

> @functools.wraps(wrapped, assigned=WRAPPER_ASSIGNMENTS, updated=WRAPPER_UPDATES)
> This is a convenience function for invoking update_wrapper() as a function decorator when defining a wrapper function. It is equivalent to partial(update_wrapper, wrapped=wrapped, assigned=assigned, updated=updated). For example:
> ```Python
> >>> from functools import wraps
> >>> def my_decorator(f):
> ...     @wraps(f)
> ...     def wrapper(*args, **kwds):
> ...         print('Calling decorated function')
> ...         return f(*args, **kwds)
> ...     return wrapper
> ...
> >>> @my_decorator
> ... def example():
> ...     """Docstring"""
> ...     print('Called example function')
> ...
> >>> example()
> Calling decorated function
> Called example function
> >>> example.__name__
> 'example'
> >>> example.__doc__
> 'Docstring'
> ```
>
> Without the use of this decorator factory, the name of the example function would have been 'wrapper', and the docstring of the original example() would have been lost.

如果没有wraps装饰器的使用，`example`函数的`__name__`属性会变成`'wrapper'`,而且原始函数`example()`的`__doc__`属性会丢失。





