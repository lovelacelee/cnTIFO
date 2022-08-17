- {{cards [[MCU-C]] }}
- #card #MCU-C 单片机C语言知识：用预处理指令#define 声明一个常数，用以表明1年中有多少秒（忽略闰年问题）
  template:: MCUC
	- `#define SECONDS_PER_YEAR (60 * 60 * 24 * 365)UL`
	- 考查：
		- 1) `#define` 语法的基本知识（例如：不能以分号结束，括号的使用，等等）
		- 2) 懂得预处理器将为你计算常数表达式的值，因此直接写出你如何计算一年中有多少秒而不是计算出实际的值，是更清晰而没有代价的。
		- 3) 意识到这个表达式将使一个16位机的整型数溢出-因此要用到长整型符号L,告诉编译器这个常数是的长整型数。
		- 4) 如果你在你的表达式中用到UL（表示无符号长整型），那么你有了一个好的起点。
- #card #MCU-C 单片机C语言知识：写一个"标准"宏MIN ，这个宏输入两个参数并返回较小的一个。
	- `#define MIN(A,B) （（A） <= (B) ? (A) : (B))`
	- 这个测试是为下面的目的而设的：
		- 1) 标识`#define`在宏中应用的基本知识。这是很重要的。因为在 嵌入(inline)操作符 变为标准C的一部分之前，宏是方便产生嵌入代码的唯一方法，对于嵌入式系统来说，为了能达到要求的性能，嵌入代码经常是必须的方法。
		- 2) 三重条件操作符的知识。这个操作符存在C语言中的原因是它使得编译器能产生比if-then-else更优的代码，了解这个用法是很重要的。
		- 3) 懂得在宏中小心地把参数用括号括起来
		- 4) 我也用这个问题开始讨论宏的副作用，例如：当你写下面的代码时会发生什么事？
			- least = MIN(*p++, b);
- #card #MCU-C 单片机C语言知识：预处理器标识#error的目的是什么？
	- 跳出一个编译错误，其目的就是保证程序是按照你所设想的那样进行编译的。
- #card #MCU-C 单片机C语言知识：嵌入式系统中经常要用到无限循环，你怎么样用C编写死循环呢？
	- for,while,goto
	- 当知道`for(;;)`更好时，考查（是不是被教着这么做的，或者示例这么做的）：
		- 1) 哪个更优？为什么用`for(;;){}`
			- `for( ; ; )`效率略高于`while(1)`
			  因为编译器在执行的时候会对语句进行优化， `for( ; ; )`是空语句所以会直接执行循环体内容，而编译器需要对`while(1)`进行判断。
			- 也就是说`for(;;)`比`while(1)`在编辑器优化后，执行更少的指令，不占用寄存器，而且没有判断跳转。
- #card #MCU-C 单片机C语言知识：用变量a给出下面的定义
	- a) 一个整型数（An integer）
		- `int a; // An integer`
	- b)一个指向整型数的指针（ A pointer to an integer）
		- `int *a; // A pointer to an integer`
	- c)一个指向指针的的指针，它指向的指针是指向一个整型数（ A pointer to a pointer to an integer）
		- `int **a; // A pointer to a pointer to an integer`
	- d)一个有10个整型数的数组（ An array of 10 integers）
		- `int a[10]; // An array of 10 integers`
	- e) 一个有10个指针的数组，该指针是指向一个整型数的。（An array of 10 pointers to integers）
		- `int *a[10]; // An array of 10 pointers to integers`
	- f) 一个指向有10个整型数数组的指针（ A pointer to an array of 10 integers）
		- `int (*a)[10]; // A pointer to an array of 10 integers`
	- g) 一个指向函数的指针，该函数有一个整型参数并返回一个整型数（A pointer to a function that takes an integer as an argument and returns an integer）
		- `int (*a)(int); // A pointer to a function a that takes an integer argument and returns an integer`
	- h) 一个有10个指针的数组，该指针指向一个函数，该函数有一个整型参数并返回一个整型数（ An array of ten pointers to functions that take an integer argument and return an integer ）
		- `int (*a[10])(int); // An array of 10 pointers to functions that take an integer argument and return an integer`
- #card #MCU-C 单片机C语言知识：关键字static的作用是什么？
	- 在C语言中，关键字static有三个明显的作用：
		- 1) 在函数体，一个被声明为静态的变量在这一函数被调用过程中维持其值不变。
		- 2) 在模块内（但在函数体外），一个被声明为静态的变量可以被模块内所用函数访问，但不能被模块外其它函数访问。它是一个本地的全局变量。
		- 3) 在模块内，一个被声明为静态的函数只可被这一模块内的其它函数调用。那就是，这个函数被限制在声明它的模块的本地范围内使用。
	- 大多数应试者能正确回答第一部分，一部分能正确回答第二部分，同是很少的人能懂得第三部分。这是一个应试者的严重的缺点，因为他显然不懂得本地化数据和代码范围的好处和重要性。
- #card #MCU-C 单片机C语言知识：关键字const有什么含意？
	- 只要一听到被面试者说："const意味着常数"，我就知道我正在和一个业余者打交道。去年Dan Saks已经在他的文章里完全概括了const的所有用法，因此ESP(译者：Embedded Systems Programming)的每一位读者应该非常熟悉const能做什么和不能做什么.如果你从没有读到那篇文章，只要能说出const意味着"只读"就可以了。尽管这个答案不是完全的答案，但我接受它作为一个正确的答案。（如果你想知道更详细的答案，仔细读一下Saks的文章吧。）
	- 如果应试者能正确回答这个问题，我将问他一个附加的问题：
	- 下面的声明都是什么意思？
		- const int a;
		- int const a;
			- 前两个的作用是一样，a是一个常整型数。
		- const int *a;
			- a是一个指向常整型数的指针（也就是，整型数是不可修改的，但指针可以）。
		- int * const a;
			- a是一个指向整型数的常指针（也就是说，指针指向的整型数是可以修改的，但指针是不可修改的）。
		- int const * a const;
			- a是一个指向常整型数的常指针（也就是说，指针指向的整型数是不可修改的，同时指针也是不可修改的）。
	- 为什么要关注Const
		- 1) 关键字const的作用是为给读你代码的人传达非常有用的信息，实际上，声明一个参数为常量是为了告诉了用户这个参数的应用目的。如果你曾花很多时间清理其它人留下的垃圾，你就会很快学会感谢这点多余的信息。（当然，懂得用const的程序员很少会留下的垃圾让别人来清理的。）
		- 2) 通过给优化器一些附加的信息，使用关键字const也许能产生更紧凑的代码。
		- 3) 合理地使用关键字const可以使编译器很自然地保护那些不希望被改变的参数，防止其被无意的代码修改。简而言之，这样可以减少bug的出现。
- #card #MCU-C 单片机C语言知识：Volatile,关键字volatile有什么含意?并给出三个不同的例子
	- 一个定义为volatile的变量是说这^^变量可能会被意想不到地改变^^，这样，编译器就不会去假设这个变量的值了。精确地说就是，^^优化器在用到这个变量时必须每次都小心地重新读取这个变量的值，而不是使用保存在寄存器里的备份。^^下面是volatile变量的几个例子：
		- 1) 并行设备的硬件寄存器（如：状态寄存器）
		- 2) 一个中断服务子程序中会访问到的非自动变量(Non-automatic variables)
		- 3) 多线程应用中被几个任务共享的变量
	- 回答不出这个问题的人是不会被雇佣的。我认为这是区分C程序员和嵌入式系统程序员的最基本的问题。搞嵌入式的家伙们经常同硬件、中断、RTOS等等打交道，所有这些都要求用到volatile变量。不懂得volatile的内容将会带来灾难。
	- 假设被面试者正确地回答了这是问题（嗯，怀疑是否会是这样），我将稍微深究一下，看一下这家伙是不是直正懂得volatile完全的重要性。
		- 1) 一个参数既可以是const还可以是volatile吗？解释为什么。
			- 是的。一个例子是只读的状态寄存器。它是volatile因为它可能被意想不到地改变。它是const因为程序不应该试图去修改它。
		- 2) 一个指针可以是volatile 吗？解释为什么。
			- 是的。尽管这并不很常见。一个例子是当一个中断服务子程序修该一个指向一个buffer的指针时。
		- 3) 下面的函数有什么错误：
		  ```c
		  int square(volatile int *ptr)
		  {
		  	return *ptr * *ptr;
		  }
		  //这段代码有点变态。这段代码的目的是用来返指针*ptr指向值的平方，
		  //但是，由于*ptr指向一个volatile型参数，
		  //编译器将产生类似下面的代码：
		  int square(volatile int *ptr)
		  {
		    int a,b;
		    a = *ptr;
		    b = *ptr;
		    return a * b;
		  }
		  ```