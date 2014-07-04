##Python学习笔记二

###类和实例
```
class Student(object):
	pass
```
- 创建实例是通过类名+()实现的 bar=Student()
- 由于类可以起到模板的作用，在创建实例的时候，可以把必须绑定的属性强制写进去：通过定义
一个特殊的__init__方法
```
class Student(object):
	def __init__(sef, name, score):
		self.name = name
		self.score = score
```
- 注意\_\_init\_\_方法的第一个参数必须是self，表示创建的实例本身。
- 如果要让内部属性不被外部访问，可以把属性的名称前面加上\_\_,在Python中变量如果以\_\
开头，就变成了一个私有变量。
- 变量名类似\_\_xxx\_\_的，是特殊变量，特殊变量可以直接访问。
- \_\_开头的变量不是不能从外部访问，而是解释器把\_\_name--->_Student__name,通过这个
名字仍能访问__name变量

###继承
```
class Animal(object):
	def run(self):
		pass

class Dog(Animal):
	pass

class Cat(Animal):
	pass
```
- 当子类和父类有相同的方法时，子类的方法覆盖父类的方法，运行的时候调用子类的方法。

###获取对象信息
- type():基本类型都可以用type(),如果一个变量指向函数或者类，也可以用type()判断。
```
type('abc')==types.StringType
```
- Python把每种type类型都定义好了常量，放在types模块里。
- 所有类型本身的类型就是TypeType

- isinstance(),可以用来判断class的继承关系。
- dir(),要获取一个对象的所有属性和方法可以使用dir(),返回一个包含字符串的list
- hasattr():判断是否有这个属性。getattr():获取一个属性的值。setattr():设置一个属性。
```
hasattr(obj, 'x') #是否属性x
setarrt(obj, 'y', 19) #设置一个属性y
getattr(obj, 'y') #获取属性y
```

###\_\_slots\_\_
```
class Students():
	pass

s = Student()
s.name = 'yp' #动态添加属性

#动态绑定方法
def set_age(self, age):
	self.age = age

from types import MethodType
s.set_age = MethodType(set_age, s, Student)
s.set_age(25)
```
- 但是上面的方法只给s绑定了方法，对其他的实例没有作用。为了给所有实例都绑定方法，可以
给class绑定方法。
```
Student.set_age = MethodType(set_age, None, Student)
```
- \_\_slots\_\_:限制class的属性
```
class Student(object):
	__slots__ = ('name', 'age') #用tuple定义允许绑定的属性
```
- \_\_slots\_\_定义的属性仅对当前的类起作用，对继承的子类不起作用。

###@property
- @property:既能检查参数，又可以用类似属性这样简单的方法来访问变量。

###定制类
```
class Student(object):
	def __init__(self, name):
		self.name = name
	def __str__(self):
		return '....'

# print Student('yp'),但是对s = Student()无效，解决办法是：
	__repr__ = __str__
```
- \_\_iter\_\_:如果一个类想被用于for...in...的循环，该方法返回一个迭代对象。
```
class Fib(object):
	def __init__(self):
		self.a, self.b = 0, 1
	def __iter__(self):
		return self
	def next(self):
		self.a, self.b = self.b, self.a + self.b
		if self.a > 100000:
			raise StopIteration()
		return self.a
```
- 循环遇到StopIteration错误时退出循环。
- \_\_getitem\_\_:把类像list一样使用
```
class Fib(object):
	def __getitem__(self, n):
		a, b = 1, 1
		for x in range(n)
			a, b = b, a + b
		return a
```
- \_\_getattr\_\_:当调用不存在的属性时，解释器调用其来返回一个值。

###metaclass 元类
- metaclass允许创建类和修改类，可以把类看成是metaclass创建出来的实例
```
class ListMetaClass(type):
	def __new__(cls, name, bases, attrs):
		attrs['add'] = lambda self, value: self.append(value)
		return type.__new__(cls, name, bases, attrs)

class MyList(list):
	__metaclass__ = ListMetaClass
```
- \_\_new\_\_方法接收参数的顺序是：1，当前准备的创建的类的对象；2，类的名字；
3，类继承的父类集合；4，类的方法集合。

###错误处理
- try...except...finally
```
try:
	print 'try...'
	r = 10 /0
	print 'result', r
except ZeroDivisionError, e:
	print 'except:', e
else:
	print 'no error'
finally:
	print 'finally...'
print 'END'
```
- 当认为代码可能会出错时，就可以用try来运行这段代码，如果执行出错，则后续代码不会继续执行，
而是直接跳转到错误处理代码，即except块，执行完except后，如果有finally语句块，则执行finally
块。
- 如果没有错误发生，则except块不会被执行.可以在except块后面加上一个else，当没有错误发生时，
会自动执行else语句。
- Python的错误其实是class，所有的错误都继承自BaseException,所以在使用except时需要注意的是
：它不仅捕获该类型的错误，还捕获其子类的错误。

###文件读写
- 打开文件 f = open()
- 读取文件 read(): f.read(), 可以一次读取文件的全部内容，把内容读到内存，
用一个str对象表示。
- 关闭文件 close(),f.close()
```
try:
	f = open('filename', 'r');
	print f.read()
finally:
	if f:
		f.close()
//这样写太麻烦
with open('filename', 'r') as f:
	print f.read()
```
- readline():每次读取一行的内容，readlines()一次读取所有内容并按行返回list。
read(size),每次最多读取size个字节的内容。
- 要读取非ASCII编码的文本，就必须要以二进制模式打开，再解码：
```
f = open('filename', 'rb')
f.read().decode('gbk')

//自动转换编码
with codes.open('filename', 'rb', 'gbk') as f:
	f.read()
```
- 写文件 f.write('...');

###序列化
- 把变量存储到磁盘的过程称为序列化(picking),把磁盘中的内容重新读到内存称为
反序列化(unpicking)
```
import cPickle
f=open('dump.txt', 'wb')
pickle.dump(f, d)//d为一个dict
f.close();
```
- 反序列化
```
/...
d=pickle.load(f)
```

###JSON
- json类型:{}-->dict,[]-->list
```
import json
json.dumps(d)
```
- dumps返回一个str，内容是标准的json，loads()把str反序列化为json。

####Sqlite3
```
import sqlite3
conn=sqlite3.connect('test.db')//如果test不存在，则会在当前目录新建。
cursor=conn.cursor()
cursor.execute('...')
cursor.execute('insert into user (id, name) values(\'1\', \'name\')')
cursor.close()
conn.close()
```
- 使用cursor对象执行insert，update，delete语句时，执行结果由rowcount返回影响的行数。
- 执行select语句时，通过fetchall()可以拿到list结果集，每个元素都是一个tuple，对应一行记录。

###Mysql
```
import mysql.connector
conn=mysql.connector.connect(user='root', password='i123', database='test',
				use_unicode=True)
cursor.execute('insert into user (id, name) values(%s, %s)', ['1', 'name'])
cursor=conn.cursor()
```
- 注意：Mysql的占位符是%s，注意比较与sqlite3的区别。
