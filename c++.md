# 内存操作

## new

`new`在堆区开辟内存，并返回一个指针。堆区的数据有程序员管理开辟，释放。

```c++
int p = new int(10);
```

比如上述代码new返回一个整型类型的指针，指针p指向的值为10。

### 利用new开辟数组

```c++
int *arr = new int[10];//数组有十个元素。
```

## delete

将内存释放

```c++
delete p;//释放一个指针
delete[] arr;//释放数组
```

# 引用

作用：给变量起别名。

语法为：`数据类型 &别名 = 原名`

```c++
int a = 10;
int &b = a;
```

a的值为10，b的值为10，a和b等价，修改b值，a值也会对应修改。但别名并不会占用新的内存，a和b指向同一块内存。

## 引用的注意事项

1.引用必须初始化

2.引用一旦初始化后，就不可以更改。

```c++
int a = 10;
int c = 20;
int &b;//错误的，没有初始化
int &b = a;
&b = c;//错误的，引用不可以更改
b = c;//正确的，这是赋值操作，不是更改引用
```

## 引用作函数参数

作用：函数传参时，可以利用引用的技术让形参修饰实参。

优点：可以简化指针修改实参。

函数参数传递方法：1.值传递 2.地址传递 3.引用传递

```c++
void swap(int &a,int &b)
{
	int tmep = a;
	a = b;
	b = tmep;
}
```

引用传递的效果与地址传递效果是一样的，但引用传递更方便。

## 引用作函数的返回值

1.不要返回局部变量的引用

```c++
int &func()
{
	int a = 10;//局部变量存放在栈区
	return a;
}
int main()
{
	int &b = func();
	
	cout<<"b="<<b<<endl;//第一次结果正确，是因为编译器做了保留
	cout<<"b="<<b<<endl;//第二次结果错误，是因为a的内存已经释放
}
```

2.函数的调用可以作为左值

```c++
int &func()
{
	static int a = 10;//静态变量，存放在全局区，程序结束时释放这块内存
	return a;
}
int main()
{
	int &b = func();
	
	cout<<"b="<<b<<endl;
	cout<<"b="<<b<<endl;
	//两次都会输出10，因为a已经是静态变量了
	
	func() = 100;
	cout<<"b="<<b<<endl;//输出100
}
```

## 引用的本质

本质：引用的本质在c++内部实现是一个指针常量（指针指向不可以修改，但指针指向的值可以改）。

```c++
int a = 10;
int &ref = a;//本质为int *const ref = &a;
ref = 20;//本质为*ref = 20;
```

## 常量引用

作用：常量引用主要用来修饰形参，防止误操作

在函数列表里，可以加`const`修饰形参，防止形参改变实参。

```c++
int a = 10;
int &ref = 10;//错误的，引用必须引一块合法的内存空间
const int &ref = 10;//加上const后，代码的意义为：int temp = 10;const int &ref = temp;
//这样操作后就不可以修改数值，只能读取

void showvalue(const int &a)
{
    a += 1;//错误的，a的值已经不能被更改了，这样可以防止误操作
}
```

# 函数高级

## 函数默认参数

函数参数中形参可以有默认值

```c++
int func(int a = 10)
{
	return a;
}
```

注意事项：

1.如果某个位置有了默认参数，那从这个位置从左往右的参数都必须有默认值。

2.如果函数的声明有了默认参数，那么函数的实现就不能有默认参数。

```c++
int func(int a=10,int b=20)
int main()
{
	...
}
int func(int a,int b)//声明有了默认参数，实现不能有默认参数
{
	...
}
```

## 函数占位参数

函数形参列表里可以有占位参数，调用函数是必须填补该位置。

```c++
void func(int a,int/*占位*/)
{
	cout<<"this is func"<<endl;
}
```

注意：占位参数也可以有默认参数。

## 函数重载

### 函数重载概述

作用：函数名可以相同，提高复用性

满足条件：

- 同一个作用域下
- 函数名称相同
- 函数参数类型不同或者个数不同或者顺序不同

```c++
void func()
{
	cout<<"func1调用"<<endl;
}
void func(int a)
{
	cout<<"func2调用"<<endl;
}
int main()
{
	func();//调用func1
	func(10);//调用func2
}
```

### 函数重载的注意事项

- 引用作为重载条件

```c++
void func(int &a)
{
	cout<<"(int &a)调用"<<endl;
}
void func(const int &a)
{
	cout<<"(const int &a)调用"<<endl;
}
int main()
{
	int a = 10;
	func(a);//调用第一个函数，int &a = 10是不合法的
	func(10);//调用第二个函数,const int &a = 10是合法的
}
```

- 函数重载遇到默认函数

```c++
void func(int a,int b = 10)
{
 	...
}
void func(int a)
{
	...
}
int main()
{
	func(10);//此时编译器不知道调用以上的那个函数，会报错
    func(10,20);//可以调用第一个函数 
}
```

# 类与对象

c++面向对象的三大特性为:封装，继承，多态

## 封装

### 封装的意义

- 将属性和行为作为一个整体，表现生活的事物
- 将属性和行为加以权限控制

设计一个类时，将属性和行为写在一起，语法：`class 类的名字 { 属性: 行为};`

属性有三个类型:`public`，`protected`，`private`。三种属性都可以在类内访问，其中只要`public`可以被类外访问，`protected`和`private`则不能。

### 权限

public（公共权限）类内可以访问，类外也可以访问

private（私有权限）类内可以访问，类外不可以访问，子类不可以访问

protected（保护权限）类内可以访问，类外不可以访问，子类可以访问

### struct与class的区别

在c++中两者唯一的区别是默认的权限不同。

- class的默认权限是私有
- struct的默认权限是公共

### 成员属性设置为私有

优点有：

- 将所有成员属性设置为私有，可以自己控制读写权限
- 对于写权限，可以检测数据的有效性

可以在public权限里实现几个函数从而控制成员的读写权限

```c++
class Student
{
	private:
	string name;
	int age;
	int ID;
	public:
	string gerName()
	{
		return name;
	}//读的权限
	void setAge(int a)
	{
		age = a;
	}//写的权限

};
```

## 对象的初始化和清理

创建的对象应该有初始化设置以及对象销毁前清理数据的设置。

### 构造函数与析构函数

构造函数用于初始化，析构函数用于清理。

这两个函数对编译器自动调用，完成对象的初始化和清理工作。若果我们不提供构造函数和析构函数，编译器会提供编译器提供的构造函数和析构函数的空实现。

- 构造函数：主要作用在与创建对象时为对象的成员属性赋值，构造函数有编译器自动调用，无需手动调用。
- 析构函数：主要作用在于对象**销毁前**系统自动调用，只写一些清理工作。

构造函数语法：`类名(){}`

1. 构造函数，没有返回值夜不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象时候会自动调用构造，无需手动调用，而且只会调用一次

析构函数语法：`~类名(){}`

1. 析构函数，没有返回值也不写void
2. 函数名称与类名相同，但要在最前面加一个符号`~`
3. 析构函数没有参数，因此不可以发生重载
4. 程序会在对象销毁前自动调用析构函数，无需手动调用，而却只会调用一次

### 构造函数的分类及调用

分类：

按参数分为：有参构造和无参构造（默认构造）

按类型分为：普通构造和拷贝构造

三种调用方式：括号法，显示法，隐式转换法

这里着重说明拷贝构造：

```c++
class Person
{
private：
	int age;
public:
	//拷贝构造函数
	Person(const Person &p)
	{
		//将传入对象的所有属性复制在这个对象中
		age = p.age;	
	}
    Person(int a)
    {
        age = a;
    }
};
```

调用方式：

```c++
int main()
{
	//括号法：
	Person p1(10);//有参构造
	Person p2(p1)//拷贝构造
	Person p3;//默认构造
	//显示法：
    Person p4 = Person(10);
    person p5 = Person(p1);
    Person(10);//匿名对象，特点：当前行执行结束，系统会立即回收匿名对象
    //隐式转换法
    Person p6 = 10;//相当于 person p4 = Person(10);
}
```

注意事项：

1. 调用默认构造时，不要加()
2. 不要利用拷贝构造函数来初始化匿名对象

```
Person(p3);//错误的，编译器会认为Person(p3)===Person p3
```

###  拷贝构造函数的调用时机

使用拷贝构造函数的情况：

- 使用一个已经创建完毕的对象来初始化一个新对象
- 值传递的方式是给函数参数传值
- 以值方式返回局部对象

```c++
Person func()
{
	Person p1;
	return p1;
}
```

### 构造函数的调用规则

c++编译器至少会给一个类添加3个函数

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造函数，对属性进行值拷贝

构造函数的调用规则如下（没有函数重载的情况下）：

- 如果用户定义有参构造函数，c++不再提供默认无参构造，但是会提供默认拷贝构造
- 如果用户定义拷贝构造函数，c++不会再提供其他构造函数

### 深拷贝和浅拷贝

浅拷贝：简单的赋值拷贝（逐字节的）

深拷贝：在堆区重新申请空间，进行拷贝操作

```c++
class Person
{
private:
	int m_age;
	int *m_height;
public:
	//自己实现拷贝构造函数老解决浅拷贝的问题,深拷贝
	Person(const Person &p)
	{
		m_age = p.m_age;
		m_height = new int(*p.m_height);
	}
	//浅拷贝带来的问题
	Person(const Person &p)
	{
		m_age = p.m_age;
		m_height = p.m_height;//两个指针指向同一块内存空间
	}
	//需要在析构函数把在堆区开辟的数据存释放干净
	~Person()
	{
		if(m_height!=NULL)
		{
			delete m_height;
			m_height = NULL;
		}
	}
};
```

### 初始化列表

作用：c++提供初始化列表语法，用来初始化属性（便捷）

语法：`构造函数():属性1(值1)，属性2(值2),...`

### 成员对象作为类成员

c++类中成员可以是另一个类的对象，把该成员称为**对象成员**

```c++
class A{...};
class B
{
	A a;//对象成员
};
```

注意事项：有其他类对象作为本类的成员，构造时候先构造对象，在构造自身。故而析构的顺序是先自身再对象。

## 静态成员

在成员变量和成员函数加上关键字`static`，称为静态成员

- 静态成员变量
  - 所有对象共享同一份数据
  - 在编译阶段分配内存
  - 类内声明，类外初始化
- 静态成员函数
  -  所有对象共享同一个函数
  - 静态成员函数只能访问静态成员变量

```c++
class Person
{
	static int m_A;//类内声明
};
Person::m_A = 100;//类外初始化
```

可以通过对象进行访问，也可以通过类名进行访问，静态函数成员的也是可以这样访问的。

```c++
p.m_A;
Person::m_A;
```

静态成员变量也可以有属性。

## c++对象模型与this指针

### 成员变量和成员函数分开储存

```c++
class Person
{
	
};
int main()
{
	Person p;//空对象占一个字节
	//c++编译器会个空对象分配一个字节的内存空间，是为了区分空对象占内存的位置。
	
}
```

```c++
class Person
{
	int a;//非静态成员变量，属于类的对象上
	static int b;//静态成员变量，不属于类的对象上
	void func01(){}//非静态成员函数，不属于类的对象上
	static void func02(){}//静态成员函数，不属于类的对象上
};
int main()
{
	Person p;//对象占4个字节
}
```

### this指针概念

作用：区分是哪个对象调用函数自己的

c++提供特殊的对象指针，this指针指向被调用的成员函数所属的对象。

this指针是隐含每个非静态成员函数内的一种指针，this指针不需要定义，可直接使用。

this指针的用途：

- 当形参与成员变量同名时，可用this指针区分
- 在类的非静态函数返回对象本身，可用`return *this`

```c++
class Person
{
	int age;
	void ageAdd()//如果想在main函数中这样使用，这样是不行的
	{
		age += 10;
	}
	Person &ageAdd()//&符号很重要，如果不用引用返回，将返回一个新对象。
	{
		age += 10;
        return *this;
	}
};
int main()
{
	Person p;
	p.ageAdd().ageAdd().ageAdd();//链式编程
}
```

### 空指针访问成员函数

c++中空指针也是可以调用成员函数的，但是也要注意有没有用到this指针

如果用到this指针，需要加以判断保证代码的健壮性

```c++
class Person
{
	public:
	void func1()
	{
		if(this==NULL)//判断是否为空指针，否则执行函数最后语句会报错
		{
			return;
		}
		cout<<"age="<<M_age<<endl;
	}
	int M_age;
}
int main()
{
	Person *p=NULL;//成员空指针
	p->func1();
}
```

### const修饰成员函数

**常函数**：

- 成员函数后加`const`后为**常函数**
- 常函数不可以修改成员属性
- 成员属性声明时加关键字`mutable`后，在常函数依然可以修改

**常对象**：

- 声明对象前加`const`为常对象
- 常对象只能调用常函数

常函数示例：

```c++
class Person
{
	public:
	//this指针的本质是指针常量，指针的指向是不可以修改的
	//const Person *const this
	//成员函数后面加const，修饰的是this的指向，让指针指向的值也不可以修改
	void func1() const//常函数的创建方式
	{
		this->m_A=100;//错误的，无法修改
		this->m_B=100;//可以的
	}
	int m_A;
	mutable int m_B;//特殊变量，即使在常函数中，也可以修改这个值，不过要加关键字mutable
}
```

常对象示例：

```c++
class Person
{
	...//借用上面的定义
}
int main()
{
	const Person p;
	p.m_A=100;//错误的，不能修改
	p.m_B=100;//可以的，特殊变量可以修改
}
```

## 友元

在程序里，有些私有属性想要类外的一些函数或者类进行访问，就需要用到友元。所以友元的目的是让一个函数或者类访问另一个类中私有成员

友元的关键字为`friend`

友元的三种实现：

- 全局函数做友元
- 类做友元
- 成员函数做友元

### 全局函数作友元

```c++
class Person
{
	friend void func(Person &p);//若没有这行代码，函数将无法访问类中私有成员
	//friend的作用是告诉编译器这个函数可以访问类中的私有内容
public:
	Person()
	{
		m_A = 10;
		m_B = 20;
	}
	int m_A;
private:
	int m_B;
};
//全局函数
void func()
{
	cout<<m_A<<endl;
	cout<<m_B<<endl;//这行
}
```

### 类作友元

与上面代码相似，只不过函数变成了类。

### 成员函数作友元

```c++
class Person
{	
	...
	public:
	void func();
};
class Phone
{
	friend void Person::func();//这样使用
}
```

## 运算符重载

运算符重载概念：对已有的运算符进行定义，赋予其另一种功能，以适应不同的数据类型

### 加号运算符重载

作用：实现两个自定义数据类型相加的运算

```C++
class Person
{
	public:
	int m_A = 10;
	int m_B = 10;
	//成员函数重载+号,其本质为 Person p3 = p1.operator+(p2)
	Person operator+(Person &p)
	{
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}
};
//通过全局函数重载+号，其本质为 Person p3 = operator+ (p1,p2)
Person operator+(Person &p1,Person &p2)
{
	Person temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;
}
```

运算符重载也可以进行函数重载

```c++
Pesron p4 = p1 + 10;//这种操作可以用函数重载实现
//重载（成员函数重载）为
Person operator+(int num)
{
	Person temp;
	temp.m_A = this->m_A + num;
	temp.m_B = this->m_B + num;
	return temp;
}
//全局函数重载
Person operator+(Person &p1,int num)
{
	Person temp;
	temp.m_A = this->m_A + num;
	temp.m_B = this->m_B + num;
	return temp;
}
```

总结：

1. 对于内置的数据类型的表达式的运算符是不可能改变的
2. 不要滥用重载运算符

### 左移运算符重载

作用：可以输出自定义的数据类型

```c++
Person p;
cout<<p<<endl;//左移运算符的作用
```

语法规则与加号运算符相似

```c++
//成员函数重载左移运算符 p.operator<<(cout) 简化版本为 p<<cout
//一般不会利用成员函数重载<<运算符
viod operator<<(cout)
{
	
}
//全局函数重载左移运算符
ostream &operator<<(ostream &cout,Person &p)//链式编程，使输出返回值为标准输出流，可以向后追加输出内容
{
	cout<<"m_A:"<<m_A<<"m_B:"<<m_B<<endl;
    return cout;
}
```

### 递增运算符重载

