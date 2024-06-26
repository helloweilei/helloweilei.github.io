---
title: Python进阶
---
## 闭包

### 什么是闭包

闭包是作用域的延伸，类似于JS中的闭包，当在函数作用域中引用外部的非全局变量时，就会形成闭包。当函数在其他地方被调用时，依然可以访问到被引用的变量。闭包与函数是否为匿名函数无关。

> 可调用对象：对象实现了 `__call__`方法，可以像函数一样被调用。

示例：

```python
def make_averager():
    sum = 0
    count = 0

    def append(val):
        nonlocal sum, count
        sum = sum + val
        count += 1
        return sum / count

    return append


avg = make_averager()
avg(10)
avg(20)
print(avg(3)) # 11
print(avg.__code__.co_varnames)  # ('val', )
print(avg.__code__.co_freevars)  # ('count', 'sum'), 自由变量名
print(avg.__closure__[0].cell_contents,
      avg.__closure__[1].cell_contents)  # 3 33 // 闭包中自由变量的值
```

> Python中不需要申明变量，所以如果上面的代码中没有 `nonlocal`申明，在内部函数中直接为sum和count复制会创建新的同名局部变量，无法达到修改外部变量的目的。

### 总结

闭包是一种函数，该函数保留了定义函数时存在的自由变量的绑定，函数调用时即使离开了作用域依然可以使用绑定。

## 装饰器

### 简介

在Python中，装饰器是一种设计模式，它可以用来修改或增强函数、方法或类的行为。装饰器通常用于在不修改原始代码的情况下，为函数或方法添加额外的功能，如日志记录、性能测试、事务处理等。

### 定义一个装饰器

装饰器本质上就是一个函数，接受被装饰的函数作为参数，装饰器需要返回一个新的函数替换原函数（也可以直接返回原函数）。下面定义一个装饰器用于在函数执行时打印参数：

```python
import functools

def print_params(func):
    @functools.wraps(func) # 将原函数的自省信息拷贝到新函数
    def new_func(*args):
        for i, name in enumerate(func.__code__.co_varnames):
            print("{}: {}".format(name, args[i]))
        return func(*args)
    return new_func

# 使用装饰器
@print_params
def add(val1, val2):
    return val1 + val2
```

### 标准库中的装饰器

- functools.lru_cache: 为函数提供缓存能力
- functools.singledispatch: 将普通函数变成泛函数；根据第一个参数的类型以不同的方式执行相同操作的一组函数。下面的示例中定义的htmlize函数对不同类型的参数执行不同的格式化处理：

  ```python
  from functools import singledispatch
  ```

> 可以同时为一个函数指定多个装饰器，例如：`@d1 @d2 def func()`等价与 `d1(d2(func))`。

### 参数化装饰器

接受参数并返回一个装饰器的工厂函数。

例如：

```python
def log(level='Info'):
    def decorator(func):
        print("{}: Calling fun '{}'".format(level, func.__name__))
        return func
    return decorator


@log(level='Debug')
def add(val1, val2):
    return val1 + val2
```

### classmethod与staticmethod的区别

在Python中，装饰器可以使用类方法（classmethod）或静态方法（staticmethod）。这两种方法都用于将装饰器与类关联起来，但它们之间有一些重要的区别。

1. 类方法（classmethod）：
   类方法是与类关联的函数，而不是与类的实例关联。类方法使用@classmethod装饰器定义，第一个参数必须是类本身（通常命名为cls）。类方法可以使用cls参数来访问类的属性和其他方法。类方法通常用于实现与类相关的功能，而不是与类的实例相关的功能。

例如，假设我们有一个装饰器用于记录函数执行的时间：

```python
import time
  class Timer:
      @classmethod
      def decorator(cls, func):
          def wrapper(*args, **kwargs):
              start_time = time.time()
              result = func(*args, **kwargs)
              end_time = time.time()
              print(f"Function {func.__name__} took {end_time - start_time} seconds to execute.")
              return result
          return wrapper
```

2. 静态方法（staticmethod）：
   静态方法是与类关联的函数，不需要与类的实例关联。静态方法使用@staticmethod装饰器定义，不需要特殊的第一个参数。静态方法不能使用self参数来访问类的属性和其他方法。静态方法通常用于实现与类相关但不依赖于实例的状态的功能。

例如，假设我们有一个装饰器用于验证函数输入：

```python
class Validator:
      @staticmethod
      def decorator(func):
          def wrapper(*args, **kwargs):
              for arg in args:
                  if not isinstance(arg, int):
                      raise ValueError("All arguments must be integers.")
              result = func(*args, **kwargs)
              return result
          return wrapper
```

总结：
类方法和静态方法的区别在于它们如何与类关联以及它们如何使用。类方法使用cls参数来访问类，可以使用cls参数来访问类的属性和其他方法。它们通常用于实现与类相关的功能。静态方法不需要特殊的第一个参数，不能使用self参数来访问类的属性和其他方法。它们通常用于实现与类相关但不依赖于实例的状态的功能。

### 上下文管理器

需要实现上下文管理器的协议，包含`__enter__`, `__exit__`两个方法，在`with`语句中会首先调用`__enter__`方法，方法的返回值会赋值给as后面的变量。在`with`语句块结束是会调用管理器的`__exit__`方法，主要用于执行一些清理工作。

```python
import sys

class ContextManager:
    def __init__(self, msg):
        self.msg = msg
        self.oroginal_write = sys.stdout.write
        sys.stdout.write = self._proxy_write

    def _proxy_write(self, msg):
        self.oroginal_write(msg[::-1])

    def __enter__(self):
        return self.msg
    
    def __exit__(self, exc_type, *others):        
        sys.stdout.write = self.oroginal_write
        print("exiting from context...")

'''
output:
dlroW olleH
exiting from context...
Hello World
'''
with ContextManager("Hello World") as msg:
    print(msg)

print(msg)
```
