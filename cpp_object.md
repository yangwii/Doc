## 反思C++面向对象和虚函数

- 二进制兼容性，如果以share library方式提供函数库，那么头文件和库文件不能轻易修改，否则容易破坏已有的
二进制可执行文件，或者其他用到这个library的库文件。
- 库文件的升级：1,使用新库的可执行程序，重新编译。2,直接替换现有的库文件。
- 虚函数的调用方式，通常是vptr/vtbl机制，然后用vtbl[offset]来调用。
- 虚函数的决议是靠偏移量，并不是靠符号名。因此 virtual foo(int) --> virtual foo(double),编译可以通过，
但是运行会出错。
- class增加新的数据成员，造成sizeof()变大，以及内部数据成员的offset变化，这可能是不安全的，但是书中的
解释我没明白！！！fuck
- 反面教材：COM，在C++中以虚函数作为接口基本上不能二进制兼容，这样的接口一旦发布，无法修改。
- Windows下，Debug与Relese生成的库是不一样的，二进制不兼容，不能混用。
- 二进制兼容的解决：1,采用静态链接，这里不是用静态库(.a),而是完全从源码编译。2,通过动态库的版本管理来
实现。
- 虚函数作为接口，若要在类中增加新的虚函数，为了避免二进制不兼容，需要将新增的函数放在类的末尾。但是如果
这个类被继承，那么便会更改派生类中的vtable offset，同样不兼容。
- 动态库的推荐做法：1,使用范围窄，做好版本的管理就行了。
- 2,使用范围广，1）暴露的接口里不要有虚函数，要显示声明构造函数，析构函数，并且不能inline。2),
在库的实现中把调用转发，这部分代码位于.so/.dll中，随库一起升级。3），要加入新的功能，不必通过继承来
扩展，可以原地修改，而且容易保持二进制兼容性。
- 使用no-virtual函数比virtual更健壮：因为virtual是bind-by-offset，而no-vitrual是bind-by-name。

- boost::function就像C#里的delegate，可以指向任何函数，包括成员函数，当用bind把某个成员函数绑定到某个
对象上时，可以得到一个闭包。用法如下
		class Foo{
				public:
				void methodA();
				void methodInt(int a);
				void methodString(const string& str);
		};

		class Bar{
				public:
				void methodB();
		}

		boost::function<void()> f1;//无参数，无返回值
		Foo foo;
		f1 = boost::bind(&Foo::methodA, &foo);
		f1();

		Bar bar;
		f1 = boost::bind(&Foo::methodB, &bar);
		f1();

		f1 = boost::bind(&Foo::methodInt, &foo, 42);//42参数
		f1();

		f1 = boost::bind(&Foo::methodString, &foo, "hello");
		f1();

		boost::function<void(int)> f2;//返回值为int类型
		f2 = boost::bind(&Foo::methodInt, &foo, _1);
		f2(53);
