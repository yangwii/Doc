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
