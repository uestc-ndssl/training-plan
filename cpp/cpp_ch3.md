## 3.1命名空间的using声明
std::in如何不使用作用域操作符::,使用using 声明,可以直接访问命名空间的名字,格式如下  
  
    using namesace::name
    
    using std::cin
    cin>>i;正确
    cout<<i;错误
    std::cout<<i;正确
    
## 3.2标准库类型string
#### 3.2.1定义和初始化string对象  
几种初始化方式:
  
    string s1 默认初始化,s1是一个空串
    string s2=s1 与下列等价
    string s2(s1) s2是s1的副本
    string s3="hi" 与下列等价
    string s3("hi") s3是该字符串字面值的副本,除了最后一个空字符
    string s4(10,'c') s4的内容是cccccccccc
直接初始化与拷贝初始化:  
使用等号=实际是**拷贝初始化**,把等号右侧的初始值拷贝到新创建的对象里面,如果不使用等号执行的是**直接初始化**  
#### 3.2.2 string对象上的操作  
  
    os<<s
    is>>s 
    getline(is,s) is中读取一行给s,返回is
    s.empty() s为空返回true,否则false
    s.size() 返回字符的个数
    s[n] s中第n个字符
    s1+s2 返回s1和s2连接后的结果
    s1=s2 s2的副本替代原s1的字符
字面值和string对象相加  
  
    string s1="hello",s2="world"
    string s3=s1+","+s2 正确
    string s4="hello"+"world" 错误,至少得有一个string
    string s5=s1+","+"world" 正确
    string s6="hello"+","+s2 错误,两个字面值不能相加
s5,s6的区别不同,是因为工作机理类似连续输入连续输出

#### 3.2.3处理string对象中的字符
    isalnum(c) 数字字母为真
    isalpha(c) 字母真
    iscntrl(c) 控制字符为真
    isdigit(c) 数字为真
    isxdigit(c) 十六进制数字为真
    isgraph(c) 不是空格但可打印为真
    isprint(c) c是可打印字符为真(包括空格)
    islower(c) 小写字母为真
    isupper(c) 大写字母为真
    ispunct(c) 标点符号为真
    isspace(c) 空白(空格,制表符,回车,换行)为真
    tolower(c) 转化成小写字母
    toupper(c) 转化成大写字母
处理每个字符?使用基于范围的for语句  
  
    for(declaration:expression)
        statement
    
    string str("something")
    for(auto c : str) 对于str的每个字符
        cout<<c<<endl;
使用for改变字符串的字符
    
    string str("something")
    for(auto &c : str) 
        c=toupper(c) 注意此处是引用,赋值会改变c值
只想处理个别字符?下标(s[0]-s[n-1])或者迭代器
    

## 3.3标准库类型vector
又称容器container,使用vector,必须包含头文件:
    
    #include <vector>
    using std::vector;
vector是模板而非类型,必须通过额外信息指定模板实例化成怎样的类,以vector为例,提供的额外信息是vector内所存放对象的类型:
    
    vector<int> ivec  ivec保存int类型的对象
    vector<Sales_item> Sales_vec  保存Sales_item类型的对象
    vector<vector<string>> file 该向量的元素是vector对象
**vector能容纳绝大部分对象作为元素,但引用不行,因为引用不是对象**
#### 3.3.1定义和初始化vector对象
    
    vector<T> v1 v1是一个空vector,潜在元素是T类型的,执行默认初始化
    vector<T> v2(v1) v2包括v1的所有元素的副本
    vector<T> v2=v1 同上
    vector<T> v3(n,val) 包含n个重复的元素,每个都是val
    vector<T> v4(n) 包含n个执行初始化后的对象
    注意以下是大括号
    vector<T> v5{a,b,c..}
    vector<T> v5={a,b,c..} 同上

#### 3.2.2向vector对象中添加元素
    
    vector<int> v2;
    for (int i=0;i!=100;++i)
     V2.push_pack(i);
#### 3.3.3其他vector操作
    
    v.empty() 不包含任何元素返回z真
    v.size() 返回元素个数
    v.push_back(t) 尾端添加t元素
    v[n] 第n个位置元素的引用
    v1=v2 v2的元素代替v1的元素
    v1={ } 列表的元素代替v1
    v1==v2 当且仅当元素数量相同且对应位置元素相同
    < <= > >= 字典顺序进行比较
访问vector对象中元素的方法类似string.
    
    for(auto &i: v)
        i*=i;
    for(auto i : v)
        cout<<i;
**下标只能用于访问现有元素,不能添加新元素,只能用push_back.另外下标访问不要超越索引,不然会引发缓冲区溢出**  

## 3.4迭代器介绍
#### 3.4.1使用迭代器
string和vector都可以用迭代器.获取迭代器不是用取地址,这些类型拥有**begin**和**end**成员,begin指向第一个元素,end指向尾的下一个元素,如果,容器为空,begin和end返回的是同一个迭代器
    
    auto b=v.begin(),e=v.end() b和e类型相同
    表3.6标准容器迭代器的运算符
    *iter 返回迭代器iter所指元素的引用
    iter->mem 解引用iter并获取该元素名为mem的成员,等价(*iter).mem
    ++iter 令iter指示容器下一个元素 --同理
    i1==i2 指向同一个元素则相等,!+同理
第一个字母改成大写程序(迭代器版本)
    
    string s("somestring")
    if(s.begin()!=s.end())    确保非空
        auto it=s.begin()
        *it=toupper(*it)
将迭代器从一个位置移动到另一位置,用++ --

    for(auto it =s.begin();it!=s.end();++it)
**泛型编程:有些容器没有定义<,但是所有容器定义了 == 和!=,所以养成使用迭代器和== !=可以不用在意容器类型**
  
begin和end运算符
    
    vector<int> v
    const vector<int> cv
    auto it1= v.begin() it1的类型是vector<int>::iterator
    auto it2= cv.begin() it2的类型是vector<int>::const_iterator
    cbegin() cend()会强制返回const_iterator
    
某些对vector对象的操作会使迭代器失效:比如push_back会使该vector对象的迭代器失效  
TIPS:(**凡是使用了迭代器的 循环体都不要添加元素**)

#### 3.4.2 迭代器运算
    
    iter +n 得到的仍是一个迭代器,向前移动了若干元素
    iter +=n iter+n的结果赋给iter
    iter1-iter2 返回两个迭代器之间的距离
    
    auto mid=vi.begin() + vi.size()/2 计算得到最接近vi中间元素的一个迭代器
    if(it<mid) 处理前半部分的元素


## 3.5数组
数组类似vector,但大小固定,不能增删
#### 3.5.1定义和初始化内置数组
    int cnt=42
    int arr[10] 定义十个整数的数组
    int *parr[10] 定义十个整形指针的数组
    string s[cnt] 错误,不是常量表达式
    string st[getsize()] 当getsize是consexpr时正确
    
    int a2[]={0,1,2} {0,1,2}
    int a5[5]={0,1,2} {0,1,2,0,0}
    int a3[2]={0,1,2} 错误,初始值过多
    
    字符数组比较特殊
    char a3[]="c++" 会添加字符串结束的空字符
    char a4[2]="ab" 错误,没有空间容纳空字符
    
    误区
    int a2[]=a 错误,不能用一个数组初始化另一个数组
    a2=a 错误,不能把一个数组赋值给另一数组
    int &ref[10] 错误,不存在引用的数组

#### 3.5.3指针和数组
指针和数组联系很紧密,使用数组时候编译器会将其转化为指针

    string nums[]={"one","two","three"}
    string *p=&nums[0]
    string *p2=nums  等价
    auto p3(num) 等价
decltype返回的是数组整体而不是单个数组元素

    int ia[]={1,2,3}
    decltype(ia) ia2={4,5,6}
#### 标准库类型begin和end  
数组不是类型,所以没有成员函数,此时数组作为参数,end返回最后一个元素下一位置的指针

    int *beg=begin(ia)
    int *last=end(ia)
    
#### c风格字符串
字符串最后一个必须是'\0'空字符,常用string函数如下:

    strlen(p)
    strcmp(p1,p2) 相等返回零
    strcat(p1,p2) p2附加到p1后面,返回p1
    strcpy(p1,p2) p2拷贝给p1,返回p1
    
    char ca[]={'C','+','+'}
    strlen(ca) 错误,ca不是空字符结尾,strlen遇到空字符前会继续找
    
    string s=s1+" "+s2 正确,string对象支持
    ca1+ca2 错误,将两个指针相加毫无意义,c风格字符串不支持
对于大多数应用,使用标准库string比c风格字符串更加高效,安全

####3.5.5与旧代码的接口
    
    string s("hello world") 可以用字符串初始化string
    char *str=s 错误,不能用string对象初始化char*
    const char *str= s.c_str() 正确
    
    数组初始化vector对象
    int  int_arr[]={0,1,2,3,4,5}
    vector<int> abc(begin(int_arr),end(int_arr)) 全部初始化
    vector<int> abc(begin(int_arr)+1,end(int_arr)-1)可以是一部分

## 3.6多维数组

    int i[3][4]={
        {0,1,2,3},
        {4,5,6,7},
        {8,9,10,11}
    }
    int i[3][4]={0,1,2,3,4,5,6,7,8,9,10,11}
    int i[3][4]={
        {0},
        {4},
        {8}
    }初始化每行首元素
    int i[3][4]={0,1,2,3} 初始化第一行
#### 多维数组下标

    int ia[rowcnt][colcnt]
    for(size_t i=0;i!=rowcnt;++i){
        for(size_t j=0;j!=colcnt;++j){
            ia[i][j]=i*colcnt+j;
        }
    }
上述程序可以简化为
    
    size_t cnt=0;
    for(auto &row:ia)
        for(auto &col:row){
            col=cnt;
            cnt++;
        }
#### 指针和多维数组
decltype和auto可以避免加上类型  

    for(auto p=ia;p!=ia+3;++p) {
        for(auto q=*p;q!=*p+4;++q)
    }
    
#### 类型别名简化多维数组的指针

    using int_array=int[4]; 新标准下的别名声明
    typedef int  int_array[4] 等价的typedef声明