[TOC]

### 内存分区
首先说一下内存的五个区：

    栈(stack)：由编译器自动分配释放，存放函数的参数值，局部变量的值（除static），其操作方式类似于数据结构中的栈。
    
    堆(heap)：一般由程序员分配释放，若程序员不释放，程序结束时可能由OS回收。注意它与数据结构中的堆(优先队列)是两回事，分配方式倒是类似于链表。
    
    全局区(静态区)：全局变量和静态变量的存储是放在一块的，初始化的全局变量和静态变量在一块区域，未初始化的全局变量和未初始化的静态变量在相邻的另一块区域(BSS)，程序结束后由系统释放。
    
    文字常量区：常量字符串就是放在这里的，如char str[]=”hello”，程序结束后由系统释放，区别const修饰的变量。
    
    程序代码区：存放函数体的二进制代码。
### static知识
从一个例子入手,去讲一些关于static的知识点,首先看一个例子(这个例子可以直接复制到qt中运行)

```c
    #include <QCoreApplication>
    #include <iostream>
    using namespace std;
    class A
    {
    public:
         // 声明static变量，任何声明都不可初始化，如extern外部变量
        static int a;
    private:
        static int b;
        int test;
    public:
        static int getAValue()
        {
            test=1;      //错误,静态方法只能访问静态变量和静态方法
            return a;
        }
        int get_B(void)
        {
            return b;
        }
    };
   // 定义**static成员变量，可初始化
    int A::a = 5;
    // 定义私有静态成员变量
    int A::b = 1;     //这个是定义
    int main(int argc, char *argv[])
    {
        // new 两个个实例（对象）
        A * instanceA = new A();
        A * instanceB = new A();

        // 改变值,均输出1
        instanceA->a = 1;
        //instanceA->b = 2;    //报错,私有静态成员变量需通过类的函数(静态或者非静态都可以)去访问
        cout << A::a << endl;
        cout << A::b << endl;  //错误
        cout << instanceA->getAValue() << endl;
        cout << instanceB->getAValue() << endl;
        cout << instanceB->get_B()<<endl;
        return 0;
    }
```
输出结果:
1
1
1
1
按 <RETURN> 来关闭窗口...

**由此我们得到如下结论:**



- **静态成员(包括变量和函数)同样遵守public,private,protected访问权限的限定规则**. 

- 静态数据成员是属于类的,整个类只有一份拷贝,相当于类的全局变量,供该类所有对象共用,能够被该类的所有对象访问,非静态数据成员是属于对象的,每个对象都有非静态数据成员的一份拷贝,为该对象专用.   

- 静态成员函数也是属于整个类的,它**只能访问属于该类的静态成员**(包括静态数据成员和静态成员函数),不能访问非静态成员(包括非静态的数据成员和成员函数)

&ensp;&ensp;&ensp;&ensp;这里主要参考了自己那本黄皮C++的P79-P82,和[这篇博客](http://blog.csdn.net/freeape/article/details/50979425)(这篇博客的代码有的地方有错,以我自己的代码为准)有些地方需要说明下,在P80的静态成员的访问中有两种方法,我觉得说的是公有的,私有的静态成员或函数是不能直接通过类名::静态数据成员名(或者函数名)访问的,也不能通过对象名访问,自己试过.
&ensp;&ensp;&ensp;&ensp;另外还有一个地方需要说明,有的时候会遇到在函数内部定义static变量,即局部变量定义为static.如果将成员函数内的某个局部变量定义为静态变量，该类的所有对象在调用这个成员函数时将共享这个变量。可以参考[这个博客](http://blog.csdn.net/u012317833/article/details/41011997),自己在跑ElasticFusion的时候,利用到的单件模式大多也都是在类的函数内定义一个局部的static变量来保证对象唯一性的.
&ensp;&ensp;&ensp;&ensp;自己想写的就这么多,如果还有不太明白的,可以去看我提到的课本页数和参考博客,我觉得我那本薄的C++书上内容挺多的.