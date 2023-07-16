# Chapter 10 泛型算法

### 泛型算法

概念：泛型算法即通用算法。 

性质：

* 算法可用于不同类型的容器；
* **算法永远不会改变底层容器的大小**。

输入范围：标准库中大多数算法都对一个范围内的元素进行操作，该范围称为 ”输入范围“。

```c++
auto result = find(vi.cbegin(), vi.cend(), val);
```



***



##### 只读算法

概念：只读算只读取而不改变输入范围内元素。如：find、accumulate、equal 

算法 equal：

```c++
equal(v.cbegin(), v.cend(), v2.cbegin());
```



形参模式：

```c++
alg(beg, end, beg2, other args);
```

* **若算法只接受一个单一迭代器来表示第二个序列，假定第二个序列至少与第一个序列一样长**。

  

##### 写算法

概念：写算法将新值写入序列中的元素。

形参模式：

```c++
alg(beg, end, dest, other args);
```

* dest 参数表示算法写入目的位置迭代器，**该形式的算法假定目标空间足够大，可容纳要写入的数据**。

算法不检查写操作，如：

```c++
vector<int> vi;
fill_n(vi.begin(), 5, 0);  // 严重错误
```



##### 插入迭代器

概念：插入迭代器与一个容器绑定，将容器元素类型的值赋予一个插入迭代器时，迭代器会将该值添加到容器中。

back_inserter：创建一个使用 push_back 的迭代器。

```c++
vector<int> vi;
auto it = back_inserter(vi);
for (int i = 0; i != 5; ++i) {
    *it = i;
}
```

front_inserter：创建一个使用 push_front 的迭代器。

inserter：创建一个使用 insert 的迭代器。

```c++
auto it = inserter(vi, vi.begin()+2);
*it = 5;
```

等价于：

```c++
auto it = vi.insert(vi.begin()+2, 5);
++it;
```



##### 拷贝算法

如：copy、replace、replace_copy

```c++
auto ret = copy(cbegin(a), cend(a), a2);
```



##### 重排元素算法

如：sort、unique

unique：将相邻的重复元素覆盖，从而使不重复元素出现在序列开始部分。



***



### 定制操作

概念：通过自己定义的操作来代替算法的默认运算符。



##### 谓词

概念：谓词返回可以转换为 bool 类型值的函数。

分类：一元谓词、二元谓词。



##### lambda

概念：lambda 表达式是一种可调用对象。定义一个 lambda 时，编译器生成一个与 lambda 对应的新的未命名的类类型。

可调用对象：可以出现在调用运算符左边的对象。

形式：

```c++
[capture list](parameter list) -> return type { function body }
```

* 捕获列表和函数体不可忽略，参数列表和返回类型可以忽略；

* 当返回类型被忽略时，若函数体为单一 return 语句，则返回类型根据返回对象的类型推断；否则默认返回类型为 void。

```c++
transform(vi.cbegin(), vi.cend(), vi.begin(), 
          [](int i) { if (i < 0) return -i; else return i; });  // 错误
```

修改：

```c++
transform(vi.cbegin(), vi.cend(), vi.begin(), 
          [](int i) -> int { if (i < 0) return -i; else return i; });  // 尾置返回类型
```

性质：

lambda 不能有默认参数；

lambda 只能使用已捕获的局部变量；lambda 可以直接使用局部 static 变量以及 lambda 所在函数之外声明的名字。



##### 捕获方式

值捕获：

采用值捕获的前提是变量可以拷贝；

被捕获变量的值是在 lambda 创建时拷贝，而非调用时拷贝：

```c++
unsigned i = 10;
auto fun = [i] { return i; };
i = 0;
auto j = fun();  // j 的值为 10
```

引用捕获：

引用捕获必须确保被引对象在 lambda 执行时仍存在，且具有预期的值。

使用：如捕获 ostream 对象。

建议：尽量让 lambda 捕获简单化；普通变量可采用值捕获，此时只需关注该变量在捕获时的值；

​			应尽量减少捕获的数据量，可能的话应避免捕获指针或引用。

隐式捕获：

在捕获列表中使用 & 或 =。

混合使用，如：

```c++
for_each(vec.cbegin(), vec.cend(), [=, &os](const string &s) { os << s << c; });
```

可变 lambda：

```c++
unsigned i = 10;
auto fun = [i] () mutable { return ++i; };
i = 0;
auto j = fun();  // j 的值为 11
```



***



流迭代器：

istream_iterator：

```c++
istream_iterator<int> in_iter(cin), eof;
vector<int> vi(in_iter, eof);
```

* istream_iterator 允许使用懒惰求值。

ostream_iterator：

```c++
ostream_iterator<int> out_iter(cout, " ");
for (auto e : vi) {
    *out_iter++ = e;  // 等效于 out_iter = e;
}
```

* ostream_iterator 的第二个参数为可选的，该参数必须是一个 C 风格字符串，在输出每个元素后都会打印该字符串。



##### 反向迭代器

概念：反向迭代器就是在容器中反向移动的迭代器。

除了 forward_list 以及流迭代器之外，其他容器都支持反向迭代器。

使用：

```c++
string s("FIRST,MIDDLE,LAST");
auto r_it = find(s.crbegin(), s.crend(), ',');
cout << string(s.crbegin(), r_it) << endl;		// 打印 TSAL
```

修改：

```c++
cout << string(r_it.base(), s.cend()) << endl;	// 打印 LAST
```



***



##### 泛型算法结构

迭代器分类：

​		输入迭代器、输出迭代器、前向迭代器、双向迭代器、随机访问迭代器

迭代器的类别形成一种层次，其中更强大的类别支持更弱类别的所有操作：

| 迭代器层次                                             |
| ------------------------------------------------------ |
| 随机访问迭代器：可读可写，多遍扫描，支持全部迭代器运算 |
| 双向迭代器：可读可写，多遍扫描，可递增可递减           |
| 前向迭代器：可读可写，多遍扫描，只能递增               |
| 输入/输出迭代器：只读不写/只写不读，单遍扫描，只能递增 |

注意：

​		输入迭代器必须支持：==、!=、++、*、->

​		输出迭代器必须支持：++、*

​		前向迭代器支持输入和输出迭代器的所有操作

​		双向迭代器支持前向迭代器的所有操作，且还支持递减，用于在序列中反向移动

​		随机访问迭代器支持全部迭代器操作



***



##### 特定容器算法

list 和 forward_list 定义了特有版本的函数形式算法：sort、merge、remove、reverse 和 unique

* 链表特有的操作会改变底层容器。



![](成电小暑.jpg)
