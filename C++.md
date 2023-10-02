**拆包数组**

```c++
int a[3] = {1,2,3};
auto [x,y,z] = a;
```

**理解复杂的数组定义**

```c++
int *ptrs[10];   //数组，存10个int*指针
int &p1[10] = ...;  //错误 不存在引用数组
int (*Parray)[10] = &arr; //Parray指向一个含有10个整数的数组
int (&arrRef)[10] = arr; //arrRef引用一个含有10个整数的数组
int *(&arry)[10] = ptrs; //arry是数组的引用，该数组含有10指针
```

**访问数组元素**

```c++
数组下标的类型：size_t。cstddef头文件
用数组初始化vector:
int a[] = {1,2,3,4,5};
vector v(begin(a),end(a));
```

**数组和指针**

```c++
使用数组时，编译器一般会把它转换成指针。
指针访问数组：在表达式中使用数组名时，名字会
自动转换成指向数组的第一个元素的指针。
标准库函数 begin end。begin(ia) end(ia)
int ia[]={1,2,3};
auto n = end(ia) - begin(ia); //n为元素数量
内置下标运算符所用的索引不是无符号数
```

**多维数组**

```c++
严格来说C++没有多维数组，多维数组是数组的数组
```

**使用范围FOR语句处理多维数组**

```c++
//c++11新标准
size_t cnt=0;
for(auto &row:ia){
    for(auto &col:row){
        col=cnt;
        ++cnt;
    }
}
//能编译通过
for(const auto &row:ia){ //防止数组被自动转化成指针
    for(auto col:row){
        cout<<col<<endl;
    }
}
```

**指针和多维数组**

```c++
使用auto和decltype，能尽可能避免在数组前加指针类型

//输出ia中每个元素的值
//p指向含有4个整数的数组
for(auto p=ia;p!=ia+3;++p){
    //q指向4个整数数组的首元素，也就是说，q指向一个整数
    for(auto q=*p;q!=*p+4;++q)
        cout<<*q<<' ';
    cout<<endl;
}

使用标准库函数begin end 更简洁
//p指向ia的第⼀个数组
for(auto p=begin(ia);p!=end(ia);++p){                                            
    //q指向内层数组⾸元素   
    for(auto q=begin(*p);q!=end(*p);++q){
        cout<<*q<<' ';//输出q所指的整数值
    }
    cout<<endl;
}
//使⽤auto关键字 不再烦⼼类型是什么
```

**类型别名简化多维数组的指针**

```c++
using int_array = int[4];
```

**顶层const与底层const**

```c
顶层const：指针本身是个常量（*在const左边）

底层const：指针指向的对象是个常量。拷贝时严格要求相同的底层const资格。（*在const右边）
```

```c++
const int &r = i;//用于声明应用的const都是底层const
```

**使用GETLINE读取一整行**

```c++
string line;
while(getline(cin,line))
    cout << line << endl;
//getline：读取一整行，包括空白符
```

**STRING的EMPTY和SIZE操作**

```c
string line;
while(getline(cin.line))
    if(!line.empty())  // 每次读入一行，遇到空行直接跳过
        cout << line << endl;
   //if(line.size() > 80)
   //     cout << line << endl;
```

**STRING::SIZE_TYPE类型**

```c++
//s.size()返回string::size_type类型，是一个无符号类型
```

**处理STRING对象中的字符**

```c++
cctype头文件定义了一组标准库函数
C++修改了c的标准库，名称为去掉.h，前面加c。
```

**cctype头文件中定义了一组标准函数**

```c++
isalnum(c)  当c是字母或数字时为真
isalpha(c)  当c是字母时为真
iscntrl(c)  当c是控制字符时为真
isdigit(c)  当c是数字时为真
isgraph(c)  当c不是空格但可以打印时为真
islower(c)  当c是小写字母时为真
isprint(c)  当c是可打印字符时为真
ispunct(c)  当c是标点符号时为真
isspace(c)  当c是空白时为真（空格、横向制表符、纵向制表符、回车符、换行符、进纸符）
isupper(c)  当c是大写字母时为真
isxdigit(c) 当c是十六进制数字时为真
tolower(c)  当c是大写字母，输出对应的小写字母；否则原样输出c
toupper(c)  当c是小写字母，输出对应的大写字母；否则原样输出c
```

**处理每个字符 使用基于范围FOR语句**

```c++
for(declaration: expression)
    statement
expression是一个对象，用于表示一个序列
declaration负责定义一个变量，用于访问序列中的基础元素
每次迭代，declaration部分变量初始化expression部分的下一个元素值
如
for(auto c: str)
    statement
for(auto &c: str)  //使用引用直接改变字符串中的字符
    statement
```

**标准库类型VECTOR**

```c++
vector是一个容器，也是一个类模板
- 容器：包含其他对象。
- 类模板：本身不是类，但可以实例化出一个类。
    - vector是一个模板， vector<int>是一个类型。
- 类模板名字后面跟一对尖括号，在括号内放上信息来指定类型，
    - 如vector<int> ivec。
    - vector的元素还可以是vector
        - 如vector<vector<int>>
        - 如vector<vector<int> > 过去的写法，需要加空格
```

**列表初始化 元素数量**

```c++
//通过花括号 圆括号区分
vector<int> v1(10); //v1有10个元素，每个值都是0
vector<int> v2{10}; //v2有1个元素，值为10
vector<int> v3(10,1); //v3有10个元素，每个值为1
vector<int> v4{10,1}; //v4有2个元素，分别为10和1


vector<string> v5{"hi"}; //列表初始化，v5有一个元素
vector<string> v6("hi"); //错误，不能使用字符串字面值构建vector对象
vector<string> v7{10}; //v7有10个默认初始化元素
vector<string> v8{10,"hi"}; //v8有10个值为"hi"的元素
```

**向VECTOR对象中添加元素**

```c++
vector<int> v2;
for(int i=0;i!=100;i++)
    v2.push_back(i);
//循环结束后v2有100个元素，0-99


//从标准输入中读取单词，将其作为vector对象的元素存储
string word;
vector<string> text;
while(cin>>word){
    text.push_back(word); //把word添加到text后面
}
```

**其他VECTOR操作**

```c++
v.empty()   //如果v不含有任何元素，返回真；否则返回假
v.size()    //返回v中元素的个数
v.push_back(t)  //向v的尾端添加一个值为t的元素
v[n]        //返回v中第n个位置上元素的引用
v1 = v2     //用v2中的元素拷贝替换v1中的元素
v1 = {a,b,c...} //用列表中元素的拷贝替换v1中的元素
v1 == v2    //v1和v2相等当且仅当它们的元素数量相同且对应位置的元素值都相同
v1 != v2    //同上
<,<=,>, >=   //以字典顺序进行比较
```

```c++
//size_type需要指定由哪种类型定义
vector<int>::size_type //正确
vector::size_type      //错误
```

**不能用下标形式添加元素**

```c++
vector对象（以及string对象）的下标运算符，
只能对确知已存在的元素执行下标操作，不能用于添加元素。
```

**使用迭代器**

```c++
vector<int>::iterator iter
auto b = v.begin();返回指向第一个元素的迭代器。
auto e = v.end();返回指向最后一个元素的下一个的迭代器。
如果容器为空，begin()和end()返回的是同一个迭代器，都是尾后迭代器。
使用解引用符*访问迭代器指向的元素。
```

**标准容器迭代器的运算符**

```c++
*iter        返回迭代器iter所指向的元素的引用
iter->mem    等价于(*iter).mem
++iter       令iter指示容器中的下一个元素
--iter       令iter指示容器中的上一个元素
iter1 == iter2 判断两个迭代器是否相等
```

**迭代器类型**

```c++
一般来说无需知道迭代器精确类型
vertor<int>::iterator it; //it能读写元素
string::iterator it2;     //it2能读写元素

vector<int>::const_iterator it3; //it3只能读元素，不能写元素
string::const_iterator it4;      //it4只能读字符，不能写字符

//const_iterator和常量指针差不多，iterator的对象可读可写
//如果vector或string是常量，只能用const_iterator
```

**BEGIN END运算符**

```c++
返回的具体类型由对象是否是常量决定是const_iterator还是iterator
为了便于得到const_iterator返回值，c++11引入cbegin cend
auto it3 = v.cbegin(); //类型是vector<int>::const_iterator
```

test
