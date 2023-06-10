### c++与#define区别

编译时区别：const在编译时分配内存；

#define在预编译时进行替换，不分配内存；

作用域范围：const定义变量的作用域范围；

#define从定义点到程序结束或#undef；



高层次 const enum inline替换#define

底层编程 #define更加灵活

MFC框架 宏 # ##

泛型本质也是替 换