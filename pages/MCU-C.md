- {{cards [[MCU-C]] }}
- #card #MCU-C 单片机C语言知识：用预处理指令#define 声明一个常数，用以表明1年中有多少秒（忽略闰年问题）
  template:: MCUC
  card-last-interval:: 4
  card-repeats:: 1
  card-ease-factor:: 2.36
  card-next-schedule:: 2022-08-21T07:27:52.497Z
  card-last-reviewed:: 2022-08-17T07:27:52.498Z
  card-last-score:: 3
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
		- #card #MCU-C 单片机C语言知识：Volatile,关键字volatile有什么含意?并给出三个不同的例子
- #card #MCU-C 单片机C语言知识：嵌入式系统总是要用户对变量或寄存器进行位操作。给定一个整型变量a，写两段代码，第一个设置a的bit 3，第二个清除a 的bit 3。在以上两个操作中，要保持其它位不变。
	- ⛔用bit fields。Bit fields是被扔到c语言死角的东西，它保证你的代码在不同编译器之间是不可移植的，同时也保证了的你的代码是不可重用的。我最近不幸看到 Infineon为其较复杂的通信芯片写的驱动程序，它用到了bit fields因此完全对我无用，因为我的编译器用其它的方式来实现bit fields的。从道德讲：永远不要让一个非嵌入式的家伙粘实际硬件的边。
	- #card #MCU-C 单片机C语言知识：嵌入式系统总是要用户对变量或寄存器进行位操作。给定一个整型变量a，写两段代码，第一个设置a的bit 3，第二个清除a 的bit 3。在以上两个操作中，要保持其它位不变。
	- 几个要点：说明常数、|=和&=~操作。
- #card #MCU-C 单片机C语言知识：嵌入式系统经常具有要求程序员去访问某特定的内存位置的特点。在某工程中，要求设置一绝对地址为0x67a9的整型变量的值为0xaa66。编译器是一个纯粹的ANSI编译器。写代码去完成这一任务。
	- 这一问题测试你是否知道为了访问一绝对地址把一个整型数强制转换（typecast）为一指针是合法的。这一问题的实现方式随着个人风格不同而不同。典型的类似代码如下：
	  ```c
	  int *ptr;
	  ptr = (int *)0x67a9;
	  *ptr = 0xaa55;
	  //一个较晦涩的方法是：
	  *(int * const)(0x67a9) = 0xaa55;
	  ```
- #card #MCU-C 单片机C语言知识：中断是嵌入式系统中重要的组成部分，这导致了很多编译开发商提供一种扩展—让标准C支持中断。具代表事实是，产生了一个新的关键字 `__interrupt`。下面的代码就使用了`__interrupt`关键字去定义了一个中断服务子程序(ISR)，请评论一下这段代码的。
  ```c
  __interrupt double compute_area (double radius)
  {
  	double area = PI * radius * radius;
  	printf("nArea = %f", area);
  	return area;
  }
  ```
	- 这个函数有太多的错误了，以至让人不知从何说起了：
		- 1) ISR 不能返回一个值。如果你不懂这个，那么你不会被雇用的。
		- 2) ISR 不能传递参数。如果你没有看到这一点，你被雇用的机会等同第一项。
		- 3) ^^在许多的处理器/编译器中，浮点一般都是不可重入的^^。有些处理器/编译器需要让额处的寄存器入栈，有些处理器/编译器就是不允许在ISR中做浮点运算。此外，ISR应该是短而有效率的，在ISR中做浮点运算是不明智的。
		- 4) 与第三点一脉相承，^^printf()经常有重入和性能上的问题^^。如果你丢掉了第三和第四点，我不会太为难你的。不用说，如果你能得到后两点，那么你的被雇用前景越来越光明了。
- #card #MCU-C 单片机C语言知识：测试你是否懂得C语言中的整数自动转换原则，我发现有些开发者懂得极少这些东西。
	- 下面的代码输出是什么，为什么？
	  ```c
	  void foo(void)
	  {
	  	unsigned int a = 6;
	  	int b = -20;
	  	(a+b > 6) ? puts("> 6") : puts("<= 6");
	  }
	  ```
	- 这无符号整型问题的答案是输出是 ">6"。原因是当表达式中存在有符号类型和无符号类型时所有的操作数都自动转换为无符号类型。因此-20变成了一个非常大的正整数，所以该表达式计算出的结果大于6。这一点对于应当频繁用到无符号数据类型的嵌入式系统来说是丰常重要的。如果你答错了这个问题，你也就到了得不到这份工作的边缘。
	- [[C语言中的类型自动转换规则]]
- #card #MCU-C 单片机C语言知识：揭露出应试者是否懂得处理器字长的重要性。
	- 评价下面的代码片断：
	  ```c
	  unsigned int zero = 0;
	  /*1''s complement of zero */
	  unsigned int compzero = 0xFFFF;
	  
	  //对于一个int型不是16位的处理器来说，上面的代码是不正确的。应编写如下：
	  unsigned int compzero = ~0;
	  ```
	- 好的嵌入式程序员非常准确地明白硬件的细节和它的局限，然而PC机程序往往把硬件作为一个无法避免的烦恼。
	- 到了这个阶段，应试者或者完全垂头丧气了或者信心满满志在必得。如果显然应试者不是很好，那么这个测试就在这里结束了。但如果显然应试者做得不错，那么我就扔出下面的追加问题，这些问题是比较难的，我想仅仅非常优秀的应试者能做得不错。
- #card #MCU-C 单片机C语言知识：尽管不像非嵌入式计算机那么常见，嵌入式系统还是有从堆（heap）中动态分配内存的过程的。那么嵌入式系统中，动态分配内存可能发生的问题是什么？
	- 我期望应试者能提到^^内存碎片^^，^^碎片收集^^的问题，^^变量的持行时间^^等等。这个主题已经在ESP杂志中被广泛地讨论过了（主要是 P.J. Plauger, 他的解释远远超过我这里能提到的任何解释），所有回过头看一下这些杂志吧！让应试者进入一种虚假的安全感觉后，我拿出这么一个小节目：
	- 下面的代码片段的输出是什么，为什么？
	  ```c
	  char *ptr;
	  
	  if ((ptr = (char *)malloc(0)) == NULL)
	  	puts("Got a null pointer");
	  else
	  	puts("Got a valid pointer");
	  ```
	- 这是一个有趣的问题。最近在我的一个同事不经意把0值传给了函数malloc，得到了一个合法的指针之后，我才想到这个问题。这就是上面的代码，该代码的输出是"Got a valid pointer"。我用这个来开始讨论这样的一问题，看看被面试者是否想到库例程这样做是正确。得到正确的答案固然重要，但解决问题的方法和你做决定的基本原理更重要些。^^malloc的返回值取决于实现^^,在Windows中
		- `void *p = malloc(0);`将在本地堆上分配零长度缓冲区。返回的指针是有效的堆指针。
		  `malloc`最终HeapAlloc使用默认的C运行时堆进行调用RtlAllocateHeap，然后调用，等等。
		  `free(p);`用于HeapFree释放堆上的0长度缓冲区。不释放它会导致内存泄漏。
	- [malloc（）是动态内存分配函数，用来向系统请求分配内存空间。当无法知道内存具体的位置时，想要绑定真正的内存空间，就要用到malloc（）函数。因为malloc只管分配内存空间，并不能对分配的空间进行初始化，所以申请到的内存中的值是随机的，经常会使用memset()进行置0操作后再使用。](https://qastack.cn/programming/2022335/whats-the-point-of-malloc0)
	- 根据规范， malloc（0）将返回“空指针或可以成功传递给free（）的唯一指针”。
		- 这基本上可以让你分配任何东西，但仍然可以通过“艺术家”variables来调用free（）而不用担心。 出于实际的目的，它和做的几乎一样：
		- artist = NULL;
	- C标准说：
		- 如果空间不能分配，则返回空指针。 如果所请求空间的大小为零，则行为是实现定义的：或者返回一个空指针，或者行为就好像大小是非零值一样，除了返回的指针不能用于访问对象。
	- Malloc手册上说：
		- 如果size为0，则malloc（0）返回NULL，或返回一个^^唯一的指针值^^，稍后可以成功传递给free（）。
- #card #MCU-C 单片机C语言知识：Typedef 在C语言中频繁用以声明一个已经存在的数据类型的同义字。也可以用预处理器做类似的事。例如，思考一下下面的例子：
  card-last-interval:: -1
  card-repeats:: 1
  card-ease-factor:: 2.5
  card-next-schedule:: 2022-08-17T16:00:00.000Z
  card-last-reviewed:: 2022-08-17T07:27:37.027Z
  card-last-score:: 1
	- ```c
	  #define dPS struct s *
	  typedef struct s * tPS;
	  ```
	- 以上两种情况的意图都是要定义dPS 和 tPS 作为一个指向结构s指针。哪种方法更好呢？（如果有的话）为什么？
	- 这是一个非常微妙的问题，任何人答对这个问题（正当的原因）是应当被恭喜的。答案是：typedef更好。思考下面的例子：
		- ```c
		  dPS p1,p2;
		  tPS p3,p4;
		  //第一个扩展为
		  struct s * p1, p2;
		  ```
		- 上面的代码定义p1为一个指向结构的指，^^p2为一个实际的结构^^，这也许不是你想要的。第二个例子正确地定义了p3 和p4 两个指针。
- #card #MCU-C 单片机C语言知识：C语言同意一些令人震惊的结构,下面的结构是合法的吗，如果是它做些什么？
	- ```c
	  int a = 5, b = 7, c;
	  c = a+++b;
	  ```
	- 这个问题将做为这个测验的一个愉快的结尾。不管你相不相信，上面的例子是完全合乎语法的。问题是编译器如何处理它？水平不高的编译作者实际上会争论这个问题，根据最处理原则，编译器应当能处理尽可能所有合法的用法。因此，上面的代码被处理成：
	- `c = a++ + b;`
	- 因此, 这段代码持行后`a = 6, b = 7, c = 12`。
	- 如果你知道答案，或猜出正确答案，做得好。如果你不知道答案，我也不把这个当作问题。我发现这个问题的最大好处是这是一个关于代码编写风格，代码的可读性，代码的可修改性的好的话题。
- #card #MCU-C 单片机C语言知识：关于宏
	- ```c
	  //交换2个数值
	  #define SWAP(a, b)\
	          (a) = (a) + (b);\
	          (b) = (a) - (b);\
	          (a) = (a) - (b);\
	  #define MIN(a, b) (((a) < (b)) ? (a) : (b))
	  //已知一个数组table，使用宏定义求出数组元素的个数
	  #define TABLE_SIZE (sizeof(table) / siezof(table[0]))
	  ```
	- ![image.png](../assets/image_1660719466075_0.png)
- #card #MCU-C 单片机C语言知识：使用两个栈来实现一个队列的功能
	- 设两个栈A，B，并将其初始化为空
	- 入队：
		- 将新元素push入栈A;
	- 出队：
		- (1) 判断栈B是否为空
		- (2) 若不为空，则将栈A中的所有元素依次pop出，并push到栈B
		- (3) 将栈B的栈顶元素pop出
	- 这样的实现，入队与出队的平摊复杂度都为O(1)
- #card #MCU-C 单片机C语言知识：如何判断一般程序是由C编译还是由C++编译的？
	- ```c
	  #ifdef __cplusplus
	      cout << "c++";
	  #else
	      printf("c");
	  #endif
	  ```
- #card #MCU-C 单片机C语言知识：分别写出bool、int、float、指针类型的变量a与零比较的语句：
	- ```c
	  //bool
	  if(!a) or if(a)
	  //int
	  if(a == 0)
	  
	  //float:
	  const EXPRESSION EXP = 0.000001
	  if(a < EXP && a >-EXP)
	  //指针：
	  if(a != NULL) or if(a == NULL)
	  
	  ```
	-
- #card #MCU-C 单片机C语言知识：数组和指针的区别
	- 数组要么在静态数据区被创建，要么在栈上被创建。
	- 指针可以随时指向任意类型的内存块。
	  ```c
	  char a[] = “hello”;
	  a[0] = ‘x’;
	  
	  char *p = “world”;//p指向的是常量字符串
	  p[0] = ‘x’;//编译器无法发现该错误，运行时会报错
	  ```
	- 使用sizeof()运算符计算容量
		- 数组可以使用sizeof计算出容量，而sizeof(p)计算得到的是一个指针变量的字节数，一般为4个字节，而不是p所指向的内存容量。
		- 注意：当数组作为函数形参进行传递时，该数组自动退化为同类型指针：
		  ```c
		  void TestBufferSize1(char a[]) {
		      printf("buffer size = %d \r\n", sizeof(a));
		  }
		  
		  void TestBufferSize2(char a[6]) {
		      printf("buffer size = %d \r\n", sizeof(a));
		  }
		  
		  int main(){
		      char b[6] = "12345";
		      printf("b size = %d \r\n", sizeof(b));
		      TestBufferSize1(b);
		      TestBufferSize2(b);
		      system("pause");
		  }
		  //Output
		  b size = 6
		  buffer size = 4
		  buffer size = 4
		  ```
- #card #MCU-C 单片机C语言知识：`#define`和const的区别
	- 1.const有数据类型，宏定义没有数据类型；编译器可以对前者进行安全检查，对于后者不能进行安全检查，只能进行字符替换。
	- 2.有些调试工具可以对const进行调试，而宏定义无法调试。
	- 3.const定义的常量有作用域，而#define不重视作用域，默认定义处到文件结尾。
- #card #MCU-C 单片机C语言知识：什么是预编译，什么时候需要预编译？
	- 预编译又称为预处理，是做代码文本的替换工作，处理#开头的指令，比如拷贝#include包含的头文件代码；#define宏定义的替换。在程序开始编译之前进行。
	- C语言编译系统在对程序编译之前，先进行预处理。预处理主要提供以下功能：
	- （1）宏定义（2）头文件包含（3）条件编译
- #card #MCU-C 单片机C语言知识：堆和栈的区别
	- （1）申请方式：
		- 堆：由程序员申请，在c语言中使用malloc函数申请。（嵌入式单片机应用中不建议使用此函数）。
			- 例如：`p2 = (char *)malloc(10)`，p2本身是在栈上。
			- C++中，new运算符
		- 栈：由系统分配。
	- （2）申请后系统响应：
		- 栈：若当前栈的剩余空间大于申请空间，系统将会给程序提供内存；否则，将提示异常，栈溢出。
		- 堆：操作系统有个记录空闲内存地址的链表，当系统收到程序的申请时，将会遍历该链表，寻找第一个内存空间大于所申请空间的堆节点，然后将该节点从链表中删除，并将该内存空间分配给程序。找到的堆节点的大小不一定正好等于申请的大小，此时系统会将多余的部分重新放入空闲链表中。对于大多数系统，这块内存空间的首地址会记录本次分配的大小，这样，代码中的delete语句才能正确释放该块内存空间。
	- （3）申请大小的限制：
		- 栈：在windows下，栈是向低地址扩展的数据结构，是一块连续的内存区域（栈顶地址与容量是系统提前规定好的）。
		- 堆：堆是向高地址扩展的数据结构，是不连续的内存区域。堆的大小限制于系统中有效的虚拟内存。
	- （4）申请效率：
		- 栈：由系统自动分配，速度快，但程序员无法操作。
		- 堆：由new、malloc分配，速度慢，容易产生内存碎片，但使用方便。
		- Windows下最好使用virtual alloc分配内存，这块内存既不在堆，也不在栈，是直接在进程的地址空间中保留一块内存，速度快，灵活，使用不方便。
	- （5）堆和栈中的存储内容
		- 栈：在函数调用时，是主函数中后的下一条指令（函数调用语句的下一条可执行语句）的地址，然后是函数的各个参数，在大多数C语言编译器中，参数是由右往左入栈的，然后是函数中的局部变量。静态变量不入栈，在全局初始化区域。
			- 当函数调用结束后，局部变量先出栈，然后是参数，最后栈顶指向最开始存的地址，也就是主函数中的下一条指令，程序由该点继续执行。
		- 堆：一般在堆的头部用一个字节存储堆的大小。堆中的具体内容由程序员分配。
	- （6）存取效率比较：在存取中，栈上的数组比指针指向的字符串（堆）块。
- #card #MCU-C 单片机C语言知识：程序的内存是如何分配的
	- C/C++编译的程序占用的内存分为以下几部分。
		- （1） 栈区，编译器自动分配释放，存放函数的参数值，局部变量的值等。操作方式类似于数据结构中的栈。
		- （2） 堆区，由程序员分配释放。若程序员没有进行资源回收，程序结束时，可能会由OS回收。与数据结构中的堆是两回事。
		- （3） 全局区（静态区），全局变量与静态变量是存储在一起的。初始化的全局变量与初始化的静态变量在一块区域，未初始化的全局变量与未初始化的静态变量存储在一起。程序结束后由OS释放
		- （4） 常量区，存储常量，字符串。程序结束后由OS回收。
		- （5） 程序代码区，存放函数体的二进制代码。
	- 示例
	  ```c
	  int a = 0; //全局初始化区域
	  char *p1; //全局未初始化区域
	  
	  int main(int argc, char const *argv[]) {
	      int b; //栈
	      char s = "sss"; //栈
	      char *p2; //栈
	      char *p3 = "123456"; //p3在栈上，“123456\0”在常量区
	      static int c = 0; //全局初始化区域
	  
	      p1 = (char *)malloc(10); //分配得到的区域在堆上
	      p2 = (char *)malloc(20);
	      strcpy(p1, "123456"); //“123456\0”在常量区
	      return 0;
	  }
	  ```
- #card #MCU-C 单片机C语言知识：全局变量能不能定义在可被多个.c文件包含的头文件中？
	- 可以。在不同的C文件中以static的形式来声明同名全局变量。
	- 可以在不同的C文件中声明同名的全局变量，前提是只能在一个C文件中对变量赋初值，此时链接不会报错。
- #card #MCU-C 单片机C语言知识：全局变量与局部变量是否存在区别？
	- 全局变量存在静态数据区，局部变量存在栈中