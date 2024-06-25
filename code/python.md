# importlib.import_module
```python
import importlib
import os
import logging

def dynamic_import(case_path):
    try:
        class_name = os.path.basename(case_path).split('.')[0]
        group_name = os.path.basename(os.path.dirname(case_path))
        module_name = f'cases.{group_name}.{class_name}'

        module = importlib.import_module(module_name)
        case_class = getattr(module, class_name)
        case_instance = case_class()
        case_instance.group = group_name
        case_instance.case_path = case_path
        return case_instance

    except Exception as e:
        logger.error(e)
        raise ImportError(f'导入{case_path}失败')

# 多态
obj_list = []
for path in path_list:
   ojb_list.append(dynamic_import(path)) 
```

# classmethod
在Python中, `classmethod`是一个装饰器, 它将方法绑定到类而不是其对象. 这意味着, 使用classmethod装饰的方法可以通过类本身调用, 而不需要创建类的实例. 这种方法的第一个参数是类本身，通常命名为`cls`, 与实例方法的第一个参数是实例本身(通常命名为self)不同.

classmethod的典型用途包括:
1. 工厂方法: 创建并返回类的实例, 可能使用不同的参数比直接使用构造函数更灵活; 
```python
class Animal:
    @classmethod
    def speak(cls):
        raise NotImplementedError("Subclasses must implement this method")

class Dog(Animal):
    @classmethod
    def speak(cls):
        return "Woof!"

class Cat(Animal):
    @classmethod
    def speak(cls):
        return "Meow!"

# 使用类方法实现多态
animals = [Dog, Cat]
for animal in animals:
    print(animal.speak())
```

2. 通过类调用的方法, 这些方法可以访问类级别的数据或属性.

```python
class MyClass:
    @classmethod
    def example_classmethod(cls):
        print(f"This is a class method of {cls}")

    def example_method(self):
        print("This is an instance method")

# 使用类名直接调用classmethod
MyClass.example_classmethod()

# 创建实例并调用实例方法
instance = MyClass()
instance.example_method()
```