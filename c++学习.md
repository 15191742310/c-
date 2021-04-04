

# 模板

# （1）模板的应用

```c++
template <typename T>  //为了让编译器区分是普通函数，还是模板函数
T MySwap(T &a,T &b)    //函数模板
{
    T tmp = a;
    a=b;
    b=tmp;
}
int MySwap(int &a, int &b) //模板函数
{
    int tmp = a;
    a = b;
    b = tmp;
}
int main()
{
    int a = 1;
    int b = 2;
    MySwap(a,b);      //调用普通函数
    MySwap<>(a,b);    //调用模板
}
```

函数模板可以被普通函数重载，**函数调用的时候优先调用普通函数**

##   (2)模板实现原理剖析

函数模板->模板函数->被调用

例如：MySwap(int , int); 由 T MySwap( T &a ,T &b)推导出 int MySwap(int &a,int &b) 然后才被系统调用；**这个法则可以在Linux下的预编译，编译，链接，汇编验证**   **c++编译机制**独立编译

## （3）类模板的基本语法

```c++
template<typename T>
class Preson
{
    public:
    Preson(T id,T age)
    {
        Mide = id;
        Mage = age;
    }
    void Show()
    {
        cout<<Mid<<age;
    }
    ~Preson(){};
    private:
    T Mid;
    T Mage;
};
int main()
{
    Preson<int> (10.20);
    return 0;
}
```

**案例**(函数)

```c++
temlpate<typename T>
void PrintfArr(T arr[],T n)
{
    for(int i = 0; i < len; i++)
    {
        cout<<arr[i];
    }
}
temlpate<typename T>
void MySort(T arr[] ,T n)
{
    for(int i = 0; i < n-1; i++)
    {
        for(int j = 0; j < n-1-i; i++)
        {
            if(arr[i] > arr[j])
            {
                T tmp = arr[i];
                arr[i] = arr [j];
                arr[j] = tmp;
            }
        }
    }
}
int main()
{
    int arr[]={9,8,7,6,5,4};
    int len=sizeof(arr)/arr[0];
    PrintfArr(arr,len);
    MySort(arr,len);
    return 0;
}
```

**案例**(类)

```c++
template<typename T>
class Preson
{
    public:
    Preson(){
        Mage = 0;
    }
    private:
    T Mage;
};
//系统需要知道得分配多少内存
class SubPreson : public Preson<int>  //这里需要明确具体类型
{
    
}
```

**案例**

```c++
template<typename T>
class Animal
{
    public:
    Animal( T A)
    {
        A = mAge;
    }
    private:
    T mAge;
};
templrate<typename T>
class Cat : public Animal<T>{ };
```

**派生类无论如何都需要给出基类的类型**

## (3)模板类中使用友元

```c++
template<typename T> typename Preson;
template<typename T> void Preson(Preson<T> &p);

template<typename T>
class Preson
{
	public:
    Preson(T ag);
    friend void Pritf <T>(Preson <T> &p);
    //template<typenamt T> 加上这一句只能在windows可以编译通过linux不行，下面的方法两个编译器都可以
    friend ostream &operator<< <T>(ostrram &os, Preson <T> &p);
    private:
    T age;
}；
 template<typename T>
 Preson<T>::Preson(T ag)
 {
     age = ag;
 }
template<typename T>
void Preson<T>::Print(Preson<T> &p)
{
    
}
int main()
{
    Preson<int> p(10);
    return 0;
}
```

**不要滥用友元**

## （4）类模板h和cpp分离写

Preson.h

```c++
#ifnedf PRESON_H
#define PRESON_H
template<typename T>
class Preson
{
    Preson(T ag);
    ~Prager(){}
    private:
    T age;
}
#endif
```

Preson.hpp

```c++
#include"Preson.h"
template<typename T>
Preson<T>::Preson()
{
    age =ag;
}
```

main.cpp

```c++
#include<Preson.hpp>
int main()
{
    Preson<int> p(10);
}
```

当类模板h和cpp分开写的时候因为c++编译的独立性会导致函数模板无法产生模板函数**一般将.h文件的实现.cpp改为.hpp代表这里有模板类的实现**

## （5）类模板中的static成员

```c++
template<typename T>
class Preson
{
    public:
    static int a;
    private:
    T age; 
}
template<typename T>
int Preson<T>::a = 10;
int main()
{
    Preson<int> p1,p2,p3;    //这三个共享同一个a
    Preson<char> p1,p2,p3;   //这三个共享同一个a
} 
```

## (6)仿写vector

```c++
#incoude<iostream>
using namespace std;

template<typename T>
class MyArry
{
    public:
    MyArry(int capacity)
    {
        mCapacity = capacity;
        mSize = 0;
        pAddr = new T[mCapacity];
    }
    MyArry(const MyArry<T> &arr)
    {
        mSize = arr.mSize();
        mCapacity = arr.mCapacity;
        
        //申请内存空间
        pAddr = new T[mCapacity];
        for(int i=0;i<mSize;i++)
        {
            pAddr[i] = arr.pAddr[i];
        }
    }
    T &operator[](int index)
    {
        return pAddr[index];
    }
    MyArry<T> operator=(const MyArry<T> &arr)
    {
        if(this == arr) return this;
        if ( nullptr != pAddr) 
        {
            delect [] pAddr;
        }
        mSize = arr.mSize;    
       mCapacity = arr.mCapacity;
        
        //申请内存空间
        pAddr = new T[mCapacity];
        for(int i=0;i<mSize;i++)
        {
            pAddr[i] = arr.pAddr[i];
        }
    }
    void PushBack(T &data)
    {
        if(mSize>=mCapacity)
        {
            return;
        }
        pAaddr[mSize] = data;
        mSize++;
    }
    void PushBack(T &&data)    //PushBack(100);  对右值取引用
    {
        if(mSize>=mCapacity)  
        {
            return;
        }
        pAaddr[mSize] = data;
        mSize++;
    }
    ~MyArry();
    public:
    //一共可以容下多少东西
    int mCapacity;
    //当前数组有多少元素
    int mSize;
    //保存数据的首地址
    T *pAddr;
}
```

**1对象元素必须能拷贝2容器中都是值寓意而不是应用寓意3深拷贝浅拷贝**

*深拷贝指拷贝指针指向的内容浅拷贝指的是只拷贝指针的本身*

# c++类型转换

static_cast                 一般转换

dynamic_cast            通常在基类和派生类之间转换时使用(做安全检查)

const_casr                         主要对const进行转换

reinterpret_cast         用于进行没有任何关联之间的转换，比如一个字符指针转换为一个整数形

```c++
#include<isotream> 
using namespace std
class Building{};
class Animal{};
class Cat public : Animal{};
//static_cast
void text1()
{
    int a = 10;
    char c = static_cast<char>(a); 
    //指针
    /*int *p = NULL;
    char *cp = static_cast<chat>(p);*/报错
    //对象指针
     /*Building *building = nullptr;
     Animal   *animal = static_cast<Animal *>（building）*/ 报错
    //转换具有继承关系的指针
     //父转子   
     Animal *an = nullptr;
     Cat cat = static_cast<Cat *>(an);
    //子转父 
    an = static_cast<Animal *>(cat);
}
int main()
{
    text1();
}
//static_cast 用于内置类型的数据类型
//还有具有继承关系的指针或引用
```

```c++
//dynamic_cast      转换具有继承关系的指针或者引用，在转换前会进行对象类型检查
void text2()
{
    //基础数据
    /*
    int a = 10;
    char c = dynamic_cast<char>(a);    报错
    */
    //非继承关系的指针
    /*Animal *an = nullptr;
    Building bu = dynamic_cast<Building *>(an
    
    //具有继承关系的指针
    /*
     Animal *an = nullptr;
     Cat *cat = dynamic_cast<Cat *>(an);   报错
    */
    
    Cat cat = nullprt;
    Animal an = dynamic_cast<Ainmal *>(cat);
}
// dynamic_cast 只能转换具有继承关系的指针，并且只能子类指针转换为父类指针
```

const_cast

```c++
void text()
{   
    //基础数据类型  也可以
    int a = 10;
    const int &b = a;
    int & c= const_cast<int &>(b);
    c = 20;
    //指针   怎加或者去除const性
    const int *p = nullptr;
    int *pp = const_cast<int *>(p);
    
    int *p = nullptr;
    const int *pp = const_cast<const int *>(p)l
}
```

reinterprent_cast    函数指针和无关的指针都可以进行转换

```c++
typedef void(*FUNC1) (int,int)
typedef void(*FUNC2) (int, char *)    
void text()
{   
    //1/无关的指针类型都可以进行转换
    Building *building = nullptr;
    Animal *animal = reinterprent_cast<Animal *>(building);
   //2函数指针
    FUNC1 func1;
    FUNC2 func2 =reinterprent_cast<FUNC2>(func1);
}
```

c语言的类型转换**暴力不安全不进行类型检查**     **必须清楚要转变的变量，转换前是什么类型，转换后是什么类型，以及转换后有什么后果**

# c++异常机制

## （1）异常概念

```c++
//异常基本语法
int Div(int x,int y)
{
    if(0 == y)
    {
        throw y;  //抛异常
    }
}
void text01()

    //尝试捕获异常
    try{
        Div(10,0);
    }
    catch(int p) {    //异常时根据类型匹配
        cout<< "除数为0！" << p << endl;
    }
}

```

```c++
int Div(int x,int y)
{
    if(0 == y)
    {
        throw y;  //抛异常
    }
}
void CallDiv(int x, int y)
{
    Div(x,y);
}
void text02()
{
 try{
     CallDiv(10,0);
   }
   catch(int e)
   {
   cout << "除数为0！" << e <<endl;
   }
}
int main()
{
    void text02();
}
```

****

**c++的异常机制跨函数，c++的异常处理必须处理，如果到main还没有处理程序终止**

## （2）栈解旋

```C++
class Preson
{
public:
preson(){
    cout<<"struct"<<endl;
}
~Preson(){
    cout<<"delect"<<endl;
}
    
};
public:
}
int Div(int x,int y)
{
    Preson p1,p2;
   if(0 == y)
  {
   throw y;
  }
    return x/y;
}
void text
{
    try
    {
        Div(10,0);
    }
    catch(int e)
    {
        cout<<"y异常"<<e<<endl;
    }
}
```

**如果抛出异常在函数中创建的所有对象被析构**

## （3）异常接口的声明

```c++
这个函数只能抛出int,float,char异常，如果抛出其他类型异常会错
void func()thrwo(int,float,char)
{
    throw '2';
}
//不能抛出任何异常
void func02()throw()
{
    throw -1;
}
//可以抛出任何类型的异常
void func03()
{
    
}
int main()
{
    try{
        func();
    }catch(char *str)
    {
        cout<<str;
    }catch(char c)
    {
        cout<<c;
    }catch(int n)
    {
        cout<<n;
    }catch(...)    //捕获所有异常
    {
        cout<<"捕获未知异常";
    }
}
```



```c++
class MyException {
    
    public:
    MyException(char *str)
    {
        error = new char[srtlen(str)+1];
        strcup(error,str);
    }
     MyException(const MyException &ex)
     {
         error = new char[strlen(str)+1];
         strcpy(error,ex.error);
     }
    MyException &operator=(const MyException &ec)
    {
        if(nullptr != error)
        {
            delete[] error;
            error = nullptr;
        }
        error = new char[strlen(ex.error)+1];
        strcpy(error,ex.error);
    }
    void what()
    {
        cout<<error<<endl;
    }
    public:
    char *error;
    ~MyException()
    {
        delete[] error;
        error = nullptr;
    }
};
void func()
{
    throw MyException();
}

void text()
{
    try{
        func();
    }catch(Exception e)
    {
        cout<<e.what();
    }
}
```

## (4)异常生命周期

```c++
class MyException {
    
    public:
    MyException(char *str)
    {
        cout<<"struct"<<endl;
    }
     MyException(const MyException &ex)
     {
         cout<<"cpstruct"<<endl;
     }
     ~MyException()
    {
      
    }
};
void func()
{
    throw MyException();   //创建匿名对象
}
void text()
{
    try{
        func();
    }catch( MyException e)
    {
        cout<<"捕获异常"<<endl;
    }
}
//构造函数
//拷贝构造
//捕获异常
//析构函数
//析构函数
/*
普通类型元素 引用指针 
普通元素 异常对象catch处理完之后就析构
引用 不调用拷贝构造，异常对象catch处理完之后就析构
*/
```

**catch严格匹配异常类型**

## （5）c标准异常类_编写自己的异常类

```c++
#include<stdexcept>
class Preson{

  public:
    Preson(){
       age = 0;
   }
  void setAge(int ag)
    {
        if(ag<0 || ag > 100)
        {
            throw out_of_range("年龄应该在0-100之间")；
        }
      age = ag;
 }
    public:
     int age;
};

class MyOutOfOrange : exception
{
    public:
    MyOutOfOrange(char *error)
    {
        pError = new char[strlen(error)+1];
        strcpy(pError,error);
    }
        virtual const char *what() const
    {
        }
  ~MyOutOfOrange()
    {
      if(nullptr != pError)
        {
        delect[] pError;
        pError = nullptr;
        }
    }
    public:
    char *pError;
}
void text()
{
    Preson p;
    try
    {
        p.setAge(1000);
    }catch(exceprion e)
    {
        cout<<e.what()<<endl;
    }
}
```

# STL

## (1)STL基本概念

 stl 是标准模板库（基于模板）

STL分为：**容器** **算法** **迭代器**容器和算法之间通过迭代器进行无缝连接。STL几乎所有代码都采用了模板类或者模板函数

**头文件**

<algorithm> 、<deque>、<functional>、<vector>、<list>、<map>、<memory>、<numeric>、<queue>、<stack>、<utility>

## (2)容器算法迭代器分离案例

```c++
int mycount(int *start,int *end,int val)
{
    int num = 0;
    while(start != end)
    {
        num++;
    }
    return num;
}
int main()
{
    int arr[]={1,2,3,4,5,6,5,5,5,6};
    int *pbegin = arr;
    int *endd = (arr[sizeof(arr)/sizeof(arr[0]));
   int num= mycount(pbegin,endd,5);
}
```



```c++
#include<vector>
#include<algorithm>
using namespace std;

//STL基本语法

void PrintfVector(int v)
{
 cout << v<< endl;++++   
}
void text()
{
    vector<int> vec;  //定义了一个容器，并指定容器存放的元素类型的int
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);
    //通过STL提供的算法for_each算法
    //容器提供迭代器
   vector<int>::itreator pbegin = v.begin();
   vector<int>::itreator pend = v.end();
    //容器中可能存放基础的数据类型
    for_each(pgegin,pend,PrintfVector);
}
```

## (3)string

string 的特性

char *是一个指针，String是一个类

string封装了char *，管理这个字符串，是一个char *型的容器

string封装了很多方法

   查找（find），拷贝（copy），删除（delete），替换（replace）,插入（insert）

不用释放内存和越界

string管理char *所分配的内存，每一次string的赋值，取值都由string类负责维护，不用担心赋值越界和取值越界。

string 与char *可以转换     调用c_str()方法

```c++
void text()
{
	string s1;             //无参构造
    string s2(10,'a');
    string s3("asdfdsg");
    string s4(s3);         //拷贝构造
}
void text2()
{
    string s1;
    string s2("1321654");
    s1 = "465";
    
    s1=s2;
    
    s1='s';
    //提供成员方法assign方法赋值
    s1.assign("13");
    
    
    for(int i = 0;i<s1.size();i++ )
    {
        cout << s1[i]<<" ";
    }
    
    for(int i = 0;i<s1.size();i++)
    {
        cout << s1.at(i)<<' ';
    }
    
    //区别： []访问越界不会抛出异常   at(i)会抛出异常out_of_range
}
```

拼接

```c++
void test()
{
	string s1 = "45646";
    string s2 = "789";
    s1 += "dasd";
    s1 += s2;
    
    string s3 = '222';
    s2.append(s3);
    
    string s4 = s2+s3;
}
```

查找

```c++
void text()
{
    string s1 = "asdasfg";
    //查找第一次fg出现的位置
    int pos = s.find("fg");
    cout<<"pos"<<pos<<endl;
    //查找最后一次出现fg的位置
    int pos = s.rfind("fg");
    cout<<"pos"<<pos<<endl;
}
```

替换

```c++
void text()
{
    srting s1 = "456789";
    s.replace(0,2,"111");  //0号位置开始两个将两个字符替换成111
    cout<<s1<<endl;
}
```

比较

```c++
void text()
{
     string s1 = "123456";
     string s2 = "78946";
     if(s1.compare(s2) == 0)
     {
         cout<<"字符串相等"<<endl;
     }else
     {
         cout<<"字符串不相等"<<endl;
     }
}
```

字串

```c++
string substr(int pos = 0,int n = npos) const;//返回由pos开始的n个字符组成的字符串
void text()
{
    string s1="456789";
    string s2 = s.substr(1,3)
}
```

string插入与删除

```c++
string &insert(int pos,const char *s);     //插入字符串
string &insert(int pos,const string &str)  //插入字符串
string &insert(int pos,int n,char c);      //在指定位置插入n个字符c
string &erase(int pos,int n = npos);      //从pos开始删除n个字符

void text()
{
    string str = "123456";
    
    str.insert(1,"78946");
    str.insert(4,"4456");
    str.insert(1,5,'w');
    str.erase(1,3);
}
```

## (3)vector

