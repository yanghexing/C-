# C++读书笔记

### vector容器

```c++
#include <vector>
vector<int> a = {1,2,3,4,5,6};
vector<int> b(a.begin(),a.begin()+3); //将a中的三个元素拷贝到b中
a.push_back(5); //在数组最后添加元素
a.pop_back();	//去掉数组最后一个元素

a.clear();		//清除容器中所有元素，并不改变capacity()的大小
a.empty();		//判断容器是否为空
a.capacity();	//当前容器的容量是多少
a.max_size();	//得到最大的可能的元素的个数，是一个超大的数哈哈哈哈

a.front();		//得到容器头的引用
a.back();		//得到容器尾巴的引用

a.begin();		//得到容器头的指针，类型为iterator
a.end();		//得到容器尾巴的指针

a.erase(a.begin()+pos);		//删除某一位置的元素
a.insert(a.begin(),value);	//在某一位置插入元素

//向量切片
vector<int> b;
b.assign(a.begin(),a.begin()+2);

//遍历容器
for(vector<string>::iterator it = text.begin();it != text.end(); ++it)
	cout << *it << ' ';

//正确删除内存的方式
vector<type> v;
//.... 这里添加许多元素给v
//.... 这里删除v中的许多元素
vector<type>(v).swap(v);
//此时v的容量已经尽可能的符合其当前包含的元素数量
```

### 字符串

​		C++提供了两种字符串的表示：**C 风格的字符串**和**标准 C++引入的string 类**类型。

##### C风格的字符串

```c++
#include <cstring>
//字符串的结束标志：'\0'
int strlen(const char*);  //返回字符串的长度,不包含结束标志
int strcmp(const char*,const char*); //按照字典顺序比较是否相等，区分大小写
int stricmp(const char*,const char*) //不区分大小写，小于返回-1，大于返回1
char* strcpy(char*,const char*);  //把第二个字符串拷贝到第一个字符串中
char* strcat(char*,const char*);  //把第二个字符串粘贴到第一个字符串尾部
char* strstr(const char*, const char*) //在第一个字符串中查找第二个字符串
char* strchr (const char*, int)   //在第一个字符串中查找字符 ASCII:A = a-32
```

遍历C风格的字符串：

```c++
while(*st ++) {...}
```

##### string类型

```c++
string st(length,'o'); //初始化长度为length，全为o的字符串
int len = st.length();  //返回字符串的长度，等同于string.size();
bool res = st.empty();  //如果是空字符串，返回bool类型true
int res = st.compare(st2);  //和st2比较，小于返回-1，等于返回0，大于返回1
int pos = st.find(st2);   //在st中查找st2，返回数值，未找到返回st.npos
string s = st.substr(3,5);  //st的子串，从st[3]开始的五个字符串
string s = st.replace(0,1,str2);  //将st中从0开始长度为1的子串替换为st2
st2 = st1;  //把st1拷贝到st2中
st2 += st1;  //把第二个字符串粘贴到第一个字符
```

string 类型能够自动将C风格的字符串转换成string对象。但是反向的转换不能自动执行。例如：

```c++
const char *pc = "hello world";
string s1 = pc;  //ok
const char *str = s1.c_str(); //注意c_str()返回了一个指向常量数组的指针
```

##### string2int 和 int2string

1. ostringstream ： 用于执行C风格字符串的输出操作。

   istringstream ： 用于执行C风格字符串的输入操作。

   stringstream ： 同时支持C风格字符串的输入输出操作。

2. 采用标准库中`atoi()` 函数和 `to­_string()`函数。

```c++
char a[] = "-100";  
string b = "123";  
int c = atoi(a) + atoi(b.c_str());  

int i = 12;
string s = to_string(i); 
```

### const限定符

1. 不能将一个非const对象指针指向一个const对象

```c++
const int a = 10;	//cont对象在声明时必须进行初始化
int *p = &a;		//error  不能将一个非const对象指针指向一个const对象
*p += 1;		
```

### 引用类型

​		引用(reference)， 通过引用我们可以间接的操作对象，使用方式类似于指针。在实际应用中，引用主要被用于用作函数的形式参数—通常将类对象传递给一个函数。

**1. 引用必须被初始化（和const类似）**

```c++
int ival = 1024;
//ok: refVal是一个指向ival的应用
int &refVal = ival;
//error: 引用必须被初始化
int &refVal2;
//error: refVal是int类型，不是int*
int &refVal = &ival;
//ok: ptrVal是一个指向指针的引用
int *&ptrVal = &ival;
```

 **2. 一旦引用已经定义，它就不能再指向其它的对象（这也是为什么必须要被初始化的原因）。**

```c++
// 定义两个int 类型的对象
int ival = 1024, ival2 = 2048;
// 定义一个引用和一个对象
int &rval = ival, rval2 = ival2
// 定义一个对象一个指针和一个引用
int ival3 = 1024, *pi = &ival3, &ri = ival3;
```

**3. const引用可以用不同类型的对象初始化（只要能从一种类型转换成另一种类型即可）**

```c++
double dval = 1024;
const int &ri = dval;
```

编译器将其变换为：

```c++
int temp = dval;
const int &ri = temp;
```

如果我们给ri赋一个新值，这样做不会改变dval，而是会改变temp。

```c++
const int ival = 1024;
//错误，要求一个const引用
int *&pi_ref = &ival;
//仍然错误
const int *&pi_ref = &ival;
//ok
const int*const &pi_ref = &ival;
```

### 异常处理

​		C++的异常处理机制被称为是不可恢复的。

​		对于合法的throw表达式，抛出的异常往往时class类型的对象（**类的构造函数**），但是throw也可以抛出任何类型的对象（虽然很不常见）。例如，在下面的示例中，函数mathFunc()抛出一个枚举类型的异常对象：

```c++
enum EHstate { noErr, zeroOp, negativeOp, severeError };
int mathFunc( int i ) {
	if ( i == 0 )
		throw zeroOp; // 枚举类型的异常
}
```

#### 异常规范

1. 空的异常规范保证函数不会抛出任何异常。例如函数`no_problem()`保证不会抛出任何异常：

```c++
extern void no_problem() throw();
```

2. 如果一个函数声明没有指定异常规范，则该函数可以抛出任何类型的异常。

3. 在被抛出的异常类型与异常规范中指定的类型之间不允许类型转换。

##### 异常规范与函数指针

```c++
void (*pf) (int) throw(string);
extern void (*pf)( int ) throw(string);
// 错误: 缺少异常规范
void (*pf) ( int );
```

1. 上述声明表示pf是一个函数指针，它只能抛出string类型的异常。和函数声明一样，指针pf的所有声明都必须指定相同的异常规范。
2. 当带有异常规范的函数指针被初始化时，用作初始值的指针异常规范必须与被初始化或赋值的指针异常规范一样或更严格。

```c++
void recoup( int, int ) throw(exceptionType);
void no_problem() throw();
void doit( int, int ) throw(string, exceptionType);

// ok: recoup() 与 pf1 的异常规范一样严格
void (*pf1)( int, int ) throw(exceptionType) = &recoup;

// ok: no_problem() 比 pf2 更严格
void (*pf2)() throw(string) = &no_problem;

// 错误: doit()没有 pf3 严格
void (*pf3)( int, int ) throw(string) = &doit;
```

###  函数模板

​		有时候，强类型语言对于实现相对简单的函数似乎是个障碍。例如：

```c++
int min(int a,int b){
    return a<b?a:b;
}
double min(double a,double b){
    return a<b?a:b;
}
```

​		有一种方法可以替代这种*为每个min() 实例都显示定义一个函数* 的方法，这种方法很有吸引力，但是也很危险，那就是用**预处理器的宏拓展设施**

```c++
#define min(a,b) ((a)<(b)?(a):(b))
```

对于简单的min()调用都能正常工作，但是复杂的调用下，它的行为是不可预期的。这是因为它的机制并不像函数调用那样工作，只是简单的提供函数的替换。例如：

```c++
#define min(a,b) ((a)<(b)?(a):(b))
int main()
{
    int size = 10
    int a[size];
    int count = 0;
    int *p = &a[0];
    
    //计数数组元素的个数
    while(min(p++,&a[size])!= &a[size])
        ++ count;
    cout<<count<<endl;      //输出：5 期望输出：10
    return 0;
}
```

这是因为应用在指针实参p上的后置递增操作随每次扩展而被应用了两次。

**函数模板**

```c++
template <class Type>
Type min(Type a,Type b){
	return a<b?a:b;   // //整数或浮点数皆可使用,若要使用类(class)或结构体(struct)时必须重载大于(>)运算符
}
```

​		关键字template总是放在模板的定义与声明的最前面。关键字后面是用逗号分割的模板参数表(`template parameter list`)，他用尖括号(<>,)括起来。该列表是**模板参数表**，不能为空。模板参数可以是一个**模板类型参数**，它代表了一种类型；也可以是一个**模板非类型参数**，它代表了一个常量表达式。

​		模板类型参数由关键字`class`或`typename`后加一个标识符构成。在函数的模板参数中，这两个关键字的意义相同。

### 内联函数

```c++
int min(int a,int b){
    return a<b?a:b;
}
```

​		将min()写成函数有一个严重的缺点：**调用函数比直接计算条件操作符要慢得多**，因为不但必须拷贝两个实参、保存机器的寄存器程序，还必须转向一个新位置。因此手写条件操作符能快得多。

​		`inline` 内联函数给出了一种解决方案，若一个函数被指定为`inline` 函数则它将在程序中每个调用点上被内联地展开。例如:

```c++
int minVal2 = min( i, j );
//在编译时被展开为
int minVal2 = i < j << i : j;
```

在函数声明或定义中的函数返回类型前加上关键字`inline` 即把`min()`指定成为`inline`：

```c++
inline int min( int v1, int v2 ) { /* ... */ }
```

​		但是注意`inline` 指示对编译器来说只是一个建议，编译器可以选择忽略该建议。因为把一个函数声明为`inline` 函数并不见得真的适合在调用点上展开。一般地`inline` 机制用来优化小的只有几行的经常被调用的函数。

​		当然，对于同一程序的不同文件，如果`inline` 函数出现的话其定义必须相同。对于由两个文件`compute.C` 和`draw.C`构成的程序来说，程序员不能定义这样的`min()`函数，它在`compute.C`中指一件事情，而在`draw.C`中指另外一件事情。如果两个定义不相同，程序将会有未定义的行为。编译器最终会使用这些不同定义中的哪一个作为非`inline` 函数调用的定义是不确定的，因而程序的行为可能并不像你所期望的那样。**为保证不会发生这样的事情建议把`inline` 函数的定义放到头文件中**。

### 函数指针

```c++
//typedefs使声明更易读
typedef int (*PFV) ();  //定义函数类型指针的typedef
PFV testCases[10];
testCases[0] = lexicoCompare;  //ok
testCases[1] = &lexicoCompare;  //ok
```

### 类

#### 类关键字

1. public： 类内，派生类内，类的对象，派生类的对象 -->>均可访问。
2. protected： 类内，派生类内-->>可以访问；类的对象、派生类的对象 -->>不可访问。
3. private：类内 -->> 可以访问；类的对象，派生类内，派生类对象 -->>不可以访问。

#### 友元

​		**友元声明的主要用处是用在重载操作符上。**

​		友元函数是**可以直接访问类的私有成员的非成员函数**，它是定义再类外的普通函数，它不属于任何类，但需要在类中加以声明，声明时只需在友元的名称前加上关键字friend，**它们不受其在类体中被声明的public、private、protected区的影响**。*在某些情况下，特别时在对某些成员函数多次调用时，由于参数传递，类型检查和安全检查等都需要时间开销，而影响程序运行效率。友元的作用在于提高运行效率，但是它破坏了类的封装性和隐藏性。***友元也可以是一个类，友元类的所有成员都被给予访问类的私有成员的权力。**

示例：

```c++
class Screen {
	friend istream& operator>> ( istream&, Screen& );
	friend ostream& operator<< ( ostream&, const Screen& );
public:
	// ... Screen 类的其他部分
};
```

1. 如果我们决定一个函数必须被声明为两个类的友元，则友元声明如下：

```c++
class Window;//只声明
class Screen{
    friend bool is_equal(Screen&, Window&);
}
class Window{
    friend bool is_equal(Screen&, Window&);
}
```

2. 如果我们决定该函数必须作为一个类的成员函数，并又是另一个类的友元，则声明如下：

```c++
class Window;
class Screen {
public:
	// copy 是类 Screen 的成员
	Screen& copy( Window & );
};
class Window {
	// copy 是类 Window 的一个友元
	friend Screen& Screen::copy( Window & );
}
```

#### 类对象

​		类的定义，如类Screen，不会引起存储区分配。只有当定义一个类的对象时，系统才会分配存储区。

​		我们必须用成员访问操作符来访问类对象的数据成员或成员函数，**点成员访问操作符（.)与类对象或引用连用；箭头访问操作符（->）与类对象的指针连用。**

```c++
Screen *s1;
s1->height();
(*s1).height();
```

#### 类成员

1. **成员变量在使用初始化列表时初始化时，与构造函数中初始化列表的顺序无关，只与成员变量的声明顺序有关。**
2. **如果不适用初始化列表初始化，在构造函数内初始化时，此时与成员变量在构造函数中的位置有关。**
3. **类中的const成员变量通常必须在构造函数初始化列表中初始化(类似的还有：引用)。**
4. **类中static成员变量，必须在类外初始化！（在C++11中，只有static型变量不能在声明时初始化）**
5. 变量的初始化顺序是：
   1. 基类的静态变量。
   2. 派生类的静态变量
   3. 基类的成员变量
   4. 派生类的成员变量

##### 实例

1. 常量数组的初始化：

```c++
//解法1 转换成static数组，在cpp文件中初始化
class A{
public:
    A(){}
private:
    static const int val[5];
}

const int A::val[5] = {1,2,3,4,5};//在实现文件中

//解法2 指针
class A{
public:
    A(const int v[]):val(v){}
private:
    const int* const val;
}
```

2. C++中一般是不能使用变量作为一个数组长度的，必须使用常量。这是因为数组作为c++的内置数据类型，其空间分配在**栈内存**中，这部分空间在**编译时就要确定**，不能等到运行时再分配。但是仍有办法使用变量作为数组长度：**绕过栈内存，将数组空间开辟在堆空间。**

```c++
int* val = new int[n];
```

#### 类成员函数

​		在类定义中定义的成员函数被自动作为**内联函数**处理。通常一两行以上的成员函数最好被定义在类体之外，在类体外定义的成员函数不是inline的。但是这样的函数可以显式的在类体中的函数声明上使用inline，或是在类体外出现的函数定义上显式使用关键字inline。

​		**由于内联函数必须在调用它的每个文本文件中被定义，所以没有在类体中定义的内联成员函数必须被放在类定义出现的头文件中，且跟在类定义的后面。**

###### 构造函数和析构函数

​		**缺省构造函数通常是必须的。**因为容器类（比如vector）要求它们的类元素或者提供缺省的构造函数，或者不提供构造函数。类似地，对于类对象的动态数组，在分配内存的时候也要求或者有缺省构造函数或者没有构造函数。例如：

```c++
//要求Account类提供缺省构造函数，很遗憾我们没有办法提供一组显式的值用来初始化
Account *pact = new Account[value];
delete [] pact;
```

###### const和volatile成员函数

​		通常，程序不直接修改类对象，为尊重类对象的常量性，编译器必须区分**安全**与**不安全**成员函数（即区分试图修改类对象和不试图修改类对象的函数）。通常把成员函数声明为const，以表明他们**不修改类对象**。例如：

```c++
class Screen{
public:
    char get() const {return _screen[_cursor];}
    bool isEqual(char ch) const;
}

bool Screen::isEqual(char ch) const{...}
```

**1. 只有被声明为const的成员函数才能被一个const类对象调用(构造函数和析构函数除外)。**关键字const被放在成员函数的参数表和函数体之间。

**2. 把一个成员函数声明为const可以保证这个成员函数不修改类的数据成员，但是，如果该类含有指针，那么在const成员函数中就能修改指针所指对象。**

```c++
class Text {
public:
	void bad( const string &parm ) const;
private:
	char *_text;
};
void Text::bad( const string &parm ) const
{
	_text = parm.c_str(); 		// 错误: 不能修改 _text
	for ( int ix = 0; ix < parm.size(); ++ix )
		_text[ix] = parm[ix]; 	// 不好的风格, 但不是错误的
}
```

**3. const成员函数可以被相同参数表的非const成员函数重载。**在这种情况下根据类对象的常量性决定调用哪个函数：

```c++
class Screen{
public:
	char get(int x, int y);
	char get(int x, int y) const;
}

int main() {
	const Screen cs;
	Screen s;
	char ch = cs.get(0,0); // 调用 const 成员
	ch = s.get(0,0); // 调用非 const 成员
}
```

​		同样的也可将成员函数声明为volatile。**如果一个类对象的值可能被修改的方式是编译器无法控制或检测的**（例如，I/O端口的数据结构），则把它声明为volatile。与const类似，对于一个volatile类对象，只有volatile成员函数、构造、析构函数可以被调用。

###### mutable数据成员

​		为了允许修改一个类的数据成员，即使它是一个const 对象的数据成员，我们也可以把该数据成员声明为mutable （易变的）。mutable 数据成员永远不会是const 成员，即使它是一个const 对象的数据成员，**mutable 成员总可以被更新**，即使是在一个const 成员函数中。

```c++
class Screen {
public:
// 成员函数
private:
	string _screen;
	mutable string::size_type _cursor; // mutable 成员
}
```

#### 静态类成员

*提供一个所有对象共同使用的全局对象*

​		静态数据成员被当作该类类型的全局对象。对于非静态数据成员，每个类对象都有自己的拷贝，而静态数据成员对每个类类型只有一个拷贝。静态数据成员只有一份，由该类类型的所有对象共享访问。同全局对象相比使用静态数据成员有两个优势：

1. **静态数据成员没有进入程序的全局名字空间，因此不存在与程序中其他全局名字冲突的可能性。**
2. **可以实现信息隐藏，静态成员可以是private 成员，而全局对象不能。**

```c++
class Account {
// ...
private:
    static double _interestRate;
	static const int nameSize = 16;     //有序类型，ok
	static const char name[nameSize];    //数组，无序类型，必须在类外定义
};

// 文本文件
const int Account::nameSize; // 必需的成员定义，C++11中好像不是必须的
//namesize没有被Account限定修饰
const char Account::name[nameSize] = "Savings Account";
```

​		**在类体内初始化一个const 静态数据成员时，该成员必须仍然要被定义在类定义之外（C++11中并不一定需要类外的成员定义）！！！**但是因为这个静态数据成员的初始值是在类体中指定的，所以在类定义之外的定义不能指定初始值。

##### 静态成员函数

​		静态成员函数**只能访问静态变量，任何试图访问非静态数据成员都将引起报错。出现在类体外的函数定义不能指定关键字static！这一点和const成员函数有很大的不同！！！**

**1. 静态成员函数没有this指针！！！**

**2. 静态成员的初始值可以直接引用类成员，而无需使用点、箭头或域解析操作符。**

```c++
class Account {
// ...
private:
	static double _interestRate;
	static double initInterest();
};
// 引用 Account::initInterest()
double Account::_interestRate = initInterest();
```

#### 指向类成员的指针

成员函数指针和普通函数指针**不匹配。**

```c++
//所有指向类成员的指针都可以用0赋值
int (Screen::*pmf1) () = 0;
int (Screen::*pmf2) () = &Screen::height;
//使用typedef可以使成员指针语法更易读
typedef Screen& (Screen::*Action)();
Action default = &Screen::home;
```

使用指向类成员的指针：**要求有括号！！！**

```c++
Screen myScreen, *bufScreen;
//直接调用成员函数
if(myScreen.height() == bufScreen->height())
    ...
//通过成员指针的等价调用
if((myScreen.*pmf2)() == (bufScreen->*pmf2)())
    ...
```

示例：

```c++
Action Menu[] = {
	&Screen::home,
	&Screen::forward,
	&Screen::back,
	&Screen::up,
	&Screen::down,
	&Screen::end
};
enum CursorMovements {
	HOME, FORWARD, BACK, UP, DOWN, END
};

class Screen{
public:
    Screen & repeat(Action = &Screen::forward,int = 1);
    Screen & move(CursorMovements cm);
    //...
}

Screen& Screen::move( CursorMovements cm )
{
	(this->*Menu[cm])();
	return *this;
}

Screen& Screen::repeat( Action op, int time s )
{
	for ( int i = 0; i < times; ++i )
	(this->*op)();
	return *this;
}
```

##### 静态类成员的指针

​		在非静态类成员指针和静态类成员指针之间有一个区别，**指向类成员的指针语法不能用来引用类的静态成员！**静态类成员是属于该类的全局对象和函数，他们的指针是普通指针。（**静态成员函数没有this指针**）

例如：

```c++
class Account {
public:
	static double interest() { return _interestRate; }
private:
	static double _interestRate;
};

double (*pf)() = &Account::interest;
```

#### 联合 union：一种节省空间的类

​		union 不能有静态数据成员或是引用成员。如果一个类类型定义了构造函数、析构函数或拷贝赋值操作符，则它不能成为union 的成员类。例如：

```c++
union illegal_members {
	Screen s; // 错误: 有构造函数
	Screen *ps; // ok
	static int is; // 错误: 静态成员
	int &rfi; // 错误: 引用成员
};
```

​		我们可以为union定义成员函数，包括构造函数和析构函数。例如：

```c++
union TokenValue {
public:
	TokenValue(int ix) : _ival(ix) { }
	TokenValue(char ch) : _cval(ch) { }
	// ...
	int ival(){return _ival};
	char cval(){return _cval};
private:
	int _ival;
	char _cval;
};
//如何使用union TokenValue
enum TokenKind { ID, Constant /* 及其他语法单元 */ };
class Token {
public:
	TokenKind tok;   //一个额外对象，跟踪当前被存储在union中值的类型
	TokenValue val;
};
```

#### 类域

1. inline成员函数体内名字解析，对于类体中内联定义的成员函数，只考虑在外围类定义之前可见的全局声明。

```c++
int _height;
class Screen{
public:
    Screen(int _height){
        _height = 0;   		//构造函数局部域内
        this->_height = 0;	//类域内的_height
        Screen::_height = 0;
        ::_height = 0;		//全局_height
    }
private:
    int _height;
}
```

2. 嵌套类不能直接访问其外围类的非静态成员，即使这些成员是共有的，任何对外围类的非静态成员的访问都要求通过外围类的指针，引用或对象来完成。尽管访问外围类的非静态数据成员需要通过对象指针或引用才能完成，但是**嵌套类可以直接访问**外围类的**静态成员、类型名、枚举值**（假定这些成员是公有的）。例如：

```c++
class List {
public:
    typedef int (*pFunc)();
	enum ListStatus { Good, Empty, Corrupted };
	int init( int );
private:
	class ListItem {
	public:
		ListItem( int val = 0 );
		void mf( const List & );
        ListStatus status; // ok
		pFunc action; // ok
		int value;
	};
};

List::ListItem::ListItem( int val )
{
	// List::init() 是类 List 的非静态成员
	// 必须通过 List 类型的对象或指针来使用
	value = init( val ); // 错误: 非法使用 init
}

void List::ListItem::mf( const List &il ) {
	memb = il.init(); // ok: 通过引用调用 init()
}
```

### 抽象类

#### 纯虚函数

​		什么是纯虚函数？（含有纯虚函数的类，我们称为**抽象类**，或**纯虚类**）

​		**1. 将成员函数声明为virtual**

​		**2. 后面加上 = 0**

​		**3. 该函数没有函数体**

​		**4. 抽象类不能被实例化！（接口Interface，用于替代C中的回调函数）**

```c++
class CmdHandler{
public:
    virtual void OnCommand(char* cmdline) = 0;
}
```



### 重载操作符

- C++要求，赋值（=）、下标（[]）、调用（（））和成员访问箭头（->）操作符必须被定义为类成员操作符。任何把这些操作符定义为名字空间成员的定义都会被标记为编译时刻错误。

1. **C++有些重载运算符为什么要返回引用?**

```c++
class TestClass {
    private:
        int number;
    public:
        TestClass& operator+=(const TestClass& rhs) {
            number += rhs.number;
            return *this;
        }
};
```

其中+=运算符，它本身的意义是[自增，并返回自增后的值]，所以就要返回自己，而不是返回一个自己的拷贝。

2. **返回的引用赋给一个变量后，那个变量不是引用，该如何理解？**

```c++
class StupidClass {
    int flag;
    public:
        StupidClass(int flag): flag(flag) {
            cout << "Constructor " << flag << endl;
        }
        ~StupidClass() {
            cout << "Destructor " << flag << endl;
        }
        StupidClass(const StupidClass& rhs) {
            cout << "Copy Constructor *this=" << flag << " rhs=" << rhs.flag << endl;
        }
        StupidClass& operator=(const StupidClass& rhs) {
            cout << "Operator = *this=" << flag << " rhs=" << rhs.flag << endl;
            return *this;
        }
        StupidClass& operator+=(const StupidClass& rhs) {
            cout << "Operator += *this=" << flag << " rhs=" << rhs.flag << endl;
            flag += rhs.flag;
            return *this;
        }
};

int main() {
    StupidClass var1(1), var2(2);
    StupidClass var3 = var1 += var2;
    return 0;
}
```

输出结果是：

```c++
Constructor 1
Constructor 2
Operator += *this=1 rhs=2
Copy Constructor *this=3 rhs=3
Destructor 3
Destructor 2
Destructor 3
```

3. **重载输入和输出流<<和>>运算符**

```c++
ostream& operator<<(ostream& os, const TestClass& rhs) {
    os << rhs.number;
    return os;
}
```
