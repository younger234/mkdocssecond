# 面向对象程序设计

关于C++的学习，使用c++菜鸟教程网站，[click here](https://www.runoob.com/cplusplus/cpp-vector.html)

我们先从第一个程序开始
```cpp
#include<iostream>
using namespace std;

string str1 = "foo";//赋值初始化
string str4("hello,china");//拷贝构造函数初始化
string str5(str3);//同样是拷贝构造函数初始化
string str6(str3,7,5);//从第七个位置开始初始化五个字符

string str7 = str3.substr(7,5);

string str8 = str4;
str8.replace(7,5,"zjg");//替换从第七个开始的五个字符"china"为"zjg"

string str8.assign(10,"A");
cout << "str8=" << str9 << endl;//str8=AAAAAAAAAA
string str_find="hanghzuo";
cout<< str9.find(str_find) << endl;
str9.replace(str.find(str_find),str_find.length(),"shanghai");//find the index and then replace
```


关于正则表达式
```cpp
int main(){
    string s = "Hello,Student@zju!"
    regex re("a|e|i|o|u");
    string s1 = regex_replace(s,re,"*");
}

```