---
title: python日志管理——blogging
tags: 
    - Python
    - 教程
categories:
    - Python
---

## 认识blogging中的对象
python从python2.3开始加入日志模块logging，其子模块包含loggers，handlers，filters 和 formatters。

### Logger——记录器
#### Logger的作用
Logger的作用：

1. 为应用程序提供接口，以便应用可以在运行时写log；
2. 按照log等级或filter对象来决定是否将log信息传递给相关handlers；

#### Logger的方法
Logger从来不直接实例化，经常通过logging模块级方法（Module-Level  Function）logging.getLogger(name)来获得，其中如果name不给定就用root。名字是以点号分割的命名方式命名的(a.b.c)。对同一个名字的多个调用logging.getLogger()方法会返回同一个logger对象。这种命名方式里面，后面的loggers是前面logger的子logger，自动继承父loggers的log信息，正因为此,没有必要把一个应用的所有logger都配置一遍，只要把顶层的logger配置好了，然后子logger根据需要继承就行了。

Logger的具体方法有：

- setLevel(lvl)：定义处理log的最低等级，内建的级别为：DEBUG，INFO，WARNING，ERROR，CRITICAL；下图是级别对应数值：


| 级别 | 何时使用 | 数值 |
| --- | --- | --- |
| NOTSET |  | 0 |
| DEBUG | 详细信息，典型地调试问题时会感兴趣 | 10 |
| INFO |  <div>证明事情按预期工作。</div>| 20 |
| WARNING | 表明发生了一些意外，或者不久的将来会发生问题（如‘磁盘满了’）。软件还是在正常工作。 | 30 |
| ERROR | 由于更严重的问题，软件已不能执行一些功能了。 | 40 |
| CRITICAL | 严重错误，表明软件已不能继续运行了。 | 50 |

- debug(log_message, [*args[, **kwargs]])
- info(log_message, [*args[, **kwargs]])
- warning(log_message, [*args[, **kwargs]])
- error(log_message, [*args[, **kwargs]])
- critical(log_message, [*args[, **kwargs]])
- exception(message[, *args])：和error()一样，多出一个stack trace用于转储，exception()方法只能从一个exception handler里面调用
- log(log_level, log_message, [*args[, **kwargs]])   显式的带一个level参数,用这个可以得到比使用上面所列举的方法更为详细的log信息
- addFilter()和removeFilter()分别用于为handler增加一个filter和删除一个filter
- addHandler(hdlr)加入一个Handler

### Handler——处理器
Logger对象通过addHandler()方法添加零个或多个handler对象到Logger中，Handler将（记录器产生的）日志记录发送至合适的目的地，Handler对象有以下几种：


| 类型 | 说明 | 位置 |
| --- | --- | --- |
| StreamHandler | 发送日志到流中 | 核心logging模块中 |
| FileHandler | 发送日志到文件中  | 核心logging模块中 |
| NullHandler | 无操作的日志处理类  | 核心logging模块中 |

常用的一些Handler定义在子模块logging.handlers中:


| Handler | 说明 |
| --- | --- |
| WatchedFileHandler | 是一个FileHandler，当日志文件改变后，文件会被关闭，并且使用同样的文件名再次打开。如果由于在运行的程序外部执行了日志备份操作, 从而导致日志文件已被删除或移动, 这些变化可能就会发生。此处理器只能工作在UNIX系统上。 |
| RotatingFileHandler | 发送日志到文件，并且限制最大的日志文件大小，并适时轮徇。maxBytes单个文件的最大字节数；backupCount部分文件的个数，当文件超出maxBytes后会重新部分在新文件中，旧文件会以.1重新命名，最终备份文件数等于.backupCount |
| TimedRotatingFileHandler | 发生日志到文件，并在适当的事件间隔进行轮徇 |
| SocketHandler  | 日志通过TCP协议发送 |
| DatagramHandler | 日志通过UDP协议发送 |
| SysLogHandler  | 发送日志到UNIX syslog服务，并支持远程syslog服务 |
| NTEventLogHandler | 发送日志到WindowsNT/2000/XP事件日志 |
| MemoryHandler | 发送日志到内存中的缓冲区，并在达到特定条件时清空 |
| HTTPHandler | 通过GET或POST方法发送日志到HTTP服务器 |

更详细的Handler介绍：

1. logging.StreamHandler：使用这个Handler可以向类似与sys.stdout或者sys.stderr的任何文件对象(file object)输出信息。它的构造函数是：StreamHandler([strm])，其中strm参数是一个文件对象。默认是sys.stderr
2. logging.FileHandler：和StreamHandler类似，用于向一个文件输出日志信息。不过FileHandler会帮你打开这个文件。它的构造函数是：FileHandler(filename[,mode])，filename是文件名，必须指定一个文件名。mode是文件的打开方式。默认是’a'，即添加到文件末尾。
3. logging.handlers.RotatingFileHandler：这个Handler类似于上面的FileHandler，但是它可以管理文件大小。当文件达到一定大小之后，它会自动将当前日志文件改名，然后创建一个新的同名日志文件继续输出。比如日志文件是chat.log。当chat.log达到指定的大小之后，RotatingFileHandler自动把 文件改名为chat.log.1。不过，如果chat.log.1已经存在，会先把chat.log.1重命名为chat.log.2。。。最后重新创建 chat.log，继续输出日志信息。它的构造函数是：RotatingFileHandler( filename[, mode[, maxBytes[, backupCount]]])，其中filename和mode两个参数和FileHandler一样。maxBytes用于指定日志文件的最大文件大小。如果maxBytes为0，意味着日志文件可以无限大，这时上面描述的重命名过程就不会发生。backupCount用于指定保留的备份文件的个数。比如，如果指定为2，当上面描述的重命名过程发生时，原有的chat.log.2并不会被更名，而是被删除。
4. logging.handlers.TimedRotatingFileHandler：这个Handler和RotatingFileHandler类似,不过,它没有通过判断文件大小来决定何时重新创建日志文件，而是间隔一定时间就 自动创建新的日志文件。重命名的过程与RotatingFileHandler类似，不过新的文件不是附加数字，而是当前时间。它的构造函数是：TimedRotatingFileHandler( filename [,when [,interval [,backupCount]]])，其中filename参数和backupCount参数和RotatingFileHandler具有相同的意义。interval是时间间隔。when参数是一个字符串。表示时间间隔的单位，不区分大小写。它有以下取值：S 秒、M 分、H 小时、D 天、W 每星期（interval==0时代表星期一）、midnight 每天凌晨
5. logging.handlers.SocketHandler
6. logging.handlers.DatagramHandler：以上两个Handler类似，都是将日志信息发送到网络。不同的是前者使用TCP协议，后者使用UDP协议。它们的构造函数是：Handler(host, port)，其中host是主机名，port是端口名

Handler部分方法：

- setLevel()：跟logger对象里面的setLevel()一样，也是用于设定一个最低分发log信息的级别。为什么有两个setLevel()呢？logger的严重等级用于决定那个级别的log信息可以分发到它的handlers。handler里面的level设置用于控制哪些个log信息是handler需要转寄的。
- setFormatter()：选定一个格式化对象给它自己用。
- addFilter()和removeFilter()分别用于为handler增加一个filter和删除一个filter。
 
### Filter——过滤器
Filters能够用在Loggers和Handlers上，可以实现比level更复杂的过滤。可以直接实例化。通过构造函数参数name可以过滤日志名称。

### Formatter——格式化器
Formatters定义了Logger记录的输出格式，信息格式字符串用%(<dictionary key>)s风格的字符串做替换。

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/20190504201134.png)

## 各对象的关系
Logger是一个树形层级结构：Logger可以包含一个或多个Handler和Filter，即Logger与Handler或Fitler是一对多的关系;一个Logger实例可以新增多个Handler，一个Handler可以新增多个格式化器或多个过滤器，而且日志级别将会继承。

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/20190504192051.png)

## Logging工作流程

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/20190504201426.png)

1. 判断日志的等级是否大于Logger对象的等级，如果大于，则往下执行，否则，流程结束。
2. 产生日志。第一步，判断是否有异常，如果有，则添加异常信息。第二步，处理日志记录方法(如debug，info等)中的占位符，即一般的字符串格式化处理。
3. 使用注册到Logger对象中的Filters进行过滤。如果有多个过滤器，则依次过滤；只要有一个过滤器返回假，则过滤结束，且该日志信息将丢弃，不再处理，而处理流程也至此结束。否则，处理流程往下执行。
4. 在当前Logger对象中查找Handlers，如果找不到任何Handler，则往上到该Logger对象的父Logger中查找；如果找到一个或多个Handler，则依次用Handler来处理日志信息。但在每个Handler处理日志信息过程中，会首先判断日志信息的等级是否大于该Handler的等级，如果大于，则往下执行(由Logger对象进入Handler对象中)，否则，处理流程结束。
5. 执行Handler对象中的filter方法，该方法会依次执行注册到该Handler对象中的Filter。如果有一个Filter判断该日志信息为假，则此后的所有Filter都不再执行，而直接将该日志信息丢弃，处理流程结束。
6. 使用Formatter类格式化最终的输出结果。 注：Formatter同上述第2步的字符串格式化不同，它会添加额外的信息，比如日志产生的时间，产生日志的源代码所在的源文件的路径等等。
7. 真正地输出日志信息(到网络，文件，终端，邮件等)。至于输出到哪个目的地，由Handler的种类来决定。

## logging的使用
### logging 配置
#### 使用默认配置
第一次导入logging模块或使用reload函数重新导入logging模块，logging模块中的代码将被执行，这个过程中将产生logging日志系统的默认配置。

#### dictConfig
python可以通过字典的键值对应参数的方式配置日志。字典配置通过`logging.config.dictConfig(config)` 来实现。配置字典包含以下键：`version`；`formatters`；`filters`；`handlers`；`loggers`；`root`；`disable_existing_loggers`等。

下图是键值实例:

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/20190504202223.png)

#### fileConfig
python可以通过配置文件来定义日志，配置文件的格式通过~logging.config.fileConfig`（此函数基于 `ConfigParser` ）来解析，`[loggers]` , `[handlers]` 和 `[formatters]` ,用来标识文件定义的几种类型的实体，这价格标识必须包含在配置文件中。

- [loggers]标识：关键字 `keys` 后通过 "=" 号指定对应 `logger` 实体的名称，多值用“,”分开；而对具体的logger实体配置就在[logger_logger实体名]标识下进行；
- [handlers]标识：关键字 `keys` 后通过 "=" 号指定对应 `handler` 实体的名称，多值用“,”分开；而对具体的 handler 实体配置就在[handler_handler实体名]标识下进行；
- [formatters]标识：关键字 `keys` 后通过 "=" 号指定对应 `formatter` 实体的名称，多值用“,”分开；而对具体的 formatter 实体配置就在[formatter_formatter实体名]标识下进行；
- [logger_logger实体名]标识：配置具体的logger参数，可能包含以下关键字：
    - level：设置logger过滤log的等级，具体等级见上文；
    - handlers：要添加的handler，具体的handler见上文，多值用“,”分开；
    - propagate：propagete项如果为1，则表示该logger必须处理它继承的父类logger的信息，如果为0则不必；
    - qualname：表示logger的分层结构的上下文名称，应用程序中就是用这个名字结合logger实体名称来获得logger的全路径；
- [handler_handler实体名]：配置具体的handler参数，可能包含以下关键字：
    - class：表示handlers的具体类；
    - level：handler的日志级别；
    - formatter：用于此handler的格式，此处如果不为空必须在[formatters]中定义，如果为空则用默认的formatter(logging._defaultFormatter)；
    - args：表示handler实例化时传给handler构造函数的参数，多个参数用“()”扩起来；
- [formatter_formatter实体名]：配置具体的formatter格式，具体格式说明见上文；

一个实例：

```
[loggers]  
keys=root,traceback

[handlers]
keys=tracebackHandler,infoHandler

[formatters]
keys=simpleFormatter

[logger_traceback]
level=ERROR
handlers=tracebackHandler
qualname=traceback_logger
propagate=0

[logger_root]
level=INFO
handlers=infoHandler
qualname=info_logger
propagate=0

[handler_tracebackHandler]
class=handlers.RotatingFileHandler
level=ERROR
formatter=simpleFormatter
args=("log/error.log", 'a', 10*1024*1024, 100)

[handler_infoHandler]
class=handlers.TimedRotatingFileHandler
level=INFO
formatter=simpleFormatter  
args=("log/info.log" ,'D', 1, 100)

[formatter_simpleFormatter]  
format=%(asctime)s - %(filename)s:%(lineno)s - %(levelname)s - %(message)s
```

### logging 调用


#### 读取日志配置
以文件配置方式为例：读取后 logging 会按照配置文件当中的配置来更新日志配置。

```python
# 在程序的入口读取日志配置文件，并创建logger对象
import logging
logging.config.fileConfig(os.path.join(os.path.dirname(__file__), '../config/log.conf'))
```

#### 创建记录器

```python
# getLogger中的name为配置文件[logger_logger实体名]下的qualname值
tracebackLog = logging.getLogger('traceback_logger')
infoLog      = logging.getLogger('info_logger')
```

#### 记录日志

```
# 对于info，常用于监控程序执行流程
infoLog.info(message)
# 对于error，常和try语句一起使用，此外traceback.format_exc可以返回错误的完整信息
try:
    self.recommandMember(activity)
except Exception as e:
    tracebackLog.error(traceback.format_exc())
```

## 参考
[python日志管理：logging](https://wjdadi-gmail-com.iteye.com/blog/1984354)

[Python日志库logging总结](https://juejin.im/post/5bc2bd3a5188255c94465d31)

[python logging模块使用教程](https://www.jianshu.com/p/feb86c06c4f4)









