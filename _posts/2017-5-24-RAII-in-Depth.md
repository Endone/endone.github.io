---
layout: post
title: RAII (Resource Acquisition Is Initilization)
---
Resource Acquisition Is Initilization的字面意思是“资源的获取即初始化”，其起源于C++，简单的理解就是所有资源的分配和释放都应该与对象的生命周期绑定在一起，在构造函数中完成分配，析构函数中完成释放。

这么做的优点很明显：
* 资源始终能够得到释放，只要对象正确的析构；
* 代码逻辑更明确，可以将异常处理和资源释放分离开，提高可读性；
看例子：
* 非RAII
```C++
FileResourceHandle* handler = createFileResourceHandler(path);
handler.parse();
......
deleteFileResource(handler);
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
