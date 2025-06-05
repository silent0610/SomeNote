---
Type:
  - Page
aliases: 
tags: 
modifiedDate: 星期六, 五月 31日 2025, 1:27:35 下午
---
[C++的三种继承方式:public,protected,private - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/356580826)
基类的private 成员在子类中无论如何都无法直接访问（通过基类public函数（非private继承）间接访问应该可以）
***注意区分对象，和类内。***
***private成员相当于私有，只允许自身成员访问，不允许子类直接访问，必须借助基类成员函数。***
***protect成员则允许子类访问。***
private继承将所有成员变为private，对于对象，无法直接访问任何父类成员。对于类内，可以访问非原private成员。
public继承不改变基类成员属性。对于对象，基类的private成员和protect成员不可直接访问。对于类内，对于类内，可以访问非private成员。
protect继承将所有成员变为protected。对于对象，无法直接访问任何父类成员。对于类内，可以访问非private成员。

>public（公有的）：父类中私有成员子类不可访问，公有及保护成员在子类中保持原有访问状态。
>private（私有的）：父类中私有成员子类不可访问，公有及保护成员为子类中的私有成员（即只能通过子类内的成员函数访问）。
>protect（保护的）：父类中的私有成员子类不可访问，公有及保护成员为子类中的保护成员。
