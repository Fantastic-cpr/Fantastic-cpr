char 2，bool , 

byte 1, short 2 , int 4, long  4/8,

float 4 ,double 8

Byte[]怎么将一个 类 变成一个 字节流：

```c#
BitConverter.GetBytes(); 
Buffer. BlockCopy();//相当于c/c++的memcopy 
```



地址传参 用ref

```c#
void func(ref int num)
```



