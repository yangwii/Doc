## 学习Python笔记
- 输出

>>>print 'hello, world',print语句也可以跟上多个字符串，用逗号','隔开
>>>print 'hello,', 'world'.每遇到一个逗号会输出一个空格代替。
>>>print '100+200=', 100+200

- 输出
raw_input(),可以让用户输入字符串，并存放到一个变量，name=raw_input()
增加提示信息，name=raw_input("Please input name:");

### Python基础
- 注释以#开始，其他每一行都是一个语句，当语句以：结尾时，缩进的语句视为代码块。
#### 数据类型
- 字符串，如果字符串里有很多字符都需要转义，可以使用r''.如果内部有很多换行，用\n写在
一行不好读，为了简化可以用'''...'''的格式。
>>> print '''line1
... line2
... line3'''
写成程序就是：
print '''line1
line2
line3'''
- 字符串的值是写时更改。

###字符编码
- ASCII使用一个字节，为了统一编码出现了unicode,unicode通常是两个字节
- 可变长编码UTF-8,可以把一个Unicode字符根据不同的数字大小编成1-6个字节，英文字母一个字节
汉字通常三个字节。
- 在计算机内存中使用Unicode编码，当保存到磁盘时，转换为UTF-8.
- ord(), chr()可以把数字和字符转换。
- 若要使python支持Unicode，可以使用u''.
- u'ABC'.encode('utf-8'),转化为utf-8编码
- len(),返回字符串长度。

###list[]，tuple()

- list是一种有序集合，可以随时添加和删除。用len()可以获得list的长度。用-1索引获得最后一
个元素。
- insert(1, 'jack'),在1的位置插入jack，删除末尾的元素使用pop()。删除指定位置的元素使用
pop(i).
- 向list末尾添加元素使用list.append('jack');
- list里面的元素可以类型不同。

- tuple，元祖和list类似，但是tuple一旦初始化就不能修改。
- 注意定义只有一个元素的tuple时候，需要使用(1,)格式，在元素后面加一个逗号。防止歧义的
发生。
- range()生成整数序列。range(5),依次生成0,1,2,3,4。 

###dict{}&set()
- 判断key是否存在，1：通过in，‘Thomas’ in dict.2:dict.get('Thomas')
- 删除一个key，用pop(key)的方法。
- set是key的集合，但不存储value，并且key不重复。
- 创建一个set时，需要提供一个list作为输入集合。s = set([1,2,3])
- 通过add(key)添加元素到set中，通过remove(key)方法删除元素。
- set可以看做是数学意义上的无序和无重复集合，可以做并集，交集运算。

- list是可变对象，str是不可变对象。对于不可变对象来说，调用对象自身的任意方法，也不会
改变对象自身的内容，相反，这些方法会创建新的对象并返回，这样，就保证了不可变对象本身永远
是不可变的。

###函数
- 函数执行完毕也没有return语句时，自动return None
- 空函数，如果想定义一个什么事都也不用做的空函数，可以用pass语句
```
def nop():
	pass
```
pass语句什么都不做，相当于占位符。
- 参数检查：数据类型检查用内置函数isinstance()实现：
```
if not isinstance(x, (int, float)):
	raise TypeError('bad type')
```
- 返回多个值：函数可以返回多个值，但其实是一个tuple。
- 默认参数必须指向不可变对象。
- 可变参数
```
def cal(*nums):
	for n in nums:
```
- 关键字参数：可变参数允许你传入0个或者人一个参数，这些可变参数在函数内部组装成一个tuple
，而关键字参数允许你传入0个或者任意个含参数名的参数，这些关键字在函数内部组装成一个dict
- 尾递归：是指，在函数返回的时候，调用函数本身，并且return语句不能包含表达式，这样递归
无论本身调用多少次，都只占用一个栈帧。

###切片
- 取list前三个元素，L[0:3]
- 取list后面到末尾的元素，L[-2:]

###列表生成式[x*x for x in range(1,11)]
- 写列表生成式时，把要生成的x*x放到前面，后跟for循环，就可以把list创建出来。
- for后面还可以加上if判断 [x&x for x in range(1,11) if x%2 == 0]

- 还可以使用两层循环，可以生成全排列 [m+n for m in 'ABC' for n in 'XYZ']
- for循环其实可以同时使用两个甚至多个变量，比如dict的iteritems()可以同时迭代key和value。

###生成器()
- 列表生成式和生成器的区别仅是最外层的[]和()
- 取generator的元素使用next()方法。

###传入函数&高级函数
- map()函数接收两个参数，一个是函数，一个是序列，map将传入的函数依次作用到序列的每个元素
，并把结果作为新的list返回。
- reduce()函数：接收一个函数和一个序列，这个函数必须接收两个参数，reduce把结果继续和序列
的下一个元素做积累计算。

###匿名函数
- map(lambda x:x*x, [1,2,3,4,5]),关键字lambda表示匿名函数，冒号前面的x表示函数参数。
- 匿名函数有一个限制，就是只有一个表达式，不用写return。

###装饰器
- 由于函数也是一个对象，而且函数对象可以被赋值给变量，所以通过变量也可以调用函数。
- 函数对象有一个__name__属性，可以得到函数的名字。
- 在代码运行期间动态增加功能的方式，称为“装饰器”。本质上装饰器就是一个返回函数的高阶函数
```
def log(func):
	def wrapper(*args, **kw):
		print 'call %s():'%func.__name__
		func(*argx, **kw)
	return wrapper

@log
def now();
	print '2014-6-27'
```
- 把@log放到now函数的定义处，相当于执行了语句：now=log(now)
- 如果装饰器本身需要传入参数，那就需要编写一个返回装饰器的高阶函数：
```
def log(txt):
	def decorator(func):
		def wrapper(*args,**kw):
			print '%s %s():'%(txt, func__name__)
			return func(*args,**kw)
		return wraper
	return decorator

@log('execute')
def now():
		print '2014-6-27'
```
- 和前面的两层嵌套相比，三层相当于now=log('execute').now(),首先执行log('execute'),返回
的是decorator函数，再调用返回的函数，参数是now函数，返回值最终是wrapper函数。
- 以上两种decorator的定义都没有问题，但因为函数也是对象，有__name__对象，但是经过
decorator装饰后的函数，他们的__name__已经变成了'wrapper'.
- 不需要写复杂的代码，Python内置的functools.wraps就是干这个事的，所以一个完整的decorator
如下：
```
import functools

def log(func):
	@functools.wraps(func)
	def wrapper(*args, **kw):
		print 'call %s():' %func.__name__
		return func(*args, **kw)
	return wrapper
```

###偏函数
- functools.partial帮助我们创建一个偏函数 int2=functools.partial(int, base=2)
- functools.partial的作用就是把一个函数的某些参数给固定住，返回一个新的函数，调用这个
新的函数会更简单。

###模块
- sys模块，import sys后，就有了sys变量，利用sys变量，就可以访问sys模块的所有功能
- sys模块有一个argv变量，用list存储了命令行的所有参数，至少有一个参数，同C/C++。
- 当在命令行运行模块文件时，Python解释器把__name__置为__main__
- 类似_xxx或者__xxx这样的函数或变量就是非公开的，不应该被直接引用。
