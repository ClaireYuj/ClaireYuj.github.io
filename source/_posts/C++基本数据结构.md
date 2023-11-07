---
title: C++基本数据结构
top: false
cover: false
author: DULULU oO
date: 2023-04-04 11:57:05
password:
summary:
tags: 
    - C++
    - 数据结构
categories: 数据结构
---

## C++基础

### 指针与引用

1. 不存在空引用，但可以有空指针。
2. 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
3. 引用必须在创建时被初始化。指针可以在任何时间被初始化。

在64位系统中地址的总线宽度是8个字节，32位系统4个字节，所以32位系统中一个指针式4个bit，64位系统上是8个bit

```cpp
int main()
{    

    int i =1;
    int &s = i;
    int *p = &i;
    cout<<"i: "<<&i<<endl;
    cout<<"s："<<&s<<endl;
    cout<<"p: "<<p<<endl;
    return 0;
}
```

> 输出：
 i: 0x61ff04
 s：0x61ff04
 p: 0x61ff04

 引用分配到内存和指针指向的内存都是同一块内存

 C++中用引用传参更加安全，如swap(&a,&b)



### 结构体


C++中的struct对C中的struct进行了扩充，它已经不再只是一个包含不同数据类型的数据结构了，它已经获取了太多的功能

1. struct能包含成员函数
2. struct能继承
3. struct能实现多态


结构体和类最本质的一个区别就是默认的访问控制： 

默认的继承访问权限：struct是public的，class是private的

C++中，不使用结构体丝毫不会影响程序的表达能力。C++之所以要引入结构体，是为了保持和C程序的兼容性

但有时仍会在C++中使用结构体，是因为可以使用结构体将不同类型数据组成整体，方便于保存数据(若用类来保存，因类中成员默认为私有，还要为每个数据成员特定函数来读取和改写各个属性，比较麻烦)

struct可以继承class，同样class也可以继承struct

struct是一种数据结构的实现体，虽然它是可以像class一样的用。我依旧将struct里的变量叫数据，class内的变量叫成员，虽然它们并无区别

概念：class和struct的语法基本相同，从声明到使用，都很相似，但是struct的约束要比class多，理论上，struct能做到的class都能做到，但class能做到的stuct却不一定做的到
类型：**struct是值类型，class是引用类型**，因此它们具有所有值类型和引用类型之间的差异
效率：由于堆栈的执行效率要比堆的执行效率高，但是堆栈资源却很有限，不适合处理逻辑复杂的大对象，因此struct常用来处理作为基类型对待的小对象，而class来处理某个商业逻辑
关系：struct不仅能继承也能被继承 ，而且可以实现接口，不过Class可以完全扩展。内部结构有区别，struct只能添加带参的构造函数，不能使用abstract和protected等修饰符，不能初始化实例字段

### 内存
内存分成5个区，**堆，栈，自由存储区，全局/静态存续区，常量存续区**。

栈区(stack sagment)：由**编译**器自动分配释放，存放**函数的参数的值，局部变量的值**等。在Windows下，栈是向低地址扩展的数据结构，是一块**连续的内存的**区域。这句话的意思是**栈顶的地址和栈的最大容量是系统预先规定好的**，在Windows下，栈的大小是1M，如果申请的空间超过栈的剩余空间时，将提示stack overflow。因此，能从栈获得的空间较小。

堆区(heap sagment) ：程序**运行时动态内存分配**，堆是可以上增长的。 一般由程序员分配释放，内存使用new进行分配，使用delete或delete[]释放。如果未能对内存进行正确的释放，会造成内存泄漏。但在程序结束时，会由操作系统自动回收。它与数据结构中的堆是两回事。**堆是向高地址扩展的数据结构，是不连续的内存区域**。这是由于系统是用链表来存储的空闲内存地址的，自然是不连续的，而链表的遍历方向是由低地址向高地址。堆的大小受限于计算机系统中有效的虚拟内存。由此可见，堆获得的空间比较灵活，也比较大。

自由存储区：使用malloc进行分配，使用free进行回收。

全局/静态存储区：**全局变量和静态变量被分配到同一块内存中**，C语言中区分初始化和未初始化的，C++中不再区分了。（全局变量、静态数据 存放在全局数据区）

### 构造函数与析构函数

构造函数--初始化对象时执行
析构函数在每次删除所创建的对象时执行，它**不会返回任何值，也不能带有任何参数**。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。

```cpp
#include<iostream>
using namespace std;
class test{
   
    public:
        int val;
        test(string s);
        ~test();
};

test::test(string s){
    cout<<"create test:"<<s<<endl;
}

test::~test(){
    cout<<"delete test"<<endl;
}

int main()
{    
    test t("test 1");
    t.val = 10;
    printf("hello world, %d\n", t.val);  
    return 0;
}
```

> 输出
 create test:test 1
 hello world, 10
 delete test

#### 实例化方法

##### 直接创建

```cpp
test t("test");
```
创建在栈里


##### new
```cpp

    test* t = new test("test");
    (*t).val=1;
    cout<<"hello:"<<(*t).val<<endl;
    delete t;


```
指针形式，常用于初始化链表头。
创建在堆里，需要自己手动释放内存

使用new的方式，如果对象没有初始化，此时没有分配内存空间，也无法delete。
不要new一个null对象
### 继承

> class derived-class: access-specifier base-class

访问修饰符是public，protected，private

```cpp
// 基类
class Animal {
    // eat() 函数
    // sleep() 函数
};


//派生类
class Dog : public Animal {
    // bark() 函数
};
```

### 修饰符

**public**: 可以被类以外成员访问
**private**： 只可以被类和友元函数访问，要想修改相应的参数或操作，只能通过类中public函数和友元函数，类中成员默认是private
**protected**： 可以被派生类访问

### 拷贝

拷贝是指将一个对象的值或状态复制到另一个对象中，使得两个对象在某些方面具有相同的特性。

#### 拷贝构造函数

拷贝构造函数必须**是当前类的引用**
**拷贝构造函数是const 引用**

拷贝构造函数的目的是用其它对象的数据来初始化当前对象，并没有期望更改其它对象的数据

```cpp
class MyClass {
public:
  int x;
  MyClass(int value) : x(value) {} // 普通构造函数
  MyClass(const MyClass& other) : x(other.x) {} // 拷贝构造函数
};

MyClass obj1(42); // 使用普通构造函数创建对象
MyClass obj2(obj1); // 使用拷贝构造函数创建对象

```
在创建对象 obj1 时，使用了普通构造函数，将参数值 42 赋值给成员变量 x；在创建对象 obj2 时，使用了拷贝构造函数，将对象 obj1 的值复制到新对象 obj2 中。


#### 浅拷贝

浅拷贝（shallow copy）是指将**对象的所有成员变量直接复制到另一个对象中，包括指针类型成员变量所指向的内存地址**。因此，如果两个对象共用同一块内存地址，那么**它们之间的关系就会非常紧密，任何一方的修改都会影响到另一方。**

```cpp
class MyString {
public:
  char* str;
  MyString(const char* s) {
    str = new char[strlen(s) + 1];
    strcpy(str, s);
  }
  ~MyString() {
    delete[] str;
  }
  MyString(const MyString& other) {
    str = other.str;
  }
};

MyString str1("hello");
MyString str2(str1); // 浅拷贝
str1.str[0] = 'H';
std::cout << str1.str << std::endl; // 输出 "Hello"
std::cout << str2.str << std::endl; // 输出 "Hello"

```

#### 深拷贝
深拷贝（deep copy）是指将对象的所有成员变量复制到另一个对象中，并**为指针类型成员变量分配新的内存空间，将原来指向的内存数据复制到新的内存空间中。**因此，深拷贝可以避免共用内存带来的问题。

```cpp
class MyString {
public:
  char* str;
  MyString(const char* s) {
    str = new char[strlen(s) + 1]; 
    strcpy(str, s);
  }
  ~MyString() {
    delete[] str;
  }
  MyString(const MyString& other) {
    str = new char[strlen(other.str) + 1]; //分配新的内存空间
    strcpy(str, other.str);
  }
};

MyString str1("hello");
MyString str2(str1); // 深拷贝
str1.str[0] = 'H';
std::cout << str1.str << std::endl; // 输出 "Hello"
std::cout << str2.str << std::endl; // 输出 "hello"

```

## 数据结构(容器)

### array

内存空间连续的一段地址，无法改变大小

```cpp
# include<array> //头文件

array<int,10> myarray; //初始化

for(int i=0;i<myarray.size();i++){
    myarray[i] = i;
} //赋值

for(auto it = myarray.begin;it!=myarray.end();it++){
    cout<<*it<<endl;
} //遍历

cout<<myarray[3]<<endl;//获取特定数值

```
### vector

动态数组

```cpp

# include<vector> // 头文件
vector<int> myvector;

//添加元素
for(int i = 0; i<10;i++) myvector.push_back(i);

//遍历
for(auto it = myvector.begin();it!=myvector.end();it++) cout<< *it << endl;

int size = myvector.size();
bool isEmpty = myvector.empty();
myvector.insert(myvector.end(),5,3);//在末尾插入五个3

myvector.pop_back();//删除末尾元素
myvector.at(0);//返回第一个元素

iterator it = myvector.begin();//获取迭代首地址
```


### 链表


```cpp
#include<list>
int num[] = {1,2,3,4,5};
list<int> mylist(num,num+sizeof(num/sizeof(int)))//使用数组创建list

for(auto it = mylist.begin;it!=mylist.end();it++){
    cout << *it << endl;

}//遍历

auto it = mylist.begin();
for(int i = 0; i < 5; i++){
    mylist.insert(it,i);//插入
}



```

都大差不差
```cpp
typedef struct Link
{
	char elem;//代表数据域
	struct Link * next;//代表指针域，指向直接后继元素
}link;
```

双向链表

```cpp
//C++双向链表模板
class MyList
{
private:
    struct ListNode
    {
        int val;
        ListNode *next,*prev;
        ListNode(int x):val(x),next(nullptr),prev(nullptr){} //复制val->x,next->nullptr,prev->nullptr
    };
private:
    //头节点尾节点都为空，表示为空链表
    ListNode *head,*tail;
    int size=0;
public:
    MyList():size(0),head(nullptr),tail(nullptr){}
}
```

实现反转链表
```cpp
ListNode* reverseList(ListNode* head)
    {
        ListNode* pre = nullptr;
        ListNode* cur = head;
        ListNode* nex ;        
        while(cur)
        {
            nex = cur->next;//提前该语句
            cur->next=pre;
            pre=cur;
            cur=nex;
           // nex = cur->next;//当cur为空指针时该语句会报错，运行超时，需要将该语句提前
        }
        return pre;
    }

```

### 队列

deque:前后两端都可以进行数据的插入和删除，至此数据的快速随机访问

```cpp
#include<iostream>
#include<deque>
using namespace std;

int main(){
    deque<int> D;
    D.push_back(1);
    D.push_back(2);
    D.push_back(3); //赋值
    for(auto it = D.begin();it!=D.end();it++){
        cout<<*it<<" ";//遍历
        }
    cout<<endl;
    D.insert(D.begin()+1,10); //在索引为1的地方插入10
        for(auto it = D.begin();it!=D.end();it++){
        cout<<*it<<" ";//遍历
        }

    D.push_front(-1); //在头部插入
    cout<<endl;
    for(auto it = D.begin();it!=D.end();it++){
        cout<<*it<<" ";//遍历
        }

    D.pop_front();//头部弹出
    cout<<endl;
    return 0;
}
```
> 输出
 1 2 3 
 1 10 2 3 
 -1 1 10 2 3 

### 栈

后入先出LIFO
动态实现：
```cpp
typedef struct SqStack_dynamic{
	int* base;//栈底 
	int* top;//栈顶 
}SqStack_dynamic;
```
静态实现
```cpp

typedef struct SqStack_static{
	int data[Maxsize];
	int top;//栈顶 
}SqStack_static;
```
STL中的栈

```cpp
#include<iostream>
#include<stack>
using namespace std;

int main(){

    stack<int> S;
    S.push(1);
    S.push(2);
    S.push(3);
    for(;!S.empty();){
        cout<<S.top()<<" ";
        S.pop();
    }

    return 0;
}
```
### Map

```cpp
#include<iostream>
#include<map>
using namespace std;

int main(){
    map<int, string> my_map;
    //三种插入的方式
    my_map.insert(pair<int,string>(1,"first")); //插入pair对
    my_map[2] = "insert second";
    my_map[10] = "ten"; 
    for(auto it = my_map.begin();it!=my_map.end();it++){
        cout<<it->first <<"->"<<it->second<<endl;
    } //it->first得到第一个的值，it->second得到第二个的值

    //map的删除
    my_map.erase(2);//删除2
    auto it = my_map.find(10);
    my_map.erase(it); //删除找到的10
    return 0;
}
```

### Set
set也是一种关联性容器，它同map一样，底层使用红黑树实现，插入删除操作时仅仅移动指针即可，不涉及内存的移动和拷贝，所以效率比较高。从中文名就可以明显地看出，**在set中不会存在重复的元素**，若是保存相同的元素，将直接视为无效。

unordered_set常用：
**insert**:插入指定元素
**erase**: 删除指定元素
**find**:查找指定元素
**size**,获取容器中元素的数量
**empty**:判断是否为空
**swap**:交换

```cpp

#include <iostream>
#include <unordered_set>
using namespace std;

int main()
{
	unordered_set<int> us;
	//插入元素（去重）
	us.insert(1);
	us.insert(4);
	us.insert(3);
	us.insert(3);
	us.insert(2);
	us.insert(2);
	us.insert(3);
	//遍历容器方式一（范围for）
	for (auto e : us)
	{
		cout << e << " ";
	}
	cout << endl; //1 4 3 2
	//删除元素方式一
	us.erase(3);
	//删除元素方式二
	unordered_set<int>::iterator pos = us.find(1); //查找值为1的元素
	if (pos != us.end())
	{
		us.erase(pos);
	}
	//遍历容器方式二（迭代器遍历）
	unordered_set<int>::iterator it = us.begin();
	while (it != us.end())
	{
		cout << *it << " ";
		it++;
	}
	cout << endl; //4 2
	//容器中值为2的元素个数
	cout << us.count(2) << endl; //1
	//容器大小
	cout << us.size() << endl; //2
	//清空容器
	us.clear();
	//容器判空
	cout << us.empty() << endl; //1
	//交换两个容器的数据
	unordered_set<int> tmp{ 11, 22, 33, 44 };
	us.swap(tmp);
	for (auto e : us)
	{
		cout << e << " ";
	}
	cout << endl; //11 22 33 44
	return 0;
}

```