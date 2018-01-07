##Static用法

在类中,若在数据成员或者成员函数的声明或定义前面加上关键字static,就将它定义成了静态数据成员或静态成员函数. ==静态成员同样遵守public,private,protected访问权限的限定规则==.

静态数据成员是属于类的,整个类只有一份拷贝,相当于类的全局变量,供该类所有对象共用,能够被该类的所有对象访问,非静态数据成员是属于对象的,每个对象都有非静态数据成员的一份拷贝,为该对象专用.

静态成员函数也是属于整个类的,它只能访问属于该类的静态成员(包括静态数据成员和静态成员函数),不能访问非静态成员(包括非静态的数据成员和成员函数)

接下来将从一个例子入手,去讲一些关于static的知识点,首先看一个例子(这个例子可以直接复制到qt中运行)

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
    public:
        static int getAValue()
        {
            return a;
        }
        int get_B(void)
        {
            return b;
        }
    };
   // 定义static成员变量，可初始化
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
        cout << instanceA->getAValue() << endl;
        cout << instanceB->getAValue() << endl;
        cout << instanceB->get_B()<<endl;
        return 0;
    }
```
下面是输出结果:
![](/home/zhang/123.png) 


***
