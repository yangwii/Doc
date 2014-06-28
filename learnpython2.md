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

##\_\_slots\_\_
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
