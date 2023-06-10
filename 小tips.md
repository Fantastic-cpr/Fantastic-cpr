1.[Node.js (nodejs.org)](https://nodejs.org/zh-cn)

Node.js® is an open-source, cross-platform JavaScript runtime environment.

Node.js® 是一个开源、跨平台的 JavaScript 运行时环境。

![image-20230326194337821](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230326194337821.png)

2.typeof（C# 参考）

用于获取类型的 **System.Type** 对象。**typeof** 表达式采用以下形式：

```
System.Type type = typeof(int);
```

3.获取字典中的key值

```c#
private Dictionary<string, Dictionary<string, SkinnedMeshRenderer>> girlData = new Dictionary<string, Dictionary<string, SkinnedMeshRenderer>>();
string name = AvatarSystem.instance.GirlData.ElementAt(i).Key;
string num = AvatarSystem.instance.GirlData[_name].ElementAt(i).Key;
```

4.注释

方法一：Ctrl + /

方法二：开启注释 ctrl + K再加上C消除注释 ctrl + K 再加上 U
