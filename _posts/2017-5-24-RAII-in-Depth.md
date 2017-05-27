---
layout: post
title: RAII (Resource Acquisition Is Initilization)及延伸
---
## RAII
Resource Acquisition Is Initilization的字面意思是“资源的获取即初始化”，其起源于C++，简单的理解就是所有资源的分配和释放都应该与对象的生命周期绑定在一起，在构造函数中完成分配，析构函数中完成释放。

这么做的优点很明显：
* 资源始终能够得到释放，只要对象正确的析构；
* 代码逻辑更明确，可以将异常处理和资源释放分离开，提高可读性；
看例子：
* 非RAII

```C++
void test(){
FileResourceHandle* handler = createFileResourceHandler(path);
handler.parse();
......
deleteFileResource(handler);
}
```
问题是在create和delete之间如果有任何异常，资源将不能得到释放，而且C++中并没有finally可以在异常的情况下保证资源的释放，所以就有了RAII：

```C++
class FileResourceHandlerManager{
public:
	FileResourceHandlerManager(FileResourceHandle* handler):handler_(handler){}
	~FileResourceHandleManager(){delete handler_;}
	parse(){handler_.parse();}
	......
private:
	FileResourceHandle* handler_;
}

FileResourceHandleManager manager(createFileResourceHandler(path));
manager->parse();
......
```
通过代码可以看出来，C++中实现RAII的方法很简单：
* 将资源类化，即将资源包装为一个类中，并通过指针的形式来定位资源；
* 在构造函数中初始化资源，在析构函数(对象出作用域的时候)中释放资源；

很显然，在C++中可以这么做的原因是内存的分配和释放完全由我们掌控，即deterministic destruction，内存的分配和释放都是确定的，如对象在new的时候进行内存分配，在delete的时候进行内存释放，而对于内存管理被托管的语言如Java、C#，内存的分配是deterministic的，但是其释放是由GC控制对的，GC在合适的点将内存回收或者重新利用，所以我们无法写出符合RAII的代码，但是在较新版本的Java和C#中，都添加了对RAII的语法支持，Java在1.7之后，添加了try()的语法：
```Java
try(File file = new File(path)){
	perform(file);
}
```
同时C#中也有类似try的功能的using语法，这些语法糖都是对RAII的使用。
```
using(Entity en = new Entity){
    entity.refresh();
    // Entity需要实现IDisposable接口
    ......
}
```

## Object Lifetime and Determinism
从上面的RAII的示例可以看出，对象的生命周期是否确定是一个重要因素。对象的生命周期是指从被创建到被销毁的过程，对应如constructor/destructor，initialize/finalize，allocation and deallocation等概念，在很多基本类型的情况下，对象的生命周期和其对应变量的生命周期一致，变量出作用域，对象就被销毁，但是在很多面向对象的语言中，两者并没有确定性的关系，对象被创建在堆上，而变量的内容只是一个引用，一个地址，变量的销毁只是引用的销毁，真正的对象合适得以销毁是不确定的，尤其是在GC存在的情况下，GC更多的是根据Heap Pressure来执行内存回收，所以其对象的生命周期是不确定的。
 

##### Reference:
[StackOverflow](https://stackoverflow.com/questions/2321511/what-is-meant-by-resource-acquisition-is-initialization-raii)
[Microsoft Developer](https://blogs.msdn.microsoft.com/brada/2005/02/11/resource-management/)