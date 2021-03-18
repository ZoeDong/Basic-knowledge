[TOC]

# String

## 头文件/声明

### C、C++头文件与命名空间
|头文件类型|约定|示例|说明|
|--|--|--|--|
|C旧式风格|以.h结尾|math.h|C、C++程序可以使用|
|C++旧式风格|以.h结尾|iostream.h|C++程序可以使用,不需声明命名空间，定义的所有类以及对象都是在全局空间里，所以你可以直接用cout|
|转换后的C|加上前缀c，没有扩展名|cmath|C++程序可以使用，可以使用不是C的特性，如namespace std|
|C++新式风格|没有扩展名|iostream|C++程序可以使用，使用namespace std，定义的东西都在名字空间std里面|

using namespace std //使用名字空间（使用所有）
using namespace std::cout//只使用cout
如不用using，则在代码前可以用sdt::cout<<表示使用的是std中的cout。


### string、string.h、cstring三者的区别与联系 
C++要兼容C的标准库，而C的标准库里碰巧也已经有一个名字叫做“string.h”的头文件，包含一些常用的C字符串处理函数，比如strcmp；这个头文件跟C++的string类半点关系也没有，所以<string>并非<string.h>的“升级版本”，他们是毫无关系的两个头文件。

- **string**：C++头文件，包含了C风格字符串操作的库函数，**可以定义string类**，使用时**需**声明namespace std
- **string.h**：C风格字符串操作的一个库函数，对应的是基于char*的字符串处理函数，**不能定义string类**，使用时**不需**声明namespace std
- **cstring**：把string.h放到std中，它的功能和string.h一样，因为使用了std，所以使用时需要声明namespace std。

我们在使用头文件时，为了统一，应该遵循以下原则。
- 当使用C的库函数时，应该在原C的库函数名前加字符C，把后缀.h去掉。例如，
```C++
　　//#include<string.h>应变为
　　#include<cstring>
　　using namespace std;  
```
- 当使用C++的函数库时，直接使用不要加后缀.h。例如，
```C++
　　#include<string>
　　using namespace std;
```


## 常用函数
### 构造函数
```C++
string str1;               //生成空字符串
string str2("123456789");  //生成"1234456789"的复制品
string str3("12345", 0, 3);//结果为[0,3)的子串"123"
string str4("0123456", 5); //结果为[0,5)的子串"01234"
string str5(5, '1');       //结果为"11111"
string str6(str2, 2);      //结果为[2,结束]的子串"3456789"
```

### 大小和容量
```C++
string s("1234567");
s.size(); //7
s.length(); //7：返回string对象的字符个数，和length()执行效果相同。
s.max_size(); //返回string对象最多包含的字符数，超出会抛出length_error异常
s.capacity(); //15：重新分配内存之前，string对象能包含的最大字符数
```

### 字符串比较
```C++
//A.compare(B.下标,B.长度, B, A.下标,A.长度)
//A减去B的ASCII码，>0返回1，<0返回-1，相同返回0
// (A的ASCII码是65，a的ASCII码是97)

string A("aBcd");
string B("Abcd");
string C("123456");
string D("123dfg");

// "aBcd" 和 "Abcd"比较------ a > A
cout << "A.compare(B)：" << A.compare(B)<< endl;                          // 结果：1

// "cd" 和 "Abcd"比较------- c > A
cout << "A.compare(2, 3, B):" <<A.compare(2, 3, B)<< endl;                // 结果：1

// "123" 和 "123"比较 
cout << "C.compare(0, 3, D, 0, 3)" <<C.compare(0, 3, D, 0, 3) << endl;    // 结果：0
```

### 插入
```C++
//push_back()  尾插一个字符
s1.push_back('a');
s1.push_back('b');
s1.push_back('c');// s1:abc

//insert(pos,char) 在pos前插入char/char*/string
s1.insert(s1.begin(),'1');// s1:1abc
```

### 拼接字符串
```C++
//append() 
string s1("abc");
s1.append("def");// s1:abcdef

//+ 操作符
string s2 = "abc";
string s3 = "def";
s2 += s3;// s2:abcdef
```

### 遍历：下标和※迭代器
```C++
string s1("abcdef"); // 调用一次构造函数

// 方法一： 下标法
for( int i = 0; i < s1.size() ; i++ )
    cout<<s1[i];

// 方法二：正向迭代器
string::iterator iter = s1.begin();
for( ; iter < s1.end() ; iter++)
    cout<<*iter;

// 方法三：反向迭代器
string::reverse_iterator riter = s1.rbegin();
for( ; riter < s1.rend() ; riter++)
    cout<<*riter;
```

### 删除
```C++
iterator erase(iterator p);//删除字符串中p所指的字符
iterator erase(iterator first, iterator last);//删除字符串中迭代器区间[first,last)上所有字符
string& erase(size_t pos = 0, size_t len = npos);//删除字符串中从索引位置pos开始的len个字符
void clear();//删除字符串中所有字符
```

```C++
string s1 = "123456789";
s1.erase(s1.begin()+1);              // 结果：13456789
s1.erase(s1.begin()+1,s1.end()-2);   // 结果：189
s1.erase(1,6);                       // 结果：189
```

### 字符替换
```C++
string& replace(size_t pos, size_t n, const char *s);//将当前字符串从pos索引开始的n个字符，替换成字符串s
string& replace(size_t pos, size_t n, size_t n1, char c); //将当前字符串从pos索引开始的n个字符，替换成n1个字符c
string& replace(iterator i1, iterator i2, const char* s);//将当前字符串[i1,i2)区间中的字符串替换为字符串s
```

```C++
string s1("hello,world!");
s1.replace(s1.size()-1,1,1,'.');           // 结果：hello,world. （s1.size()=12）

// 这里的6表示下标，5表示长度；s1.begin(),s1.begin()+5 是左闭右开区间
s1.replace(6,5,"girl");                    // 结果：hello,girl. 
s1.replace(s1.begin(),s1.begin()+5,"boy"); // 结果：boy,girl.
```

### 查找
```C++
//字符串查找-----找到后返回首字母在字符串中的下标

//在当前字符串的pos索引位置开始，查找子串s/字符c，返回找到的位置索引，-1表示查找不到子串
size_t find (const char* s/char c, size_t pos = 0) const;

//查找char*的前n个子串
size_t find(const char*,size_type pos=0,size_type n)

//在当前字符串的pos索引位置开始，反向查找子串s/字符c，返回找到的位置索引，-1表示查找不到子串
size_t rfind (const char* s/char c, size_t pos = npos) const;


//在当前字符串的pos索引位置开始，查找子串s的字符，返回找到的位置索引，-1表示查找不到字符
size_tfind_first_of (const char* s, size_t pos = 0) const;

//在当前字符串的pos索引位置开始，查找第一个不位于子串s的字符，返回找到的位置索引，-1表示查找不到字符
size_tfind_first_not_of (const char* s, size_t pos = 0) const;

//在当前字符串的pos索引位置开始，查找最后一个位于子串s的字符，返回找到的位置索引，-1表示查找不到字符
size_t find_last_of(const char* s, size_t pos = npos) const; 

//在当前字符串的pos索引位置开始，查找最后一个不位于子串s的字符，返回找到的位置索引，-1表示查找不到子串
size_tfind_last_not_of (const char* s, size_t pos = npos) const;
```

```C++
string s("dog bird chicken bird cat");

// 1. 查找一个字符串
cout << s.find("chicken") << endl;        // 结果是：9

// 2. 从下标为6开始找字符'i'，返回找到的第一个i的下标
cout << s.find('i',6) << endl;            // 结果是：11

// 3. 从字符串的末尾开始查找字符串，返回的还是首字母在字符串中的下标
cout << s.rfind("chicken") << endl;       // 结果是：9

// 4. 从字符串的末尾开始查找字符
cout << s.rfind('i') << endl;             // 结果是：18-------因为是从末尾开始查找，所以返回第一次找到的字符

// 5. 在该字符串中查找第一个属于字符串s的字符
cout << s.find_first_of("13br98") << endl;  // 结果是：4---b

// 6. 在该字符串中查找第一个不属于字符串s的字符------先匹配dog，然后bird匹配不到，所以打印4
cout << s.find_first_not_of("hello dog 2006") << endl; // 结果是：4
cout << s.find_first_not_of("dog bird 2006") << endl;  // 结果是：9

// 7. 在该字符串最后中查找第一个属于字符串s的字符
cout << s.find_last_of("13r98") << endl;               // 结果是：19

// 8. 在该字符串最后中查找第一个不属于字符串s的字符------先匹配t--a---c，然后空格匹配不到，所以打印21
cout << s.find_last_not_of("teac") << endl;            // 结果是：21
```

### 排序
```C++
// sort(s.begin(),s.end())

string s = "cdefba";
sort(s.begin(),s.end());// 结果：abcdef
```

### ※分割/截取字符串：strtok() & substr()
```C++
char str[] = "I,am,a,student; hello world!";

const char *split = ",; !";
char *p2 = strtok(str,split);
while( p2 != NULL ){
    cout<<p2<<endl;
    p2 = strtok(NULL,split);
}

string s1("0123456789");
string s2 = s1.substr(2,5); // 结果：23456-----参数5表示：截取的字符串的长度
```


### ※大小写转换
#### 使用C语言之前的方法，使用函数，进行转换
```C++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string s = "ABCDEFG";
    for( int i = 0; i < s.size(); i++ ){
        s[i] = tolower(s[i]);
    }
    cout<<s<<endl;
    return 0;
}
```

#### 通过STL的transform算法配合的toupper和tolower来实现
transform函数：在指定的范围内应用于给定的操作，并将结果存储在指定的另一个范围内。包含在\<algorithm\>头文件中。

以下是std::transform的两个声明，

一元操作：
```C++
template <class InputIterator, class OutputIterator, class UnaryOperation>
OutputIterator transform (InputIterator first1, InputIterator last1,OutputIterator result, UnaryOperation op);
```
将op应用于[first1, last1]范围内的每个元素，并将每个操作返回的值存储在以result开头的范围内。给定的op将被连续调用last1-first1+1次。op可以是函数指针或函数对象或lambda表达式。

例如：op的一个实现 即将[first1, last1]范围内的每个元素加5，然后依次存储到result中。
```C++
int op_increase(int i) {return (i + 5)};
std::transform(first1, last1, result, op_increase);
```

二元操作：
```C++
template <class InputIterator1, class InputIterator2,class OutputIterator, class BinaryOperation>
OutputIterator transform (InputIterator1 first1, InputIterator1 last1,InputIterator2 first2, OutputIterator result,BinaryOperation binary_op);
```
使用[first1, last1]范围内的每个元素作为第一个参数调用binary_op,并以first2开头的范围内的每个元素作为第二个参数调用binary_op,每次调用返回的值都存储在以result开头的范围内。给定的binary_op将被连续调用last1-first1+1次。binary_op可以是函数指针或函数对象或lambda表达式。

例如：binary_op的一个实现即将first1和first2开头的范围内的每个元素相加，然后依次存储到result中。
```C++
int op_add(int, a, int b) {return (a + b)};
std::transform(first1, last1, first2, result, op_add);
```

```C++
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;

int main(){
    string s = "ABCDEFG";
    transform(s.begin(),s.end(),s.begin(),::tolower);
}
```




# 字符串
字符串翻转
字符串长度
字符串复制