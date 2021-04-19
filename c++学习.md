

# 模板

## （1）模板的应用

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

**vector支持随机访问**

vector动态增长的基本原理：

vector当插入新元素的时候如果空间不足，那么vector会重新申请更大的一块内存空间，将原来的数据拷贝到新空间，释放旧空间，再把元素插入新空间。

构造函数

```c++
vector<T> v;              //采用模板实现类实现，默认构造函数
vector(v.begin(),v.end()) //将begin与end之间的元素拷贝给本身
vector(n,elem)            //构造函数将n个elem拷贝给本身
vector(const vector &vec) //拷贝构造函数
#include<vector>    
void text()
{
    vector<int> vec;
    int arr []= {1,2,3,4,5,6};
    vector<int> vec1(arr,arr[sizeof(arr)/sizeof(arr[0])]);
    vector<int> vec2(3,2);         //初始化3个2
    vector<int> vec3(v2);
    
 for(vector<int>::inerator it = vec.begin();it != vec4.end();it++)
 {
     cour<<*it<<endl;
 }
}
```

赋值操作

```c++
assign(v.begin,v.end)   //将begin与end区间数据拷贝赋值给本身
assign(n,elem)          //将n个emel赋值给本身
vector &operator=(const vector &vec)  //重载赋值操作
swap(vec);                            //将本身的值与vec交换 

void text()
{
      int arr []= {1,2,3,4,5,6};
      vector<int> v1(arr,arr[sizeof(arr)/sizeof(arr[0])]);
    
      vector<int> v2;
      v2.assign(v1.begin(,v1.end()));
    
      vector<int> v3;
      v3=v2;
      
      v3.swap(v2);                   //原理是将两个指针的指向进行交换
}
```

大小

```c++
size();          //返回容器中的元素的个数
empty();         //判断容器是否为空
resize(int num); //重新指定容器的长度为num，若容器变长，则默认值填充新位置，若容器变短则删除末尾超出容器的元素被删除
capacity();      //容器的容量
reserver(int len);      //容器预留len个位置，预留位置不初始化不可访问

void text()
{
    int arr[] = {1,2,3,4,5,6};
    
    vector<int> vec = arr;
    cout<<"size"<<sizeof()<<endl;
    if(vec.empty())
    {
        cout<<" "<<endl;
    }
}else
{
    cout<<"not"<<endl;
}
vex.resize(2);
 for(vector<int>::inerator it = vec.begin();it != vec4.end();it++)
 {
     cour<<*it<<endl;
 }                              //只能打印两个，后面的为0；
 vec.resize(6);                 //改变大小补0
 vecresize(6,1);                //后面的补1
 vec.capacity();                //容量大于等于size 
```

数据存取操作

```c++
at(int index);  //返回索引index指向的数据，如果越界，抛出out_of_range异常
operator[];     //返回索引所指向的值，越界时直接报错
front();        //返回容器的第一个元素
banck();        //返回容器的最后一个元素
```

插入与删除

```c++
insert(const iterator pos,int cont ,int elem) //迭代器指向位置插入count个num
push_back();                 //尾插
pop_back();                  //尾删
erase(const iterator start,const iterator end); //删除迭代器start到end的值
erase(const iterator pos);    //删除迭代器指向的元素
clear();                      //清空容器
void text()
{
    vector<int> v;
    v.push_back(10);
    v.push_back(20);
    v.insert(v.begin(),30);
    v.insert(v.end,40);
    v.insert(v.begin()+2,100);   //vector支持随机访问
    v.erase(v.begin());
    v.erase(v.begin()+1,v.end());
    v.clear();
}
```

**当容器空间不足的时候会重新申请一片新的空间**

swap缩减空间  **牛逼了**

```c++
void text()
{
    vector<int> v;
    for(int i = 0; i < 10000;i++)
    {
        v.push_back();
    }
    cout<< "size" <<v.size()<<endl;
    cout<<"capacity"<<v.capacity()<<endl;
    
    v.resize(10);                   //只改变size不改变容量
    
    cout<< "size" <<v.size()<<endl;
    cout<<"capacity"<<v.capacity()<<endl;   //空间还是10000
    
    //收缩空间
    vector<int> (v).swap(v);         //vextor<int> ()匿名对象用v去初始化  初始化完成之后用swqp交换   
    cout<< "size" <<v.size()<<endl;
    cout<<"capacity"<<v.capacity()<<endl;
    
    
}
```

resize()于reserve()

```c++
void text()
{
    vector<int> v;
    int *addr = nullptr;
    int num = 0;
    for(int i=0;i < 10000 ;i++)
    {
        v.push_back(i);
        if(addr != &v[0])
        {
            addr = &v[0];
            num++;
        }
    }                          //结论多次拷贝浪费时间
    
    v.reserve(10000);
      for(int i=0;i < 10000 ;i++)
    {
        v.push_back(i);
        if(addr != &v[0])
        {
            addr = &v[0];
            num++;
        }
    }                         //少了拷贝释放的过程
    
}
```

## （4）deque双端队列

尾部插入  push_back();

尾部删除  pop_back();

头部插入 push_front();

头部删除 pop_front();

迭代器：

begin();      //类似于头指针    

end();         //类似于尾指针

方法：  

front ();   //返回第一个元素

back();     //返回最后一个元素

**在两端插入删除效率高 ，中间的、移动的效率差， 和vector一样支持随机访问**

deque构造函数

```c++
deque<T> deqT;				//默认构造形式
deque(beg,end);       		//构造函数将beg与end之间的元素拷贝给本身
deque(n,elem);              //构造函数将n个emel拷贝给本身
deque(const deque &deq);    //拷贝构造函数


void text()
{
    deque<int>  deq1;
    
    for(int i = 0;i < 10; i++);
    {
        deq.push_back(i);
    }
    deque deq2(que1.begin(),que1.end());
    
    deque que3.(que2);
    
    deque que4(3,10);
}
```

deque  赋值

```c++
assign(beg,end);       //将beg到end的元素拷贝赋值给本身
assign(n,elem);        //将n个elem个元素拷贝赋值给本身
deque &operator=(const deque &deq);   //重载等号
swap(deq);             //与deq本身的元素互换

void text()
{
    deque<int> que1;
    for(int i=0;i<100;i++)
    {
        que1.push_back(i);
    }
    
    deque que2.assign(que1.begin(),que1.end());
    
    deque que3 = que2;
    deque que4(10,5);
    deque que5 .swap(que4);
}
```

deque 大小操作

```c++
deque.size();     			//deque的大小
deque.empty();				//deque是否为空
deque.resize(num);			//重新指定容器，如果容器长度变长则采用默认值天聪位置如果容器长度都变短则删除尾部元素
deque.resize(num,elem);     //重新指定容器大小，如果容器的长度变长则用elem填充，如果容器的长度变短则删除尾部元素
```

deque  双端插入和删除

```c++
push_back(elem);     //在容器的尾部插入数据
push_front(elem);  	 //在容器的头部插入数据		
pop_back(elem); 	 //在容器的尾部删除数据		
pop_front(elem);  	 //在容器的头部删除数据

void text()
{
    deque<int> que;
    
    que.push_back(1);
    que.push_front(2);
    que.pop_back();
    que.pop_front();
}
```

deque 数据的读取

```c++
at(idx);      //返回索引inx所指的值如果越界则抛出out_of_range
operator[];   //返回索引inx所指的值如果越界不返回异常直接报错
front();      //返回第一个元素的值
back();		  //返回最后一个元素的值
```

deque的删除操作

```c++
clear();            //清空双端队列
erase(beg,end);     //删除beg至end的数据
erase(pos);			//删除pos位置的数据

void text()
{
    deque<int> que;
    for(int i = 0; i < 10 ; i++)
    {
        que.push_back(i);
    }
    
    que.erase(que.begin(),que.end());
    que.erase(pos);
    que.clear();
    
}
```

**deque练习**

```c++
#include<iostream>
#include<vector>
#include<string>
#include<deque>
using namespace std;
class Player
{
  public:
    Player(string name,int score):mName(name),mScore(score){}
  public:
    string mName;
    int mScore;
};
void Create_Player(vector<Player> &v)
{
    string str = "abcde";
    for(int i = 0;i < 5;i++)
    {
        Player p;
        p.mName = "选手";
        p.mName += str[i];
        p.mScore = 0;
        
        v.push_back(p);
    }
}
void Set_Score(vector<Player> &v)
{
    
    for(vector<Player>::iterator it = v.begin();it != v.end();it++)
    {
      deque<int> que;
        for(int i = 0;i<10;i++)
        {
            int score = rand()%41+60;
            que.push_back(score);
        }
        
        sort(que.begin(),que.end());
        que.pop_back();
        que.pop_front();
        
        int avg = 0;
  for(deque<int>::iterator dit = que.begin();dit != que.end();dit++)
     {
         avg += (*it);
     }
      avg = avg/que.size();
      it->mScore = avg;
    }
}
bool mycompare(Player &p1, Player &p2)
{
    return p1.mScore > p2.mScore;
}
void Print_Rank(vector<Player> &v)
{
    sort(v.begin(),v.end(),mycompare);
    for(vector<int>:: iterator it = v.begin();it != v.end();it++)
    {
        cout<<"姓名"<<it->mName<<"分数"<<it->mScore<<endl;
    }
}
int main()
{
    vector<Player>  vPlist;
    Create_Player(vPlist);
    Set_Score(vPlist);
    Print_Rank(vPlist);
}
```



## (5)stack

**栈不能遍历，不能随机存取，只能top从栈顶获取和删除元素**

stack构造函数

```c++
stack<T> sta; 				//stack采用模板类实现，stack对象的默认构造函数
stack(const stack &sta); 	//拷贝构造函数

void text()
{
    stack<int> sta;
    sta.push(1);
    sta.push(2);
    stack<int> sta1(sta);
}
```

stack的赋值操作

```c++
stack &operator=(const stack &sta);   //重载等号操作符

void text()
{
    stack<int> sta1;
    sta1.push(1);
    sta1.push(2);
    stack<int> sta2 = sta1;
}
```

stack增加、删除、访问、清空、数据

```c++
push(elem);    //压栈
pop(); 		   //吐栈
top(); 		   //取栈顶元素
clear();       //清空栈
void text()
{
    stack<int> sta;
    sta.push(1);
    sta,push(2);
    sta.push(3);
    
    int elem = sta.top();
    
    sta.pop();
    sta.clear();
}
```

## (6)queue

**先进先出，不能遍历，不提供迭代器，不支持随机访问**

queue的构造函数

```c++
queue<T> que;				//queue采用模板类实现，queue对象的默认构造形式
queue(const quque &que);	//拷贝构造

void text()
{
    queue<int> que1;
    queue<int> que2(que1);
}
```

queue存取、插入和删除操作

```c++
push(elem);  	//往容器的末尾添加数据
pop();			//从队头移除一个数据
back();			//返回最后一个元素
front();		//返回第一个元素

void text()
{
    queue<int> que;
    que.push(1);
    que.push(2);
    que.push(3);
    que.pop();
    que.back();
    que.front();
}
```

queue的赋值操作

```c++
queue &operator=(const queue &que);  //重载等号操作符

void text()
{
    queue<int> que;
    que.push(1);
    
    queue<int> que1(que);
}
```

queue 容量及修改器

```c++
empty();   //判断是否为空 bool
size();	   //返回queue的大小 int
swap();    //交换元素
```

非成员函数

```c++
operator==
operator!=
operator<
operator<=
operator>
operator>=
按照字典顺序比较 queue 中的值
```

## (7)List

**非连续添加删除元素方便，不用移动元素效率比数组高、无迭代器**

**只要能拿到第一个节点就等于拿到整个链表**

list的构造函数

```c++
list<int> li1;   			//list采用模板类实现，对象的默认构造形式
list(beg,end);				//构造函数将beg，end区间中的元素拷贝给本身
list(n,emel);				//构造函数将n个elem拷贝给本身
list(const list &li);		//拷贝构造函数
```

list数据元素插入和删除操作

```c++
push_back(elem);    //尾插
pop_back();		    //尾删
pusn_front(elem);	//头插
pop_front();		//头删
insert(pos,elem);   //在pos位置插入elem，返回新数据的位置
insert(pos,n,elem); //在pos位置插入n个elem，无返回值
insert(pos,beg,end);//在pos位置插入beg与end之间的元素
clear();            //移除容器中的所有元素
erase(beg,end);     //移ben，end之间的元素，返回下一个元素的位置
erase(pos);         //移除pos位置的元素
remove(emel);		//删除容器中所有与elem元素相同的数字
resize(num);	//重新指定容器的大小，如果变长用默认值填充，变短则将尾部数据删除
resize(num,elem);  //重新指定容器的大小，如果变长用elem填充，变短则将尾部数据删除

void text()
{
    list<int> mlist;
    
   for(int i = 0;i < 10; i++)
   {
       mlist.push_back(i);
   }
    insert(mlist.begin()+2,5);    //报错
    list<int>::iterator li = mlist.begin();
    li++;
    li++;
    insert(li,5);
}
```

list的大小操作

```c++
size();         //容器的大小
empty();		//容器是否为空
```

list数据存储以及list反转排列序列

```c++
front();   			//返回第一个元素
banck();			//返回最后一个元素
reverse();          //反转单链表，比如list包含1，3，5元素，调用后变成5，3，1
sort(); 			//排序，成员函数

bool mycompare(int v1,int v2)
{
    return v1 > v2;
}
void text()
{
    list<int> mlist;
    for(int i = 0; i < 10; i++)
    {
        mlist.push_back(i);
    }
    
    mlist.reverse();        //完成反转
    
    mlist.(mlist);          //默认排序
    mlist.sort(mycompare);  //设置的排序 
    
}
```

**算法sort   支持可随机访问的容器，因此list自己提供了sort**

## 二叉树

**概念**

任何一个节点只有两个节点，分别是左节点和右节点

**二叉搜索树**

**平衡二叉树**



## set/multiset 容器

**set/multiset特性**

set/multiset的特是所有**元素会根据元素的值进行自动排序**，set是以RB-tree(红黑树，平衡二叉树的一种)为底层机制，查找效率非常好。set容器中不允许重复的元素，multiset允许重复元素。

**set不允许通过迭代器改变元素**因为，set是有序的集合，如果任意改变set的元素，会严重破坏set组织

**set常用API**

```C++
set<T> st;				//set默认人拷贝构造
mulitset<T> mst;		//multiset默认构造函数
set(const set &st);		//拷贝构造函数
```

**set赋值操作**

```c++
set &operator=(const set &st);    //重载等号操作符
swap(st);   					  //交换两个集合容器
```

**set大小操作**

```c++
size();                     		//返回容器中元素的大小
empty();          					//判断容器是否为空
```

**set插入删除**

```c++
insert(elem);  			//在容器中插入元素
clear();				//清除容器	
erase(pos);				//删除pos迭代器所指的元素，返回下一个元素的迭代器
erase(beg,end);			//删除beg到end之间的所有元素，返回下一个元素的迭代器
erase(elem);			//删除容器中值为elem的元素

void text()
{
    
    set<int> st;
    
	st.insert(1);
	st.insert(7);
	st.insert(8);
	st.insert(9);
	st.insert(5);
	st.erase(5);

	for (set<int>::iterator it = st.begin(); it != st.end(); it++)
	{
		cout << *it;
	}
	cout << endl;
	set<int>::iterator itb = st.begin();
	itb++;
	set<int>::iterator ite = st.end();
	ite--;
	st.erase(itb,ite);
	for (set<int>::iterator it = st.begin(); it != st.end(); it++)
	{
		cout << *it;
	}
	cout << endl;
	st.erase(9);
	for (set<int>::iterator it = st.begin(); it != st.end(); it++)
	{
		cout << *it;
	}
}
```

**set查找操作**

```c++
find(key);        		//查找键key是否存在，若存在返回该键元素的迭代器，若不存在返回set.end();
lower_bound(keyElem);   //返回第一个key>=keyElem元素的迭代器 *解引用即可
upper_bound(keyElem);   //返回第一个key>keyElem元素的迭代器  *解引用即可
equal_range(keyElem);   //返回容器中key与keyElem相等的上下限的两个迭代器
//仿函数
class mycompare
{
    public:
    bool operator()(int v1,int v2)
    {
        return v1 > v2;
    }
}
void text()
{
    set<int> st;    //从小到大
    set<int,mycompare> st;    //从小到大
    
	st.insert(1);
	st.insert(7);
	st.insert(8);
	st.insert(9);
	st.insert(5);
	st.erase(5);
    
    set<int>::iterator ret = st.find(4)
     if(ret == st.end())
     {
         cout<<"没有找到！"<<endl;
     }else
     {
         cout<<"ret"<<*ret<<endl;
     }
    
    //equal_range 返回lower_bound 和 upper_bound 值
 pair<set<int>::iterator,set<int>::iterator> myret = st.equal_range(2);
    if(myret.fist == st.end())
    {
        cout<<"找到";
    }else
    {
        cout<<"未找到";
    }
    if(myret.second == st.end())
    {
        cout<<"找到";
    }else
    {
        cout<<"未找到";
    }
}
```

**set排序补充**

```c++
class Preson
{
  public:
    Preson(int id,int age):id(id),age(age);
  public:
    int id;
    int age;
};
class mycompare
{
  public:
  bool operator()(Preson &p1,Preson &p2)
  {
      return p1.id > p2.id;
  }
};
void text()
{
   /*set<Preson> pst;//set是自动排序的这样的话set不知道怎么排序，按照什么排序
   
    Preson p1(10,20),p2(20,30),p3(30,40),p4(40,50);
    
    pst.insert(p1);
    pst.insert(p2);
    pst.insert(p3);
    pst.insert(p4);*/
    
    set<Preson，mycompare> pst;    //根据id排序   
    Preson p1(10,20),p2(20,30),p3(30,40),p4(40,50);
    
    pst.insert(p1);
    pst.insert(p2);
    pst.insert(p3);
    pst.insert(p4);
    for(set<Preson,mucompare>::itrerator it = pst.begin();it != pst.end();it++)
    {
        cout<<it->age<<" "<< it->id<<endl;
    }
    set<Presom,mycompare>::iterator ret = pst.find(p4);   //find根据id查找因为是根据id排序的
}
```

## map

**map与multimap特性**  自动排序

 map相对于set区别，map具有**键值**和**实值**，所有元素**根据键值自动排序**，pair的**第一元素被称为键值*****第二元素被称为实值**，map也是以红黑树为底层实现机制

multimap **key是能重复的**

**map不允许通过迭代器改变键值**因为，map是有序的集合，如果任意改变map的键值，会严重破坏map的组织，但是可以改变实值

map与multimap区别在于，map不允许相同的key值存在，multimap允许相同的key值存在

**对组**

对组（pair）将一对值组合成一个值，这一对值可以具有不同的数据类型，两个值可以分别用pair的两个公有函数first和second访问

**创建对组**

```c++
//第一种方法创建一个对组
pair<string ,int> p1(string("name"),20);
cout<< pair.first << endl;  //访问pair第一个值
cout<< pair.second<<endl;   //访问pair第二个值
//第二种
pair<string ,int> p2=make_pair("name",30);
cout<< pair.first << endl;  //访问pair第一个值
cout<< pair.second<<endl;   //访问pair第二个值
//第三种
pair<string,int> p3 = p2;
cout<< pair.first << endl;  //访问pair第一个值
cout<< pair.second<<endl;   //访问pair第二个值 
```



类模板：template<class t1,class t2> struct pair;

```c++

```

**map常用API**

```c++
map<T1,T2> m;                 //默认构造函数
map(const map &m);            //拷贝构造

void text()
{
    //map容器模板参数，第一个参数是key的类型，第二个参数是value类型
    map<int,int> mymap;
    
    //插入排序  pair.fist key键值 pair.second value类型
    //第一种
    mymap.insert(pair<int,int>(10,10));
    pair<map<int,int>::iterator,bool> ret = mymap.insert(pair<int,int>(10,10));
    if(ret.second)
    {
        cout<<"插入成功"<<endl;
    }else
    {
        cout<<"插入失败"<<endl;
    }
     pair<map<int,int>::iterator,bool> ret = mymap.insert(pair<int,int>(10,20));
     if(ret.second)
    {
        cout<<"插入成功"<<endl;
    }else
    {
        cout<<"插入失败"<<endl;
    }
    //第二种
    mymap.insert(make_pair(20,20));
    //第三种
    mymap.insert(map<int,int>::value_type(30,30));
    //第四种
    mymap[40]=40;
    mymap[10]=20;
    //如果没有发现key不存在，创建pair插入到map容器中
    //如果发现key存在，那么会修改key对应的value
    //打印
    
  for(map<int,int>::iterator it = mymap.begin();it!=mymap.end();it++)
  {
   //*it取出来的是一个pair
    cout << "key" <<it->first<<" "<<"value"<<it->second <<endl;
  }
    //如果通过[]方式去访问map中一个不存在key
    //那么map会将这个访问的key插入到map中并且给value一个默认值0
    cout<<"mymap[60]:" <<mymap[60] <<endl;
    
     for(map<int,int>::iterator it = mymap.begin();it!=mymap.end();it++)
  {
   //*it取出来的是一个pair
    cout << "key" <<it->first<<" "<<"value"<<it->second <<endl;
  }
```

map赋值操作

```c++
map &operator=(const map &m); //重载等号操作符
swap(m);					  //交换两个集合容器
```

**map大小操作**

```c++
size();                       //返回容器中元素数目
empty();					  //判断容器是否为空
```

**map插入数据元素操作**

```c++
insert();
```

**map查找操作**

```c++
find(key);        		//查找键key是否存在，若存在返回该键元素的迭代器，若不存在返回map.end();
count(keyElem);   //返回容器中key为keyElem的对组的个数，对map来说要么是0要么是1对于multimap来说可能大于1
lower_bound(keyElem);   //返回第一个key<=keyElem元素的迭代器 *解引用即可
upper_bound(keyElem);   //返回第一个key>keyElem元素的迭代器  *解引用即可
equal_range(keyElem);   //返回容器中key与keyElem相等的上下限的两个迭代器


void text()
{
    map<int,int> mymap;
     mymap.insert(make_pair(1,4));
     mymap.insert(make_pair(2,5));
     mymap.insert(make_pair(3,6));
pair<map<int,int>::itrator,map<int,int>::iterator>mmap.equal_range(2) ret = mymap.equal_range(2);
    if(ret.first->second)
    {
        cout<<"lower_bound";
    }else
    {
        cout<<"no";
    }
        if(ret.second->second)
    {
        cout<<"lower_bound";
    }else
    {
        cout<<"no";
    }
    
}
```

**map补充**

```c++
class MyKey
{
    public: 
    MyKey(int index;int id):mIndex(index),mID(id){};
    public: 
    int mIndex;
    int mID;
}
class mycompare
{
    bool operator()(MyKey key1,MyKey key2)
    {
        return key1.mIndex > key2.mIndex;
    }
}
void text02()
{
   /* map<MyKey,int> mymap;            //自动排序，咋排？？？？
    mymap.insert(make_pair(MyKey(1,4),10));
    mymap.insert(make_pair(MyKey(2,5),20));
    mymap.insert(make_pair(MyKey(3,6),30));*/
    map<MyKey,int,mycompare> mymap;            
    mymap.insert(make_pair(MyKey(1,4),10));
    mymap.insert(make_pair(MyKey(2,5),20));
    mymap.insert(make_pair(MyKey(3,6),30));
    for(map<MyKey,int,mycompare>::iterator it = mymap.begin();it != mymap.end();it++)
    {
        cout<< it->first.mIndex<<endl;
    }
}
```



**multimap案例**

```c++
#include<iostream>
#include<map> 
#include<set>
#include<vector>
#include<string>
#include<time.h>
#include<stdlib.h>
using namespace std;

//公司招用5个员工，5名员工进入公司后，需要指派员工在那个部门工作
//人员信息有：姓名、年龄、电话、工资
//通过multimap 进行信息的插入 保存 显示
//分部门显示员工信息 显示全部员工信息

#define SALE_DEPATMENT 1   //销售部门
#define DEVELOP_DEPATMENT 2 //研发部门
#define FINACIAL_DEPATMENT 3 //财务部门

class Worker
{
public:
	string name;
	string mTele;
	int age;
	int mSalary;
};

void Create_Worker(vector<Worker> &vWorker)
{
	string name = "abcde";
	for (int i = 0; i < 5; i++)
	{
		Worker worker;
		worker.name = "员工";
		worker.name += name[i];

		worker.age = rand() % 10 + 20;
		worker.mTele = "010-88888888";
		worker.mSalary = rand() % 10000 + 10000;
		vWorker.push_back(worker);
	}
}
//员工分组
void WorkerByGroup(vector<Worker>& vWorker, multimap<int, Worker>   &workGroup)
{
	srand(time(NULL));
	//把员工部门分配不同的组
	for (vector<Worker>::iterator it = vWorker.begin(); it != vWorker.end(); it++)
	{
		int depareID = rand() % 3 + 1;
		switch (depareID) 
		{
		case 1:workGroup.insert(make_pair(SALE_DEPATMENT, *it));
			break; 
		case 2:workGroup.insert(make_pair(DEVELOP_DEPATMENT, *it));
			break;
		case 3:workGroup.insert(make_pair(FINACIAL_DEPATMENT, *it));
			break;
		default:
			break;
		}
	}
}

void ShowWorks(multimap<int, Worker>& workGroup,int departID)
{
	 //打印销售部员工的信息
	multimap<int, Worker>::iterator it = workGroup.find(departID);
	int DepartCount = workGroup.count(SALE_DEPATMENT);
	int num = 0;
	for (multimap<int, Worker>::iterator pos = it; it != workGroup.end() && num < DepartCount; pos++, num++)
	{
		cout << pos->second.name << " " << pos->second.age << " " << pos->second.mTele << " " << pos->second.mSalary<<endl;
	}
}
//打印一部分员工信息
void PrintWorkerByGroup(multimap<int, Worker>   &workGroup)
{
	ShowWorks(workGroup,SALE_DEPATMENT);
	cout << 1 << endl;
	ShowWorks(workGroup, DEVELOP_DEPATMENT);
	cout << 1 << endl;
	ShowWorks(workGroup, FINACIAL_DEPATMENT);
}
int main()
{
	vector<Worker> vWorker;
	//创建员工
	//multimap保存分组信息
	multimap<int, Worker>   workGroup;
	Create_Worker(vWorker);***
	//员工分组
	WorkerByGroup(vWorker,workGroup);
	//打印一部分员工信息
	PrintWorkerByGroup(workGroup);
}
```

## 容器的深拷贝浅拷贝问题

类中有指针，如果要将对象放入容器，需要自己写赋值，拷贝构造函数否则会引发浅拷贝问题；

```c++
class Preson 
{
public:
	Preson(const char* n, int ge)
	{
		this->age = ge;
		name = new char[strlen(name)+1];
		strcpy(this->name, n);
	}
	Preson(const Preson& p)
	{
		this->name = new char[strlen(name) + 1];
		strcpy(this->name, p.name);
	}
	Preson& operator=(const  Preson &p)
	{
		if (nullptr != this->name)
		{
			delete[] this->name;
		}this->age = p.age;
		name = new char[strlen(name) + 1];
		strcpy(this->name, p.name);
	}
	~Preson()
	{
		if (nullptr != this->name)
		{
			delete[] name;
		}
		this->name = nullptr;
	}
public:
	char *name;
	int age;
};
int main()
{
	Preson p("AAA",5vector<Preson> pp;
	pp.push_back(p);
}
```

# 线程

## 线程的简单使用

创建的子线程可以共享住线程的堆区，代码区，数据区。

线程的拥有权和线程的转移不矛盾，在任何时间都可以执行。

**线程的活跃指的是**：无论阻塞，将亡，执行，只要句柄和内核关联就叫活跃。

```c++
#incliude<iostream>
#include<thread>
using namespace std;                        //简单的使用
void fun(int n)
{
	for (int i = 0; i < 5; i++)
	{	cout << "n1:" << n << endl;
		n++;
    }
}
void fun1(int n)
{
	for (int i = 0; i < 5; i++)
	{
		cout << "n2:" << n << endl;
		n++;
	}
}
int main()
{
	thread t1(fun, 0);
	thread t2(fun1, 0);
	t1.join();
    t2.join();
}
```

![image-20210418132906146](C:\Users\kaitian\AppData\Roaming\Typora\typora-user-images\image-20210418132906146.png)

**线程的活跃指的是**：无论阻塞，将亡，执行，只要句柄和内核关联就叫活跃。

  **t1.detach();**  当线程被剥离的时候，主线程结束，此线程会被强制结束。

```c++
void fun()
{
	cout << "fun" << this_thread::get_id() << endl;
}
int main()
{
	thread t1(fun);
	cout << "main" << this_thread::get_id() << endl;
	cout << "main" << t1.get_id() << endl;
	cout << t1.joinable() << endl;   //判断是否活跃
	t1.join();
	cout << t1.joinable() << endl;
	return 0;
}
-------------------------------------------
 main:22316	
 mian:1860
 1
 fun:1860
 0
 -------------------------------------------
 int main()
 {
     thread t1(fun);
     cout<<t1.joinable()<<endl;
     t1.detach();
     cout<<t1.joinable()<<endl;
     return 0;
 }
```

### 资源竞争

```c++
void fun(int &x)
{
	for (int i = 0; i < 10; i++)
	{
		cout << x << endl;
		x++;
	}
}
int main()
{
	int x = 10;
	thread t1(fun, std::ref(x));
	thread t2(fun, std::ref(x));
	t1.join();
	t2.join();
	return 0;
}
---------------------------------------
输出结果不确定，出现了资源的竞争，非原子性
解决方案：
 mutex g_mut;
 void fun(int &x)
{
     g_mut.lock();
	for (int i = 0; i < 10; i++)
	{
		cout << x << endl;
		x++;
	}
     g_mut.unlock();
}
int main()
{
	int x = 10;
	thread t1(fun, std::ref(x));
	thread t2(fun, std::ref(x));
	t1.join();
	t2.join();
	return 0;
}   
```

### 线程化调用对象的函数

```c++
class Object
{
private:
	int value;
public:
	Object(int x = 0):value(x){}
	~Object(){}
	void show()const
	{
		cout << "show" << value << endl;
	}
};
int mian()
{
	Object obj(10);
	thread t1(&Object::show,&obj);
	t1.join();
	return 0;
}
```

### 或者

```c++
class Object
{
private:
	int value;
public:
	Object(int x = 0):value(x){}
	~Object(){}
	void show()const
	{
		cout << "show" << value << endl;
	}
};
void fun()
{
    Object obj(10);
}
int mian()
{
	Object obj(10);
	thread t1(fun);
	t1.join();
	return 0;
}
```

### 线程调用仿函数

```c++
#include <iostream>
#include<thread>
#include<mutex>
using namespace std;
class Object
{
public:
	void operator()()
	{
		for (int i = 0; i < 10; i++)
		{
			cout << "operator" << endl;
		}
	}
};
int mian()
{
	Object obj;
	thread t1(std::ref(obj));
	t1.join();
	return 0;
}
```

### 将对象线程化

```c++
class Object
{
public:
	static void run()
	{
		Object obj;
	}
};
int main()
{

	thread t1(Object::run);
	t1.join();
	return 0;
}
```

### 锁的简单使用

**std::lock_guard<std::mutex> lock(g_mut);**   当函数自行完毕解锁。

```c++
#include <iostream>
#include<thread>
#include<mutex>
using namespace std;
mutex g_mut;
void fun(char ch)
{
	std::lock_guard<std::mutex> lock(g_mut);
	for (int i = 0; i < 5; i++)
	{
		int j = 0;
		while (j < 10)
		{
			cout << ch;
			j++;
		}
		cout << endl;
	}
	
}
int main()
{
	thread mth[5];
	for (int i = 0; i < 5; i++)
	{
		g_mut.lock();
		mth[i]=thread(fun,'a'+i);
		g_mut.unlock();
	}
	for (int i = 0; i < 5; i++)
	{
		mth[i].join();
	}直至
}
```

