---
title: 'CPP 期末总结'
date: 2018-12-18
published: true
slug: cpp-summary
tags: ['C++']
cover_image: "./images/substats.png"
canonical_url: false
description: '大一期末考试总结'
---

# 1.0 基础知识

## 1.1：

编写c++程序一般经过**编辑，编译，连接，运行**四步。
编辑：将**c++源程序输入**计算机的过程，文件名为**cpp**。
编译：将cpp文件**翻译成机器语言**，文件名为**obj**。
连接：**分配内存地址**后，转换成可执行程序，文件名为**exe**。
运行：**执行exe文件**，将结果显示到屏幕上。

**1.2：**

函数的返回值类型在定义之时就以决定。

**1.3：**

基类的公有成员采用私有继承时，在派生类中会变成私有成员。

**1.4：**

数据封装：将数据与操作封装到一起形成实体，这个实体也就是类。
类也就是数据与操作的组合体，数据是类的静态特征，操作是类的动态特征。

**1.5：**

派生类是基类的扩展和延伸，派生类一个来自本体，一个来自基类。

**1.6：**

this指针可以保证每个对象都拥有自己的数据成员，可以共享处理这些数据的代码。

**1.7：**

多态分为静态多态和动态多态，静态多态是由于参数的不同调用同名函数，动态多态是由于对象的不同调用同名函数。多态的对象都是同名函数。

**1.8：**

内联函数代码量少，执行效率高，可以被频繁调用。也就是inline。

**1.9：**

派生类的调用顺序是先调用基类构造函数，调用子对象，在调用派生类。析构函数反之。

**1.10**

后置i++先赋值再自增，前置反之。

**1.11**

自身类对象不能作为类的成员，循环定义。

**1.12**

输出流的四种流 cin/cout/cerr/clog

**1.13**

类是对象的抽象，对象是类的实例。
引用声明要初始化，指向存在的对象，初始化后就不能指向其他对象。

**带默认形参值的函数**

由默认值的形参必须在右边。
又默认值的形参右不能出现无默认值的形参。
因为在函数调用时是按从左至右的顺序建立对应关系的。
不能重复定义


## 2.0函数例题

**八位二进制转十进制(教材习题3_2.cpp)**

```cpp{3,5-7}
#include<iostream>
using namespace std;
double power(double x,int n)
{
	double val=1.0,i;
	for(i=0;i<n;i++)
		val*=x;
	return val;
}
int main() 
{
	int value=0;
	cout<<"Enter an 8 bit binary number:";
	for(int i=7;i>=0;i--)
	{
		char ch;
		cin>>ch;
		if(ch=='1')
			value+=static_cast<int>(power(2,i));//强制转换，把power（2，i）的结果由double型转换成int型。
	}
	cout<<"Decimal value is"<<value<<endl;
	return 0;
}
```

**编写程序求π值(教材习题3_3.cpp)**

arctanx=x-x*x*x/3+x*x*x*x*x/5-.....

```cpp
#include<iostream>
using namespace std;
double arctan(double x)
{
	double sqr=x*x;
	double e=x;
	double r=0;
	int i=1;
	while(e/i>1e-5){
		double f=e/i;
		r=(i%4==1)?r+f:r-f;//判断i%4的结果是否等于1，若是1则r+f若不是1则r-f；
		e=e*sqr;
		i+=2;
	}
	return r；//注意
}
int main() {
	double a=16.0*arctan(1/5.0);//整数相除结果取整若是1/5结果为0；
	double b=4.0*arctan(1/239.0);
	cout<<"PI="<<a-b<<endl;
	return 0;
}
```

**寻找回文数(教材习题3_4.cpp)**

思路：除10取余，取出每一位的数字。数字反置，低位充当高位，按反序构成新的数，与原数比较。

```cpp
#include<iostream>
using namespace std;
bool symm(unsigned n)//判断是否为回文数
{
	unsigned i=n;
	unsigned m=0;
	**while(i>0)
	{
		m=m*10+i%10;
		i/=10;
	}**
	return m==n;//函数为bool类型，所以返回值类型也是bool类型，如果m==n为返回true，反之为false。
	}
int main() 
{
	for(unsigned m=11;m<1000;m++)
	if(symm(m)&&symm(m*m)&&symm(m*m*m))
	{
		cout<<"m="<<m;
		cout<<"m*m="<<m*m;
		cout<<"m*m*m="<<m*m*m<<endl;
	}
	return 0;
}
```

 **教材习题3_5.cpp**

```cpp
#include<iostream>
#include<cmath>
using namespace std;
const double TINY_VALUE=1e-10;//定义一个常量
double tsin(double x)//实现sin函数
{
	double g=0;
	double t=x;
	int n=1;
	do{
		g+=t;
		n++;
		t=-t*x*x/(2*n-1)/(2*n-2);//阶乘的实现
	}while(fabs(t)>=TINY_VALUE);
	return g;
}
int main() 
{
	double k,r,s;
	cout<<"r=";
	cin>>r;
	cout<<"s=";
	cin>>s;
	if(r*r<=s*s)
		k=sqrt(tsin(r)*tsin(r)+tsin(s)*tsin(s));
	else
		k=tsin(r*s)/2;
	cout<<k<<endl;
	return 0;
}

```

**总结：** 

const优点
1定义一个常量，不可变性。
2便于检查，消除隐患。
3总控制
4节省空间
5提高效率，编译器不提供内存空间，而是将它保存在符号表中。

 **例3-7**

```cpp
#include<iostream>
using namespace std;
int fu2(int m)//fu1 fu2 顺序不能反。
{
	return m*m;
}
int fu1(int x,int y)
{
	return fu2(x)+fu2(y);
}

int main()
{
	int a,b;
	cin>>a>>b;
	cout<<fu1(a,b)<<endl;
	return 0;
}
```

**例3-8**

```cpp
#include<iostream>
using namespace std;
unsigned fac(unsigned n)
{
	unsigned f;
		if(n==0)
			f=1;
		else
			f=fac(n-1)*n;//循环再次进入fac 函数
		return f;
}
int main() 
{
	int a;
	cin>>a;
	cout<<fac(a)<<endl;
	return 0;
}

```

**例3-9**

```cpp
#include<iostream>
using namespace std;
int comm(int n,int k)
{
	if(k>n)
		return 0;
	else if(n==k||k==0)
		return 1;
	else 
		return comm(n-1,k)+comm(n-1,k-1);//分好类
}
int main()
{
	int n,k;
	cout<<"Please enter two integers n and k:";
	cin>>n>>k;
	cout<<"C(n,k)="<<comm(n,k)<<endl;
	return 0;
}
```

**值传递与引用传递**

```cpp
void swap（int a，int b）
```
```cpp
void swap （int &a，int &b）
```
形参写法不同，效果虽同，但差异很大。

引用声明要初始化，指向存在的对象，初始化后就不能指向其他对象。
## 3.0内联函数

**3.1**内联函数不是在调用时发生控制转移，而是在编译时将函数体嵌入在每一个调用处。

**3.2**节省参数传递，控制转移开销。inline 只是一个要求，没有inline，在现代编译器中也可能被认为是内联函数。

**4.0带默认形参值的函数**

**4.1**由默认值的形参必须在右边。

**4.2**又默认值的形参右不能出现无默认值的形参。

**4.3**因为在函数调用时是按从左至右的顺序建立对应关系的。

**4.4**不能重复定义

## 闰年

```
	if((a%4==0&&a%100!=0)||(a%400==0));
```

## switch语句的用法

```cpp
switch(day)
{
case 0:
	cout<<" "<<endl;break;
case 1：
	cout<<" "<<endl;break;	
}
```
## while do/while for语句的用法

## 数字反转

```cpp
#include<iostream>
using namespace std;
int main()
{
	int n,a,b;
	cin>>n;
	do
	{
		a=n%10;
		cout<<a;
		n/=10;
	}while(n!=0);
	cout<<sum;
	return 0;
}
```
求数字的因子
因子：从1到n，凡是可以整除n的数字均为n的因子
```cpp
#include<iostream>
using namespace std;
int main()
{
	int n,a,b;
	cin>>n;
	for(int i=1;i<=n;i++)//遍历n
	{
		if(n%i==0)
			cout<<i<<" ";
	}
	cout<<endl;
	return 0;
}
```
九九乘法表

```cpp
#include<iostream>
using namespace std;
int main()
{
	for(int i=1;i<10;i++)
	{
		for(int j=1;j<=i;j++)
		{
			cout<<j<<"*"<<i<<"="<<i*j<<" ";
		}
			cout<<endl;
	}
	return 0;
}

```
8-1

```cpp
#include <iostream>
using namespace std;
class co{
public:
	co (double r=0.0,double i=0.0)
	{
		a=r;
		b=i;
	}
	co operator+(const co &c2) const{
	return co(a+c2.a,b+c2.b);
};
	co operator-(const co &c2) const{
	return co(a-c2.a,b-c2.b);
};
	void display() const{
	cout<<"("<<a<<","<<b<<")"<<endl;
};
private:
	double a;
	double b;
};
/*co co::operator+(const co &c2) const{
	return co(a+c2.a,b+c2.b);
}
co co::operator-(const co &c2) const{
	return co(a-c2.a,b-c2.b);
}
void co::display() const{
	cout<<"("<<a<<","<<b<<")"<<endl;
}*/
int main()
{
	co c1(5,4),c2(2,10),c3;
	c1.display();
	c2.display();
	c3=c1-c2;
	c3.display();
	c3=c1+c2;
	c3.display();
	return 0;
}

```

```cpp
#include <iostream>
using namespace std;
class co{
public:
	co (double r=0.0,double i=0.0)
	{
		a=r;
		b=i;
	}
	co operator+(const co &c2) const;
	co operator-(const co &c2) const;
	void display() const;
private:
	double a;
	double b;
};
co co::operator+(const co &c2) const{
	return co(a+c2.a,b+c2.b);
}
co co::operator-(const co &c2) const{
	return co(a-c2.a,b-c2.b);
}
void co::display() const{
	cout<<"("<<a<<","<<b<<")"<<endl;
}
int main()
{
	co c1(5,4),c2(2,10),c3;
	c1.display();
	c2.display();
	c3=c1-c2;
	c3.display();
	c3=c1+c2;
	c3.display();
	return 0;
}
```

8-4

```cpp
#include<iostream>
using namespace std;
class base1{
public:
	virtual void display() const;
};
void base1::display()const{
	cout<<"base::display()"<<endl;
}
class base2:public base1{
public:
	void display()const;
};
void base2::display()const{
	cout<<"base2::display()"<<endl;
}
class derived:public base2{
public:
	void display()const;
};
void derived::display()const{
	cout<<"derived::display()"<<endl;
} 
void fun(base1*ptr)
{
	ptr->display();
}
int main()
{
	base1 base1;
	base2 base2;
	derived derived;
	fun(&base1);
	fun(&base2);
	fun(&derived);
	return 0;
}

```
9-1函数模板

```cpp
#include<iostream>
using namespace std;
template<class t>
void qq(t *a,int i)
{
	for(int j=0;j<i;j++)
		cout<<a[j]<<" ";
	cout<<endl;
}
int main()
{
	int a[5]={0,1,2,3,4};
	double b[5]={0.5,0.1,0.2,0.3,0.4};
	qq(a,5);
	qq(b,5);
	return 0;
}

```

```cpp
#include<iostream>
using namespace std;
template<class t>
t abs(t x)
{
	return x<0?-x,:x;
}
int main()
{
	int i;
	cin>>i;
	cout<<abs(i)<<endl;
	return 0;
}

```
8-5

```cpp
#include<iostream>
using namespace std;
class base
{
public:
	virtual~base();
};
base::~base()
{
	cout<<"base destructor"<<endl;
}
class derived:public base
{
public:
	derived();
	~derived();
private:
	int *p;
};
derived::derived(){
	p=new int(0);
}
derievd::~derievd(){
			cout<<"derievd destructor"<<endl;
			delete p;
}
		void fun(base*b)
		{
			delete b;
		}
		int main()
		{
			base*b=new derived();
			fun(b);
			return 0;
		}
```

## 7-5

```cpp
#include<iostream>
using namespace std;
class base1
{
public:
	base1(int i){}
	~base1(){}
};
class base2
{
public:
	base2(int j){}
	~base2(){}
};
class base3
{
public:
	base3(){}
	~base3(){}
};
class derived:public base2,public base1,public base3{
public:
	derived(int a,int b,int c,int d):base1(a),member2(d),member1(c),base2(b){}
private:
	base1 member1;
	base2 member2;
	base3 member3;
};
int main()
{
	derived obj(1,2,3,4);
	return 0;
}
```
摸球问题
```cpp
#include <iostream>
using namespace std;
int main()
{
int i,j,k;
int sum;
sum=0;
for(i=1;i<5;i++)
for(j=1;j<5;j++)
for(k=1;k<5;k++)
{
if(i!=j && j!=k &&i!=k)
{sum=sum+1;}
}
cout<<sum;
}
```