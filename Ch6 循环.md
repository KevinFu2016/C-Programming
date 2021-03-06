#Ch6 循环

本章介绍C语言的重复语句，这种语句允许用户设置循环

**循环**（loop）是重复执行其他语句（循环体）的一种语句。在C语言中，每个循环都有一个**控制表达式**（controlling expression）。每次执行循环体（循环重复一次）时都要对控制表达式求值。如果表达式为真（即值不为零），那么继续执行循环。



本章内容

C语言提供了3种重复语句，即`while`语句、`do`语句和`for`语句

主要用于`for`语句的逗号运算符

`break`语句、`continue`语句和`goto`语句：`break`语句用来跳出循环并把程序控制传递到循环后的下一条语句，`continue`语句用来跳过本次循环的剩余部分，`goto`语句可以跳到函数内的任何语句上。

空语句，它可以用于构造循环体为空的循环

## 6.1 `while`语句

在C语言所有设置循环的方法中，`while`语句是最简单也是最基本。`while`语句的格式如下所示：

```c
while ( expression ) statement
```

圆括号内的表达式是控制表达式，圆括号后边的语句是循环体。下面是一个示例：

```c
while (i < n)		/* controlling expression */
  i = i * 2;		/* loop body */
```

执行`while`语句时，首先计算控制表达式的值。如果值不为零（即真值），那么执行循环体，接着再次判定表达式。这个过程（先判定控制表达式，再执行循环体）持续进行直到控制表达式的值变为零才停止。

**无限循环**

如果控制表达式的值始终非零，`while`语句将无法终止。事实上，C程序员有时故意用非零常量作为控制表达式来构造无限循环（infinite loop)

```c
while (1) ...
```

除非循环体中含有跳出循环控制的语句(`break`、`goto`、`return`)或者调用了导致程序终止的函数，否则上述形式的`while`语句将永远执行下去。

## 6.2 `do`语句

`do`语句和`while`语句关系紧密。事实上，`do`语句本质上就是`while`语句，只不过其控制表达式是在每次执行完循环体之后进行判定的。`do`语句的格式如下所示：

```c
do statement while ( expression )
```

和处理`while`语句一样，`do`语句的循环体也必须是一条语句（当然可以用复合语句），并且控制表达式的外面也必须有圆括号。

执行`do`语句时，先执行循环体，再计算控制表达式的值。如果表达式的值是非零的，那么再次执行循环体，然后再次计算表达式的值。在循环体执行后，若控制表达式值变为0，则终止`do`语句的执行。

```c
i = 10;
do {
  printf("T minus %d and counting\n", i);
  --i;
} while (i > 0);
```

## 6.3 `for`语句

`for`语句非常适合应用在使用”计数“变量的循环中，当然它也可以灵活地用于许多其他类型的循环中。

`for`语句的格式如下所示：

```c
for ( expr1 ; expr2 ; expr3 ) statement
```

举个例子

```c
for (i = 10; i > 10; i--)
  printf("T minus %d and counting\n", i);
```

### 6.3.1 `for`语句的惯用法

对于“向上加”（变量自增）或“向下减”（变量自减）的循环来说，`for`语句通常是最好的选择。对于向上加或向下减共n次的情况，`for`语句经常会采用下列形式中的一种。

- 从`0`向上加到`n-1`

  ```c
  for ( i = 0; i < n; i++) ...
  ```

- 从`1`向上加到`n`

  ```c
  for ( i = 1; i <= n; i++) ...
  ```

- 从`n-1`向下减到`0`

  ```c
  for ( i = n - 1; i >= 0; i--) ...
  ```

- 从`n`向下减到`1`

  ```c
  for ( i = n; i > 0; i--) ...
  ```

### 6.3.2 在`for`语句中省略表达式

`for`语句远比目前看到的更加灵活。通常`for`语句用三个表达式控制循环，但是有一些`for`循环可能不需要这么多，因此C语言允许省略任意或全部的表达式。

如果省略第一个表达式，那么在执行循环前没有初始化的操作：

```c
i = 10;
for (; i > 0; --i)
  printf("T minus %d and counting\n", i);
```

在这个例子中，变量`i`由一条单独的赋值语句实现了初始化，所以在`for`语句中省略了第一个表达式。（注意，保留第一个表达式和第二个表达式之间的分号。即使省略掉某些表达式，控制表达式也必须始终有两个分号。）

如果省略了`for`语句中的第三个表达式，循环体需要确保第二个表达式的值最终会变为假。我们的`for`语句示例可以这样写：

```c
for (i = 10; i > 0;)
  printf("T minus %d and counting\n", i--);
```

为了补偿省略第三个表达式产生的后果，我们使变量`i`在循环体中进行自减。

当for语句同时省略掉第一个和第三个表达式时，它和`while`语句没有任何区别。例如，循环

```c
for (; i > 0;)
  printf("T minus %d and counting\n", i--);
```

等价于

```c
while (i > 0)
  printf("T minus %d and counting\n", i--);
```

这里`while`语句的形式更清楚，也因此更可取。

如果省略第二个表达式，那么它默认为真值，因此`for`语句不会终止（除非以某种其他形式停止）。例如，某些程序员用下列的`for`语句建立无限循环：

```c
for ( ; ; ) ...
```



### 6.3.3 C99中的`for`语句（C99）

在C99中，`for`语句的第一个表达式可以替换为一个声明，这一特性使得程序员可以声明一个用于循环的变量：

```c
for (int i = 0; i < n; i++)
  ...
```

变量`i`不需要在该语句前进行声明。事实上，如果变量`i`在之前已经进行了声明，这个语句将创建一个新的`i`且该值仅用于循环内。

`for`语句声明的变量不可以在循环外访问（在循环体外不可见）：

```c
for (int i = 10; i < n; i++){
  ...
  printf("%d", i);		/*legal; i is visible inside loop */
}
printf("%d", i);			/***WRONG***/
```

`for`语句可以声明多个变量，只要它们的类型相同：

```c
for (int i = 0, j = 0; i < n; i++)
  ...
```

### 6.3.4 逗号运算符

有些时候，我们可能喜欢编写有两个（或更多个）初始表达式的`for`语句，或者希望在每次循环时一次对几个变量进行自增操作。使用***逗号表达式***（comma expression）作为for语句中第一个或第三个表达式可以实现这些想法。

逗号表达式的格式如下所示：

```c
expr1 , expr2
```

这里的`expr1`和`expr2`是两个任意的表达式。逗号表达式的计算要通过两步来实现：第一步，计算expr1并且扔掉计算出的值。第二步，计算`expr2`，把这个值作为整个表达式的值。对`expr1`的计算应该始终会有副作用；如果没有，那么`expr1`就没有了存在的意义。

例如，假设变量`i`和变量`j`的值分别为1和5，当计算逗号表达式`++i`，`i+j`时，变量`i`先进行自增，然后计算`i+j`，所以表达式的值为7。（而且，显然现在变量`i`的值为2。）顺便说一句，逗号运算符的优先级低于所有其他运算符，所以不需要在`++i`和`i+j`外面加圆括号。

## 6.4 退出循环

### 6.4.1 `break`语句

对于退出点在循环体的中间而不是循环体之前或之后的情况，`break`语句特别有用。读入用户输入并且在遇到特殊输入值时终止的循环常常属于这种类型：

```c
for (;;) {
  printf("Enter a number (enter 0 to stop): ");
  scanf("%d", &n);
  if (n == 0)
    break;
  printf("%d cubed is %d\n", n, n * n * n);
}
```

`break`语句把程序控制从包含该语句的最内层`while`、`do`、`for`或`switch`语句中转移出来。因此，当这些语句出现嵌套时，`break`语句只能跳出一层嵌套。

### 6.4.2 `continue`语句

`break`语句刚好把程序控制转移到循环体末尾之后，而`continue`语句刚好把程序控制转移到循环体末尾之前。用`break`语句会使程序控制跳出循环，而`continue`语句会把程序控制留在循环内。`break`语句和`continue`语句的另外一个区别是：`break`语句可以用于`switch`语句和循环（`while`、`do`和`for`），而`continue`语句只能用于循环。

下面的例子通过读入一串数并求和的操作数说明了`continue`语句的简单应用。循环在读入10个非零数后循环终止。无论何时读入数0都执行`continue`语句，控制将跳过循环体的剩余部分（即语句`sum += i;`和语句`n++;`）但仍留在循环内。

```c
n = 0;
sum = 0;
while (n < 10) {
  scanf("%d", &i);
  if (i == 0)
    continue;
  sum += i;
  n++;
  /* continue jumps to here */
}
```

### 6.4.3 `goto`语句

`break`语句和`continue`语句都是跳转语句：它们把控制从程序中的一个位置转移到另一个位置。这两者都是受限制的：`break`语句的目标是包含该语句的循环结束之后的那一点，而`continue`语句的目标是循环结束之前的那一点。`goto`语句则可以跳转到函数中任何有标号的语句处。（C99增加了一条限制：`goto`语句不可以用于绕过变长数组的声明。）

标号只是放置在语句开始处的标识符：

```c
identifier : statement
```

一条语句可以有多个标号。`goto`语句自身的格式如下：

```c
goto identifier ;
```

执行语句`goto L;`，控制会转移到标号`L`后面的语句上，而且该语句必须和`goto`语句在同一个函数中。

如果C语言中没有`break`语句，可以用下面的`goto`语句提前退出循环：

```c
for (d = 2; d < n; d++)
  if (n % d == 0)
    goto done;
done:
if (d < n)
  printf("%d is divisible by %d\n", n, d);
else
  printf("%d is prime\n", n);
```

```c
while (...) {
  switch (...) {
      ...
      goto loop_done;		/* break won't work here */
      ...
  }
}
loop_done:...
```

`goto`语句对于嵌套循环的退出也是很有用的。

## 6.5 空语句

语句可以为空（null），也就是除了末尾处的分号以外什么符号也没有。

空语句主要有一个好处：编写空循环体的循环。

```c
for (d = 2; d < n && n % d != 0; d++)
  /* empty loop body */;
```

不小心在`if`、`while`或`for`语句的圆括号后面放置分号会创建空语句，从而造成`if`、`while`或`for`语句提前结束。
