# Ch9 函数

函数简单来说就是一连串语句，这些语句被组合在一起，并被指定了一个名字。在C语言中，函数不一定要有参数，也不一定要计算数值。

函数是C程序的构建块。每个函数本质上是一个自带声明和语句的小程序。可以利用函数吧程序划分成小块，这样便于人们理解和修改程序。由于不必重复编写要多次使用的代码，函数可以使编程不那么单调乏味。此外，函数可以复用：一个函数最初可能是某个函数的一部分，但可以将其用于其他程序中。



本章内容

将学习如何编写除`main`函数以外的其他函数，并更加深入地了解`main`函数本身。

定义和调用函数的方法

函数的声明，以及它和函数定义的差异

参数如何传递给函数

`return`语句

与程序终止相关的问题

递归

## 9.1 函数的定义和调用

自定义“求平均值”函数

```c
double average(double a, double b)
{
  return (a+b) / 2;
}
```

在函数开始处放置的单词`double`表示`average`函数的***返回类型***（return type），也就是每次调用该函数时返回数据的类型。标识符a和标识符b（即函数的***形式参数***（parameter））表示在调用`average`函数时需要提供的两个数。每一个形式参数都必须有类型（正像每个变量有类型一样）。函数的形式参数本质上是变量，其初始值在调用函数的时候才提供。

每个函数都有一个用花括号括起来的执行部分，称为***函数体***（body）。`average`函数的函数体由一条`return`语句构成。执行这条语句将会使函数“返回”到调用它的地方，表达式`(a+b)/2`的值将作为函数的返回值。

为了调用函数，需要写出函数名及跟随其后的***实际参数***（argument）列表。例如，`average(x,y)`是对`average`函数的调用。实际参数不一定要是变量，任何正确类型的表达式都可以。

```c
/* average.c */
/* Computer pairwise averages of three numbers */

#include <stdio.h>

double average(double a, double b)
{
  return (a + 2) / 2;
}

int main(void)
{
  double x, y, z;
  
  printf("Enter three numbers: ");
  scanf("%lf%lf%lf", &x, &y, &z);
  printf("Average of %g and %g: %g\n", x, y, average(x,y));
  printf("Average of %g and %g: %g\n", y, z, average(y,z));
  printf("Average of %g and %g: %g\n", x, z, average(x,z));
  
  return 0;
}
```

这里把`average`函数的定义放在了`main`函数的前面。

将函数的定义放在`main`函数的后面可能会有问题。

### 9.1.1 函数定义

函数定义(function definition)的一般格式

```c
return-type function-name （ parameters ）
{
	declarations
	statements
}
```

函数的“返回类型”是函数返回值的类型。下列规则用来管理返回类型/

- 函数不能返回数组，但关于返回类型没有其他限制。
- 指定返回类型是`void`类型说明函数没有返回值。
- 如果省略返回类型，C89会假定函数返回值的类型是`int`类型，（C99）但在C99中这是不合法的。

一些程序员习惯把放在函数名的上边：

```c
double average(double a, double b)
{
  return (a + b) / 2;
}
```

如果返回类型很冗长，比如`unsigned long int`类型，那么把返回类型单独放在一行是非常有用的。

函数名后边有一串形式参数列表。需要在每个形式参数的前面说明其类型，形式参数间用逗号进行分隔。如果函数没有形式参数，那么在圆括号内应该出现`void`。注意：即使几个形式参数具有相同的数据类型，也必须对每个形式参数分别进行类型说明。

```c
double average(double a, b)		/*** WRONG ***/
{
  return (a + b) / 2;
}
```

函数体可以包含声明和语句。例如，`average`函数可以写成

```c
double average(double a, double b)
{
  double sum;			/* declaration */
  
  sum = a + b;		/* statement */
  return sum / 2;	/* statement */
}
```

函数体内声明的变量专属于此函数，其他函数不能对这些变量进行检查或修改。在C89中，变量声明必须出现在语句之前。在C99中，变量声明和语句可以混在一起，只要变量在第一次使用之前进行声明就行。

对于返回类型为`void`的函数，其函数体可以为空：

```c
void print_pun(void)
{
}
```

### 9.1.2 函数调用

函数调用由函数名和跟随其后的实际参数列表组成，其中实际参数列表用圆括号括起来：

```c
average(x, y);
print_count(i);
print_pun();
```

`void`函数调用的后边始终跟着分号，使得该调用成为语句：

```c
print_count(i);
print_pun();
```

另一方面，非`void`函数调用会产生一个值，该值可以存储在变量中，还可以进行测试、显示或者用于其他用途：

```c
avg = average(x, y);
if (average(x, y) > 0)
  printf("Average is positive\n");
printf("The average is %g\n", average(x, y));
```

## 9.2 函数声明

假设重新编排程序`average.c`，使`average`函数的定义放置在`main`函数的定义之后：

```c
#include <stdio.h>

int main(void)
{
  double x, y, z;
  
  printf("Enter three numbers: ");
  scanf("%lf%lf%lf", &x, &y, &z);
  printf("Average of %g and %g: %g\n", x, y, average(x, y));
  printf("Average of %g and %g: %g\n", y, z, average(y, z));
  printf("Average of %g and %g: %g\n", x, z, average(x, z));
  
  return 0;
}

double average (double a, double b)
{
  return (a + b) / 2;
}
```

当遇到`main`函数中第一个`average`函数调用时，编译器没有任何关于`average`函数的信息：编译器不知道`average`函数有多少形式参数，形式参数的类型是什么，也不知道`average`函数的返回值是什么类型。但是，编译器不会给出出错消息，而是假设`average`函数返回`int`型的值。我们可以说编译器为该函数创建了一个***隐式声明***（implicit declaration）。编译器无法检查传递给`average`的实参个数和实参类型，只能进行默认的实际参数提升并期待最好的情况发生。当编译器在后面遇到`average`的定义时，它会发现函数的返回类型实际上是`double`而不是`int`，从而我们得到一条出错的消息。

为了避免定义前调用的问题，一种方法是使每个函数的定义都出现在其调用之前。可惜的是，有时候无法进行这样的安排；而且即使可以这样安排，程序也会因为函数定义的顺序不自然而难以阅读。

在调用前声明每个函数。***函数声明***（function declaration）使得编译器可以先对函数进行概要浏览，而函数的完整定义以后再给出。函数声明类似于函数定义的第一行，不同之处是在其结尾处有分号：

```c
return-type function-name ( parameters );
```

下面是为`average` 函数添加了声明后程序的样子：

```c
#include <stdio.h>

double average(double a, double b);		/* DECLARATION */

int main(void)
{
  double x, y, z;
  
  printf("Enter three numbers: ");
  scanf("%lf%lf%lf", &x, &y, &z);
  printf("Average of %g and %g: %g\n", x, y, average(x, y));
  printf("Average of %g and %g: %g\n", y, z, average(y, z));
  printf("Average of %g and %g: %g\n", x, z, average(x, z));
  
  return 0;
}

double average(double a, double b)		/* DEINITION */
{
  return (a + b) / 2;
}
```

为了与过去的那种圆括号内为空的函数声明风格相区别，我们把正在讨论的这类函数声明称为***函数原型***（function prototype）。原型为如何调用函数提供了完整的描述：提供了多少实际参数，这些参数应该不是什么类型，以及返回的结果是什么类型。

函数原型不需要说明函数形式参数的名字，只要显示它们的类型就可以了：

```c
double average(double, double);
```

（C99）C99遵循这样的规则：在调用一个函数之前，必须先对其进行声明或定义。调用函数时，如果此前编译器未见到该函数的声明或定义，会导致出错。

## 9.3 实际参数

形式参数（parameter）出现在函数定义中，它们以假名字来表示函数调用时需要提供的值；实际参数（argument）是出现在函数调用中的表达式。在形式参数和实际参数的差异不是很重要的时候，有时会用参数表示两者中的任意一个。

C语言中，实际参数是通过值传递的：调用函数时，计算出每个实际参数的值并且把它赋值给相应的形式参数。在函数执行过程中，对形式参数的改变不会影响实际参数的值，这是因为形式参数中包含的是实际参数值的副本。从效果上来说，每个形式参数的行为好像是把变量初始化成与之匹配的实际参数的值。

实际参数按值传递既有利也有弊。因为形式参数的修改不会影响到相应的实际参数，所以可以把形式参数作为函数内的变量来使用，这样可以见啥真正需要的变量的数量。

### 9.3.1 实际参数的转换

C语言允许在实际参数的类型与形式参数的类型不匹配的情况下进行函数调用。管理如何转换实际参数的规则与编译器是否在调用前遇到函数的原型（或者函数的完整定义）有关。

- 编译器在调用前遇到原型。就像使用赋值一样，每个实际参数的值被隐式地转换成相应形式参数的类型。例如，如果把`int` 类型的实际参数传递给期待得到`double`类型数据的函数，那么实际参数会被自动转换成`double`类型。
- 编译器在调用前没有遇到原型。编译器执行默认的实际参数提升：（1）把`float`类型的实际参数转换成`double`类型，（2）执行整值提升，即把`char`类型和`short`类型的实际参数转换成`int`类型。（C99实现了整数提升。）

### 9.3.2 数组型实际参数

数组经常被用作实际参数。当形式参数是一维数组时，可以（而且是通常情况下）不说明数组的长度：

```c
int f(int a[])			/* no length specified */
{
  ...
}
```

实际参数可以是元素类型正确的任何一维数组。只有一个问题：`f`函数如何知道数组是多长呢？可惜的是，C语言没有为函数提供任何简便的方法来确定传递给它的数组的长度；如果函数需要，我们必须把长度作为额外的参数提供出来。

下面的函数说明了一维数组型实际参数的用法。当给出具有`int` 类型值的数组`a`时，`sum_array`函数返回数组`a`中元素的和。因为`sum_array`函数需要知道数组`a`的长度，所以必须把长度作为第二个参数提供出来。

```c
int sum_array(int a[], int n)
{
  int i, sum = 0;
  
  for (i = 0; i < n; i++)
    sum += a[i];
  
  return sum;
}
```

在把数组名传递给函数时，不要在数组名的后边放置方括号；

```c
total = sum_array(b[], LEN);		/* WRONG */
```

一个关于数组型实际参数的重要论点：函数无法检测传入的数组长度的正确性。我们可以利用这一点来告诉函数，数组的长度比实际情况小。假设，虽然数组`b`有100个元素，但是实际仅存储了50个数。通过书写下列语句可以对数组的前50个元素进行求和：

```c
total = sum_array(b, 50);			/* sums first 50 elements */
```

`sum_array`函数将忽略另外50个元素。（事实上，`sum_array`函数甚至不知道另外50个元素的存在！）

关于数组型实际参数的另一个重要论点是：函数可以改变数组型形式参数的元素，且改变会在相应的实际参数中体现出来。例如，下面的函数通过在每个数组元素中存储0来修改数组：

```c
void store_zeros(int a[], int n)
{
  int i;
  
  for (i = 0; i < n; i++)
    a[i] = 0;
}
```

如果形式参数是多维数组，声明参数时只能省略第一维的长度。例如，如果修改`sum_array`函数使得`a`是一个二维数组，我们可以不指出行的数量，但是必须指定列的数量：

```c
#define LEN 10

int sum_two_dimensional_array(int a[][LEN], int n)
{
  int i, j, sum = 0;
  
  for (i = 0; i < n; i++)
    for (j = 0; j < LEN; j++)
      sum += a[i][j];
  
  return sum;
}
```

### 9.3.3 变长数组形式参数（C99）

C99增加了几个与数组型参数相关的特性。第一个是变长数组，这一特性允许我们用非常量表达式指定数组的长度。变长数组也可以作为参数。

如果使用变长数组形式参数，我们可以明确说明数组`a`的长度就是n：

```c
int sum_array(int n, int a[n])
{
  ...
}
```

第一个参数`n`的值确定了第二个参数`a`的长度。注意，这里交换了形式参数的顺序，使用变长数组形式参数时的顺序很重要。

对于新版本的`sum_array`函数，其函数原型有好几种写法。一种写法是使其看起来跟函数定义一样：

```c
int sum_array(int n, int a[n]);			/* Version 1 */
```

另一种写法是用`*`（星号）取代数组长度：

```c
int sum_array(int n, int a[*]);			/* Version 2a */
```

使用`*`的理由是：函数声明时，形式参数的名字是可选的。如果第一个参数定义被省略了，那么就没有办法说明数组`a` 的长度是n，而星号的使用则为我们提供了一个线索——数组的长度与形式参数列表中前面的参数有关：

```c
int sum_array(int, int[*]);					/* Version 2b */
```

另外，方括号中为空也是合法的。在声明数组参数中我们经常这么做：

```c
int sum_array(int n, int a[]);			/* Version 3a */
int sum_array(int, int []);					/* Version 3b */
```

但是让括号为空不是一个很好的选择，因为这样并没有说明`n`和`a`之间的关系。

一般来说，变长数组形式参数的长度可以是任意表达式。例如，假设我们要编写一个函数来连接两个数组`a`和`b`，要求先复制`a`的元素，再复制`b`的元素，把结果写入第三个数组`c`：

```c
int concatenate(int m, int n, int a[m], int b[n], int c[m+n])
{
  ...
}
```

数组`c`的长度是`a`和`b`的长度之和。这里用于指定数组`c`长度的表达式只用到了另外两个参数；但一般来说，该表达式可以使用函数外部的变量，甚至可以调用其他函数。

一维变长数组形式参数通过指定数组参数的长度使得函数的声明和定义更具描述性。但是，由于没有进行额外的错误检测，数组参数依然有可能太长或太短。

如果变长数组参数是多维的则更加实用。原始的函数要求数组的列数固定。如果使用变长数组形式参数，则可以推广到任意列数的情况：

```c
int sum_two_dimensional_array(int n, int m, int a[n][m])
{
  int i, j, sum = 0;
  
  for (i = 0; i < n; i++)
    for (j = 0; j < m; j++)
      sum += a[i][j];
  
  return sum;
}
```

这个函数的原型可以是以下几种：

```c
int sum_two_dimensional_array(int n, int m, int a[n][m]);
int sum_two_dimensional_array(int n, int m, int a[*][*]);
int sum_two_dimensional_array(int n, int m, int a[][m]);
int sum_two_dimensional_array(int n, int m, int a[][*]);
```



### 9.3.4 在数组参数声明中使用`static`（C99）

C99允许在数组参数声明中使用关键字`static`。

在下面这个例子中，将`static`放在数字3之前表明数组`a`的长度至少可以保证是3:

```c
int sum_array(int a[static 3], int n)
{
  ...
}
```

这样使用`static`不会对程序的行为有任何影响。`static`的存在只不过是一个“提示”，C编译器可以据此生成更快的指令来访问数组。（如果编译器知道数组总是具有某个最小值，那么它可以在函数调用时预先从内存中取出这些元素值，而不是在遇到函数内部实际需要用这些元素的语句时才取出相应的值。）

最后，关于`static`还有一点值得注意：如果数组参数是多维的，`static`仅可用于第一维（例如，指定二维数组的行数）。

### 9.3.5 复合字面量（C99）

当调用`sum_array`函数时，第一个参数通常是（用于求和的）数组的名字。例如，可以这样调用`sum_array`：

```c
int b[] = {3, 0, 3, 4, 1};
total = sum_array(b, 5);
```

这样写的唯一问题是需要把`b`作为一个变量声明，并在调用前进行初始化。如果`b`不作他用，这样做其实有点浪费。

在C99中，可以使用复合字面量（compound literal）来避免该问题，复合字面量是通过指定其包含的元素而创建的没有名字的数组。下面调用`sum_array`函数，第一个参数就是一个复合字面量：

```c
total = sum_array((int []){3, 0, 3, 4, 1}, 5);
```

一般来说，复合字面量的格式为：现在一对圆括号内给定类型名，随后在一对花括号内设定所包括元素的值。复合字面量类似于应用于初始化式的强制转换。事实上，复合字面量和初始化式遵守同样的规则。复合字面量可以包含指示符，就像指定初始化式一样；可以不提供完全的初始化（未初始化的元素默认被初始化为零）。例如，复合字面量`(int[10]){8, 6}`有10个元素，前两个元素的值为8和6，剩下的元素值为0。

函数内部创建的复合字面量可以包含任意的表达式，不限于常量。例如：

```c
total = sum_array((int []){2 * i, i + j, j * k}, 3);
```

其中`i`、`j`、`k`都是变量。复合字面量的这一特性极大地增加了其实用性。

复合字面量为左值，所以其元素的值可以改变。如果要求其值为“只读”，可以在类型前加上`const`，如`(const int []){5, 4}`。

## 9.4 `return`语句

非`void`的函数必须使用`return`语句来指定将要返回的值。`return`语句有如下格式：

```c
return expression ;
```

表达式经常只是常量或变量：

```c
return 0;
return status;
```

但也可能是更加复杂的表达式。例如，在`return`语句的表达式中看到条件运算符是很平常的：

```c
return n >= 0 ? n : 0;
```

执行这条语句时，表达式`n >= 0 ? n : 0`先被求值。如果`n`不是负值，这条语句返回`n`的值，否则返回0.

如果`return`语句中表达式的类型和函数的返回类型不匹配，那么系统将会把表达式的类型隐式转换成返回类型。例如，如果声明函数返回`int`类型值，但是`return`语句包含`double`类型表达式，那么系统将会把表达式的值转换成`int`类型。

如果没有给出表达式，`return`语句可以出现在返回类型为`void` 的函数中：

```c
return;		/* return in a void function */
```

如果把表达式放置在上述这种`return`语句中将会获得一个编译时错误。下面的例子中，在给出负的实际参数时，`return`语句会导致函数立刻返回：

```c
void print_int(int i)
{
  if (i < 0)
    return;
  printf("%d", i);
}
```

如果`i`小于0，`print_int`将直接返回，而不会调用`printf`。

`return`语句可以出现在`void`函数的末尾：

```c
void print_pun(void)
{
  printf("To C, or not to C: that is the question.\n");
  return;		/* OK, but not needed */
}
```

但是，`return`语句不是必需的，因为在执行完最后一条语句后函数将自动返回。

如果非`void`函数到达了函数体的末尾（也就是说没有执行`return`语句），那么如果程序试图使用函数的返回值，其行为是未定义的。有些编译器会在发现非`void`可能到达函数体末尾时产生诸如"control reaches and of non-void function"这样的警告消息。

## 9.5 程序终止

既然`main`是函数，那么它必须有返回类型。正常情况下，`main`函数的返回类型是`int`类型，因此我们目前见到的`main`函数都是这样定义的：

```c
int main(void)
{
  ...
}
```

以往的C程序常常省略`main`的返回类型，这其实是利用了返回类型默认为`int`类型的传统：

```c
main()
{
  ...
}
```

（C99）省略函数的返回类型在C99中是不合法的，所以最好不要这样做。省略`main`函数参数列表中的`void`是合法的，但是（从编程风格的角度看）最好显式地表明`main`函数没有参数。

`main`函数返回的值是状态码，在某些操作系统中程序终止时可以检测到状态码。如果程序正常终止，`main`函数应该返回0；为了表示异常终止，`main`函数应该返回非0多值。（实际上，这一返回值也可以用于其他目的。）即使不打算使用状态码，确保每个C程序都返回状态码也是一个很好的实践，因为以后运行程序的人可能需要测试状态码。

**`exit`函数**

在`main`函数中执行`return`语句是终止程序的一种方法，另一种方法是调用`exit`函数，此函数属于`<stdlib.h>`头。传递给`exit`函数的实际参数和`main`函数的返回值具有相同的含义：两者都说明程序终止时的状态。为了表示正常终止，传递0:

```c
exit(0);			/* normal termination */
```

因为0有点模糊，所以C语言允许用`EXIT_SUCCESS`来代替（效果是相同的）：

```c
exit(EXIT_SUCCESS);			/* normal termination */
```

传递`EXIT_FAILURE`表示异常终止：

```c
exit(EXIT_FAILURE);			/* abnormal termination */
```

`EXIT_SUCCESS`和`EXIT_FAILURE`都是定义在`<stdlib.h>`中的宏。`EXIT_SUCCESS`和`EXIT_FAILURE`的值都是由实现定义的，通常分别为0和1。

作为终止程序的方法，`return`语句和`exit`函数关系紧密。事实上，`main`函数中的语句

```c
return expression ;
```

等价于

```c
exit(expression);
```

`return`语句和`exit`函数之间的差异是：不管哪个函数调用`exit`函数都会导致程序终止，`return`语句仅当由`main`函数调用时才会导致程序终止。一些程序员只会使用`exit`函数，以便更容易定位程序中的全部退出点。

## 9.6 递归

如果函数调用它本身，那么此函数就是***递归的***（recursive）。例如，利用公式$n!=n\times(n-1)!$，下面的函数可以递归地计算出$n!$的结果：

```c
int fact(int n)
{
  if (n <= 1)
    return 1;
  else
    return n * fact(n-1);
}
```
