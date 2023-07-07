## 2.1基本内置类型
- **算术类型**:整型数(包括字符和布尔型), 浮点数
- **空类型**
  
### 2.1.1算数类型

类型 | 含义| 最小尺寸
---|---|---
bool | 布尔类型 | 未定义
char| 字符 |8
wchar|宽字符|16
char16_t|unicode字符|16
char32_t|unicode字符|32
short|短整型|16
int|整形|16
long|长整型|32
long long|长整型|64
float|单精度浮点数|6位有效数字
double|双精度浮点数|10位有效数字
long double|扩展精度浮点数|10位有效数字
  
  - 一个char大小和机器字长大小相同
  - char16_t 和 char32_t为**unicode**字符集服务(所有自然语言字符)
  - short<=int<=long<=long long
  - float通常32位,double通常64位
    
  #### TIPS
- 无符号类型:如果值非负,类型前面添加unsigned
- 使用int做整数运算,long可能与int大小同,所以大数直接long long
- 算术表达式不要用char,因为char在不同机器上可能有无符号
- 浮点数运算用double,因为float精度不够而且运算代价相差无几
#### 类型转换
- 非布尔值,0转false,其他转true
- 浮点数给整数,只保留小数前的部分
- 整数给浮点数,小数记0,整数位数超越浮点类型容量可能损失
- 赋予无符号类型超出范围,取模
- 赋予带符号类型超出范围,结果未定义(undefined)
- 无符号数和有符号数混搭时候,会变为无符号数  
   
  
#### 数制
10,8,16进制部分数重合,所以需要重新标识,标识最好不要为字母下划线防止识别为变量,前缀0不改变大小,即为8进制,x表示hexdecimal的简写,0X即为十六进制

#### 指定字符类型  

前缀 | 含义 |类型
---|---|---
u | unicode16字符|char16_t
U | unicode32字符|char32_t
L | 宽字符|wchar_t
u8|UTF-8(仅用于字符串字面常量)|char
**后缀**|**整型字面值**|**浮点字面值**
u或U|unsigned
l或L|long|long double
ll或LL|long long
f或F||float


## 2.2变量
类型说明符,后接一个或多个变量名
#### 初始化
    int tmp=0;
    int tmp={0};
    int tmp{0};//花括号,列表初始化
    int tmp(0);  
  
- 列表初始化内置类型如果会导致丢失值会报错
- 内置类型不初始化则值为未定义,使用会出错
- 类不初始化则由类自身决定值
  
#### 定义与声明
- 变量定义：用于为变量分配存储空间，还可为变量指定初始值。程序中，变量有且仅有一个定义。
- 变量声明：用于向程序表明变量的类型和名字,声明可多次。
- 定义也是声明：当定义变量时我们声明了它的类型和名字。
- extern关键字：通过使用extern关键字声明变量名而不定义它。extern目的是为了使用,如果尝试定义extern会出错.


#### 变量命名规范
- 变量小写字母index
- 类名大写字母开头Sales_item
- 多个单词注意划分student_loan




## 2.3复合类型--引用和指针
#### 引用
    int i=1024
    int &refi=i   //refi是指向i的别名
    int &refi2   //错误:引用必须初始化
引用一旦声明就和对象绑定,无法更改
      
    double tmp=3.14;
    int &reftmp=tmp;//错误,必须是double
    
#### 指针
    int *p1=nullptr//可以转换成任意指针,需要先#include cstdlib
    int *p1=0;//与下句等价
    int *p1=null

#### void*指针  
    double obj=3.14,*pd=obj;
    void *pv=&obj;//obj可以是任意对象
    pv=pd//pv可存放任意类型指针
void指针不能直接操作所指对象,因为并不知道所指是什么类型.  
访问未经初始化的指针很危险,所以需要指针指空.

### 指向指针的引用
    int i=42;
    int *p;//p是一个int指针
    int *&r=p;//r是对p的引用
    r=&i;//geir赋值就是令p指向i
    *r=0;//i改为零
    
## 2.4const限定符
    contst int bufsize=512;//正确
    bufsize=500;//错误,const类型无法修改
    const int k;//错误,const类型必须初始化
const默认作用域是文件,如果想要其他文件也使用,用extern
      
    extern const int buffsize=500;//f1定义并初始化
    #include"f1.cpp"//f2包含f1
    extern const int buffsize;//f2声明以使用
  
 #### 常量引用
可以把引用绑定到const对象上,这样以来就无法修改绑定对象
    
    const int i=1024;
    const int &refi=i;
    i=42//错误,是对常量的引用
#### 初始化和对const的引用
    int i=42;
    const int &r1=i;//常量引用的绑定
    const int &r2=42;//常量引用
    const int &r3=r1*2//正确
    int &r4=r1*2//错误,r4非常量引用
    
#### 引用常量引用非常量对象时
    int i=42;
    int &r1=i;//引用r1绑定i
    const int&r2=i;//常量引用r2绑定i
    r1=0;//正确,通过引用修改
    r2=0;//错误,不能通过常量引用修改
    
#### 指针和const
指向常量的必须是指针,且*p不能赋值

    const double pi=3.14;
    double*ptr=&pi//错误,ptr是一个普通指针
    const double *cptr=3.14;//正确
    *cptr=42//错误,常量不能修改
指向非常量的可以是常量指针(此时指针类型和对象类型不同)  
      
    double dval=3.14;
    cptr=&dval
#### const指针
    int num=0;
    int *const pnum=&num//pnum将一直指向num
  
#### 顶层const
- 顶层const(top level const)表示指针本身是常量
- 底层(low level const)表示所指数据类型是常量
- 一般的,顶层const可以表示任意对象是常量  
  

    int i=0;
    int *const p1=&i;//p1不能改,顶层const
    const int ci=42;//ci不能改,顶层const
    const int *p2=&ci//p2可以改,底层const
    const int *const p3=p2//靠右顶层const,靠左底层const
    const int &r=ci;//用于声明的const都是底层const
- 拷贝时底层const必须资格相同(常量不能变为非常量)  
  

    int *p=p3//错误,p3包含底层const,p无
    p2=p3//正确,都是底层const
    p2=&i//正确,int*可以转化为const *int
    int &r=ci//错误,普通int&不能绑定到int常量上
    const int &r2=i//正确
  
#### constexpr表达式(constexpr定义的对象为顶层const)
    constexpr int mf=20//20是常量表达式
    constexpr int limit=mf+1;//mf+1是常量表达式
    constexpr int sz=size()//只有当size是一个constexpr函数才正确
字面值类型:常量表达式的值在编译时进行计算,因此必须限定constexpr用到的类型,称为字面值类型literal type
- 算术类型,引用,指针都属于(指针必须nullptr或0或**固定地址**[不能定义在函数体]对象)
- 自定义类,io库,string类型不属于
    
    
 
## 2.5处理类型
#### 类型别名 
- 传统方法
  

    typedef double wages//wages是double的同义词
    typedef wages base,*p//base是double同义词,p是double*同义词
- 别名声明
  

    using SI=sales_item//SI是sales_item近义词
#### auto类型说明符(自动分析类型)
    auto item=item1+item2;//由item1,item2推断item类型
一次申明多个变量
  
    auto i=0,*p=&i;//正确,整形和整形指针
    auto z=0,pi=3.14;//错误,z,pi类型不一致
  
auto一般会忽略顶层const,同时底层const会保留下来,如果希望推断出的auto类型是一个顶层const,需要明确指出:
  
    const auto f=ci;
还可以将引用的类型设为auto,此时适用原来的初始化规则  

#### decltype类型说明符
**希望从表达式的类型推断类型,但不要用表达式的值**

    decltype(f()) sum=x;
decltype返回该变量的类型(包括顶层const和引用)
  
    const int ci=0,&cj=ci;
    decltype(ci) x=0;//x的类型是const int
    decltype(cj) y=x;//y的类型是const int&,y绑定到变量x
    decltype(cj) z;//错误,引用必须初始化  
decl的表达式如果加上括号,会变成表达式,这样declytype会变成引用类型
  
    decltype((i)) d;错误,引用类型未初始化
    decltype(i) e;正确,是一个未初始化的int

    
## 2.6自定义数据结构
函数体内定义类会使类收到限制,所以类定义在函数体外,同时不同文件中使用一个类,类需要保持一致,所以类通常被定义在头文件,比如库类型string定义在string.h  
为了防止重复的发生,使用#ifndef当且仅当未定义时为真,执行后续操作到#endif为止

    #ifdef SALES_DATA_H
    #define SALES_DATA.H
    struct  Sales_data{...};
    #endif

## 术语表
绑定bind:令某个名字和给定的实体关系到一起  
标识符identifier:组成名字的序列  
字面值:literal不能改变的值,如数字字符字符串,单引号内是字符字面值,双引号是字符串字面值  
void*:可以指向任意非常量的指针类型,不能执行解引用操作
