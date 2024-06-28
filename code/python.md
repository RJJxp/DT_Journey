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

# Jira 
```python
from jira import JIRA
import re

# JIRA服务器配置
jira_options = {'server': 'https://jira.xiaopeng.us'}

# 认证信息
jira_user = 'user_name'
jira_password = 'password'

# 认证并创建一个JIRA实例
jira = JIRA(options=jira_options, basic_auth=(jira_user, jira_password))

# 指定的JIRA单ID
issue_id = 'jira-id'

# 获取JIRA单
issue = jira.issue(issue_id)
issue_fields = dir(issue.fields)

v = getattr(issue.fields, 'description')
# print(v)

pattern = r"dds_link:\s*\[(.*?)\]"
match = re.search(pattern, v)
if match:
    dds_link = match.group(1)
    print(dds_link)
else:
    print("No match found")
```

# logger相关
python推荐4个logger库
1. logging
```python
import logging

# 配置日志
logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                    datefmt='%Y-%m-%d %H:%M:%S',
                    filename='app.log',
                    filemode='w')

# 创建 logger
logger = logging.getLogger(__name__)

# 使用不同的日志级别进行记录
logger.debug('这是一个 debug 级别的日志')
logger.info('这是一个 info 级别的日志')
logger.warning('这是一个 warning 级别的日志')
logger.error('这是一个 error 级别的日志')
logger.critical('这是一个 critical 级别的日志')
```
2. loguru
loguru 是一个第三方 Python 日志库, 以其简单的使用方式和强大的功能而受到许多开发者的喜爱. 与 Python 内置的 logging 库相比, loguru 提供了一些额外的便利性和灵活性, 例如自动变量捕获, 更好的异常跟踪, 简化的配置以及无需定义 logger 实例等.

主要特点
- 简单易用: 使用 loguru 可以非常简单地开始记录日志，无需复杂配置
- 灵活的配置: 支持日志旋转, 日志压缩, 文件记录等多种功能
- 丰富的上下文信息: 自动记录调用信息, 如文件名, 行号等
- 优秀的异常处理: 提供了更加详细的异常跟踪信息

```python
from loguru import logger

logger.add("file_{time}.log", rotation="1 week")  # 每周自动旋转日志文件

logger.debug("这是一个 debug 级别的日志")
logger.info("这是一个 info 级别的日志")
logger.warning("这是一个 warning 级别的日志")
logger.error("这是一个 error 级别的日志")
logger.critical("这是一个 critical 级别的日志")
```

3. structlog 使得记录结构化日志变得简单。它旨在与标准的 logging 库一起工作，并提供了一种方式来记录信息，这些信息更易于分析和过滤，特别是在大型应用程序和微服务架构中。
```python
import structlog

logger = structlog.get_logger()
logger.info("login_attempt", username="admin", password="secret")
```

4. daiquiri 是一个基于 Python 的 logging 库的简单封装，旨在让日志记录变得更简单。它提供了一种简单的方式来设置日志记录，包括输出到标准输出和文件。
```python
import daiquiri

daiquiri.setup(level="INFO")
logger = daiquiri.getLogger(__name__)

logger.info("Hello, world!")
```

5. logbook 是一个为 Python 设计的日志库，提供了类似于 logging 的接口，但是在性能和灵活性方面做了优化。它支持日志记录到文件、邮件、Web 应用等。
```python
from logbook import Logger, StreamHandler
import sys

StreamHandler(sys.stdout).push_application()
logger = Logger('My Logger')

logger.info('Hello, World!')
```

# 行的分割
因为Windows系统通常使用`\r\n`作为行结束符，而Linux和macOS使用`\n`。因此python中字符串行的分隔使用str.`splitlines()`来分割字符串为行，这样可以自动处理不同平台的换行符差异

# re正则表达式
如下, 对于window平台通过`\r\n`换行的字符串`txt`, 每一行通过空格分隔.
```txt
 ../
JIRA-XKPI_SR20-27080/                              21-Jun-2024 23:48       -
XBRAIN_MC_output/                                  22-Jun-2024 13:28       -
xdds_scene/                                        21-Jun-2024 21:57       -
ISSUE.SUCCESS                                      23-Jun-2024 09:55       5
access.txt                                         21-Jun-2024 23:48      50
all_target_path_dicts.json                         21-Jun-2024 23:48    4266
dds_files_relocation_mapping_table.html            21-Jun-2024 23:48    6029
discovery                                          21-Jun-2024 23:48     63K
fog_detection_logs.txt                             21-Jun-2024 23:48     48K
metadata                                           23-Jun-2024 09:41     24K
navi_info.json                                     21-Jun-2024 21:57    4906
navigation_serialization.json                      21-Jun-2024 21:57     140
oss_operations.txt                                 21-Jun-2024 23:48     47K
record_sizes_oss.json                              21-Jun-2024 23:48     108
recording_46_2024-6-21_14:26:26.dat.lz4            21-Jun-2024 21:57    912M
recording_47_2024-6-21_14:27:22.dat.lz4            21-Jun-2024 21:57    742M
recording_dds_relocation_map.json                  21-Jun-2024 23:48    2660
screen_recording.mp4                               21-Jun-2024 14:35     30M
screen_recording_2.mp4                             21-Jun-2024 14:35     11M
target_path_info.json                              21-Jun-2024 23:48     11K
version.txt                                        21-Jun-2024 23:48     221
version_detail.txt                                 21-Jun-2024 23:48    5733
```
使用正则表达式提取第一列

`^`表示字符串开始位置, `\s`表示空格, `*`表示0个或多个, `\s*`表示可以有空格, `()`内的是匹配项返回值, `^`在`[]`中, 表示非, `[^\s]`则表示非空格字符, `[^\s]+`表示一串非空字符, 因此正则表达式`'^\s*([^\s]+)'`表示, 从字符串开始匹配到第一块非空字符, 即第一列.

```python
pattern = re.compile(r'^\s*([^\s]+)', re.MULTILINE)
            matches = pattern.findall(txt)
            dir_list.extend(matches)
```
注意, 第二列中间也有空格, 使用正则表达式提取第二列

同理, `.*?`: `.*`匹配任意字符(除了换行符), `?` 使得 `.*` 变成非贪婪模式, 即尽可能少地匹配字符
```python
pattern = re.compile(r'^\s*([^\s]+)\s+([^\s].*?[^\s])\s+')
for line in txt.splitlines():
    match = pattern.match(line)
    if match:
        # 提取第二列数据
        second_column = match.group(2)
        second_column_data.append(second_column)
```

# `__init__.py`的作用
`__init__.py` 文件在 Python 中有几个重要的作用：

1. 包的标识：在 Python 中，一个目录只有包含一个 `__init__.py` 文件时才会被认为是一个包，这样才能导入该目录下的模块。这个文件可以是空的，但它的存在允许 Python 处理包含它的目录作为一个包。

2. 初始化代码：`__init__.py` 文件可以包含用于包初始化的代码。这意味着当包被导入时，`__init__.py` 中的代码将被执行。这可以用来设置包所需的任何初始状态或变量。

3. 命名空间管理：通过在 `__init__.py` 文件中导入子模块或子包，可以调整包的内部结构对外界的可见性。这样，包的使用者可以通过更简洁的路径访问这些模块，而不需要了解包内部的目录结构。

4. 方便的导入机制：可以在 `__init__.py` 文件中定义 `__all__` 列表，来指定 from module import * 时应该导入哪些对象。这样，当使用这种导入语句时，只有在 `__all__` 中明确指定的对象才会被导入。

简而言之，`__init__.py` 文件在定义 Python 包时起着至关重要的作用，它不仅标识了一个目录是 Python 的包，还可以包含包初始化代码，管理包的命名空间，以及控制模块的导入行为。

# 单例模式

在Python中，super() 函数用于调用父类(超类)的一个方法。super() 函数的常见用法是在子类中调用父类的初始化方法 (__init__)。在您提供的代码片段中，super() 被用于一个元类的上下文中，这是一种更高级的用法。

```python
class Singleton(type):
    def __init__(cls, name, bases, dict):
        super(Singleton, cls).__init__(name, bases, dict)
        cls._instance = None

    def __call__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instance
```

让我们分解 `super()` 函数在您的代码中的用法：这里的 super() 调用涉及到以下参数：

1. 第一个参数 (Singleton)：这是当前类的名称。在这个上下文中，它告诉 super()，我们想要引用的基类是相对于 Singleton 类的。这意味着 super() 将查找 Singleton 在继承链中的父类。

2. 第二个参数 (cls)：这是当前类的实例。在元类的上下文中，cls 实际上是一个类对象，而不是常规类实例的实例。这个参数告诉 super()，我们正在为哪个类对象调用父类的方法

super(Singleton, cls).\_\_init\_\_(name, bases, dict) 这行代码的作用是调用 Singleton 的父类的 \_\_init\_\_ 方法。这个 \_\_init\_\_ 方法是在创建类对象时被调用的，它接收三个参数：

**name**：创建的类的名称。
**bases**：创建的类的基类的元组。
**dict**：创建的类的命名空间的字典。
这种用法确保了 Singleton 类的父类的初始化方法被正确调用，允许 Singleton 类在Python的类构造过程中插入自定义逻辑。在这个特定的例子中，Singleton 类似乎是设计为一个元类，用于实现单例模式，这意味着每次尝试创建其实例的类的实例时，都会返回相同的实例对象。

```python
class Singleton02(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton02, cls).__call__(*args, **kwargs)
        return cls._instances[cls]
```

# yield关键字
yield 关键字在 Python 中用于从一个函数返回一个生成器（generator）。当函数执行到一个 yield 语句时，函数的状态会被暂时保存，包括所有局部变量和控制流的位置。下次调用生成器的 \_\_next\_\_() 方法或使用 next() 函数时，函数会从上次返回的 yield 语句处继续执行，直到遇到下一个 yield 语句或函数结束。

使用 yield 的函数被称为生成器函数。这种机制允许函数在保持当前状态的情况下，分批次返回结果，这对于处理大量数据或需要长时间运行的计算非常有用，因为它可以减少内存的使用，并允许即时处理每个返回的部分结果。

在提供的代码中，yield line 使得 readLargeTxtFile 函数成为一个生成器函数，允许逐行读取大文件，而不是一次性将整个文件内容加载到内存中。这样，调用者可以通过一个循环来处理每一行，每次循环都会从文件中读取并返回下一行，直到文件的末尾。这种方式非常适合处理大型文本文件，因为它可以显著减少内存的使用。

yield 关键字在 Python 中除了用于生成器函数返回值外，还有其他几种用途：

1. 生成器表达式：与列表推导类似，但使用圆括号而不是方括号，生成器表达式返回一个生成器对象。例如，(x*x for x in range(10)) 会生成一个生成器，可以逐个产生 0 到 9 的平方。

2. yield from 语句：用于从另一个生成器中产生所有值。这允许一个生成器委托部分操作给另一个生成器。例如，在一个生成器函数中，使用 yield from another_generator() 可以将 another_generator 产生的所有值传递给调用者。

3. 协程：在 Python 3.3 以后，yield 可以用在协程（coroutine）中，作为数据的生产者和消费者之间的通信方式。通过 yield，协程可以暂停执行并等待，直到它被某些外部事件（如数据的到来）唤醒。这是异步编程的一种方式，特别适用于 I/O 密集型任务。

4. 使用 yield 实现状态机：由于 yield 可以保存函数的执行状态，因此可以用来实现复杂的控制流程，如状态机。在每个状态之间切换时，可以使用 yield 暂停和恢复状态。

这些用途展示了 yield 关键字在 Python 中的灵活性，使其成为实现生成器、协程和其他基于状态或事件驱动逻辑的强大工具