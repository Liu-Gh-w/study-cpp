## 顶层const

==const:首先作用于左边，如果左边没东西，就作用于右边==

int ==*== const:修饰左边的指针，顶层const,指针指向不可改，指向的值可以改

const ==int==*:修饰右边的int类型，底层const，指针指向可以改，指向的值不能改

## constexpr和常量表达式

目前的理解是，constexpr类型数据或者函数，保证在编译器就得到结果，提高程序执行效率，在定义指针时，所定义的就是顶层const。



![image-20240426105210559](assets/image-20240426105210559-1714222241605-2.png)

![image-20240426105227815](assets/image-20240426105227815.png)

![image-20240426105450870](assets/image-20240426105450870.png)

## 指针，常量与类型别名

![image-20240426111754719](assets/image-20240426111754719.png)

## auto类型说明符

### auto会根据右值推测左值

```c++
int i = 0;
auto j = i;//那么j就是int类型
```

### auto在一条语句中声明多个变量，是同一数据类型

```C
auto a = 0,*p = &a;//a是整形，p是int*
```

### 复合类型，常量和auto

使用auto时，会忽略顶层const,保留其底层const

```c
int i = 0, &r = i;
	auto a = r;   // a是一个整数（r是i的别名，而i是以一个整数）
 
	const int ci = i, &cr = ci;//ci就是顶层const
	auto b = ci; // b是一个整数（ci的顶层const特性被忽略掉了）
	auto c = cr; // c是一个整数（cr是ci的别名，ci本身是一个顶层const）
	auto d = &i; // d是一个整型指针（整数的地址就是指向整数的指针）
	auto e = &ci; // e是一个指向整数常量(const int*)的指针（对常量对象去地址是一种底层const）
 
	const auto f = ci; // ci的推演类型是int，f是const int
	auto &g = ci; // g是一个整型常量引用，绑定到ci
	auto *p = &ci // p是const int *，不知道为什么没有忽略顶层const，原来以为p是int*;


```

## decltype类型指示符

适用于:希望从表达式的类型推出要定义的变量的类型，但是不想用该表达式的值初始化变量。

```c++
decltype(func()) sum;//decltype返回func函数的数据类型。
const int ci = 0,&cj = ci;
decltype(ci) x = 0;//x:const int
decltype(cj) y = x;//y:const int &,y绑定到变量x
decltype(cj) z;//error,z是引用，必须初始化
```

decltype不忽略顶层const

![image-20240426194253484](assets/image-20240426194253484.png)

关键在r与r+0的区别

![image-20240426194342742](assets/image-20240426194342742.png)

![image-20240426194443781](assets/image-20240426194443781.png)

## 直接初始化和拷贝初始化

![image-20240426195922857](assets/image-20240426195922857.png)

## std::string

### 基础知识

![image-20240426203300711](assets/image-20240426203300711.png)

![image-20240426203250632](assets/image-20240426203250632.png)

![image-20240426203323027](assets/image-20240426203323027.png)

![image-20240426202952570](assets/image-20240426202952570.png)

![image-20240426203235961](assets/image-20240426203235961.png)

### 改变字符串中字符

```c++
//改变所有字符
void test03() {
	std::string str0("Hello World");
	for (auto& c : str0) {
		c = std::toupper(c);//返回大写后的字符c
	}
	std::cout << str0;
}
//使用引用做循环变量时，改变量以此被绑定到每次字符上，通过引用修改改字符

//改变部分字符
void test04() {
	std::string str0("Hello World");
	if (!str0.empty()) {
		str0[1] = std::toupper(str0[1]);
	}
	std::cout << str0;
}
```

![image-20240426204904119](assets/image-20240426204904119.png)

![image-20240426205259876](assets/image-20240426205259876.png)

注意decltype(s.size())

```c++
'X'与"X"是不同的,单引号的是char 类型，双引号的是const char 类型
```

## std::vector

### 基础知识

![image-20240426212516651](assets/image-20240426212516651.png)

==不存在包含引用的vector==