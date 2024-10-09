**C#——刘铁猛笔记**

---

# 类、名称空间（简述）

类（class）是构成程序的主体

名称空间（namespace）以**树形结构**组织类（其他类型）

```text
名称空间：名称空间是用来组织和管理类、接口、结构体等类型的逻辑容器。它可以帮助开发人员避免命名冲突，将相关的类型组织在一起，提高代码的可读性和可维护性。名称空间可以嵌套使用，形成层次结构。在C#中，使用namespace关键字来定义名称空间。
```

打个比方：

在一个图书馆中，每本书都会有它专属的类放置在一个特定的位置上，那么名称空间就相当于一个图书馆，类就相当于书的类。不同的图书馆有些书的类一样，但其所含书本不一样，因此名称空间也能防止类的名字冲突。

当程序中的一个名称空间中，想要使用另一个名称空间的某一个方法时，需要在程序中名称空间的外部使用using引用所需的名称空间，或者在该名称空间中写出所需名称空间的限定名称。

```c#
namespace HelloWorld
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");

        }
    }
}
```

当引用的名称空间中有两种或两种以上的名称空间中包含同名的类，使用这个类时也需要写出限制性名称。

## 类库

类库引用时使用名称空间的物理基础，使用名称空间时，可以点开References（引用）来检查是否有包含该名称空间的类库（类库简称dll）

```text
在C#中，类库（Class Library）指的是一组封装了一些相关功能的类、接口和其他类型的集合。类库通常用于封装通用的功能，以便在多个应用程序中重复使用。

类库可以包含各种类型的功能，例如数据结构、算法、I/O操作、网络通信、图形界面控件等。C#本身提供了一些标准的类库，如.NET Framework中的类库，它包含了大量的基本功能和工具类，用于支持C#程序的开发。

除了使用.NET Framework提供的类库之外，开发人员也可以自己编写类库，以封装自己的功能并在不同的项目中重复使用。这样可以提高代码的复用性，降低开发成本，同时也有利于代码的维护和管理。

在C#中，类库通常以DLL（Dynamic Link Library）的形式存在，可以通过引用的方式在C#项目中使用。通过引用类库，开发人员可以轻松地使用其中封装的功能，而无需关心具体的实现细节，从而提高开发效率并降低代码的重复编写。

一个类库可以包含一个或多个名称空间，用于组织其中的类型。
```

### 黑盒引用（无法看源代码）

```text
黑盒引用（Black Box Reference）：指的是只能通过接口或基类来引用对象的方式。在黑盒引用中，只能访问对象的公共成员或通过接口暴露的方法，而无法直接访问对象的私有成员或实现细节。这种引用方式更加封装和安全，符合面向对象编程的封装原则。
```

- 引用一个类库时，右击reference

- 引用一个类库时，它可能受限于更底层的类库，因此引用时还需要引用其更底层的类库，为了避免这个麻烦，可以引用NuGet，类似于一个类库包。




### 白盒引用

```text
白盒引用（White Box Reference）：指的是可以直接访问对象的私有成员或实现细节的引用方式。在白盒引用中，可以直接访问对象的所有成员，包括私有成员和实现细节。这种引用方式可能会破坏对象的封装性，增加耦合度，不利于代码的维护和扩展。
```

引用自己已有的项目，此时需要将该项目添加到自己的solution里面，之后再右击reference，再solution里勾选该类库。

![image-20240915215719823](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240915215719823.png)

图中有新建项和现有项，若要使用新建项，在打开时就要选择class library项目。

```text
总的来说，黑盒引用更符合面向对象编程的封装和抽象原则，而白盒引用则更容易导致代码的耦合和依赖性。在设计和编写代码时，应该尽量使用黑盒引用，只暴露必要的接口和方法，避免直接暴露对象的内部实现细节。
```

## 依赖关系

```text
在C#中，依赖关系指的是一个类或对象在实现功能时依赖于另一个类或对象的情况。依赖关系是面向对象编程中的一个重要概念，它描述了一个对象使用另一个对象提供的功能或服务的情况。

依赖关系通常体现在类之间的关联或调用关系上。当一个类需要使用另一个类的功能时，它就会依赖于这个类。这种依赖关系可以通过构造函数注入、属性注入或方法参数传递等方式来实现。

依赖关系的存在可以带来一些好处，如提高代码的复用性、降低耦合度、便于单元测试等。但如果依赖关系设计不当，可能会导致代码的脆弱性、难以维护和扩展等问题。

在实际开发中，我们通常会借助依赖注入（Dependency Injection）等技术来管理和解决类之间的依赖关系，以提高代码的可维护性和灵活性。通过合理设计和管理依赖关系，可以使代码更加模块化、可测试和可扩展。
```

# 类、对象、类与对象的关系

## 类

类（Class）是一种模板或蓝图，用于描述对象的属性和行为。类定义了对象的结构和行为，包括属性（字段）和方法。在C#中，类是定义对象的基本单位，通过类可以创建对象的实例。

 

## 对象

在C#中，对象（Object）是类的实例化（Instance）结果，是内存中的一个具体实体，具有属性和行为。对象是类的具体化，通过实例化类可以创建对象。对象在内存中占据一定的空间，包含了类中定义的属性和方法的具体数值和实现。

## 类与对象的关系

![image-20240916101828223](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916101828223.png)

- 类是对象的模板，定义了对象的属性和行为，描述了对象的结构。
- 对象是类的实例化结果，是类的具体实体，具有类定义的属性和行为。
- 通过类可以创建多个对象，每个对象都是类的一个实例，但它们在内存中是独立存在的，各自拥有自己的属性值。
- 类定义了对象的结构和行为，而对象是类的具体化，实际应用中我们操作的是对象。

简单来说，就是你自己写了一个类，这个类就像一个概念一样，它包含着一些用来形容这个概念的东西，你想要使用这个类时候，你就需要创建一个对象，之后用这个对象来进行你后续的操作。

比如你写了一个飞机的类，但这个类是个概念，你能开概念吗？肯定不行，你要开的是飞机这个机器，所以你就要创建一个飞机开。

 

## 类的三大成员

![b6e38f83743022f23b3ea697c0d33ca](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/b6e38f83743022f23b3ea697c0d33ca.png)

前三个后面会详细说明

 

### 小知识--使用MSDN文档

把光标移到你所使用的类上，按f1键

![2309e111171f117460341aee17a7303f](C:/Users/庞亚琪/Desktop/2309e111171f117460341aee17a7303f.png)

## 静态成员与非静态成员



![2a780fd023badbd3bf9281b7dd35ce4](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/2a780fd023badbd3bf9281b7dd35ce4.png)

```
静态成员是属于类的成员，而不是属于类的实例（对象)的成员。静态成员可以被类的所有实例共享，可以通过类名直接访问，不需要创建类的实例。在内存中，静态成员只有一份拷贝，被所有实例共享。

非静态成员则是属于类的实例（对象）的成员，每个对象都有自己的一份。非静态成员需要通过对象来访问，每个对象都有自己的非静态成员的拷贝。
```

构成c#的语言的基本元素：关键字，操作符，标识符，标识符号，文本，注释。

### 小知识--声明变量和类时名称的要求

声明变量时变量名采用**驼峰命名法**，即首单词字母要小写，后续单词字母大写

声明一个类时名称时，单词都要大写

# 类型

## 什么是类型

![image-20240916102321322](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916102321322.png)

## 强类型语言、弱类型语言

强类型指的是声明的变量是什么类型，以后它就是什么类型，弱类型指声明的变量可以是多个类型

- 强类型语言是指在编程时要求严格定义变量的数据类型，不允许不同类型之间的隐式转换。
- 在强类型语言中，变量的数据类型必须在编译时就确定，并且不会发生自动类型转换。
- 强类型语言通常具有更严格的类型检查，能够在编译时捕获一些潜在的类型错误。



- 弱类型语言是指在编程时对变量的数据类型要求较为灵活，允许不同类型之间的隐式转换。
- 在弱类型语言中，变量的数据类型通常可以在运行时动态确定，允许自动类型转换。
- 弱类型语言通常具有更灵活的类型系统，但也可能导致一些难以发现的类型错误。

### 小知识--c#中用dynamic定义的变量可以让该变量在不同时刻赋任何类型的值。

```c#
namespace DynamicSample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            dynamic myVar = 100;
            Console.WriteLine(myVar);
            myVar = "Mr.Okay!";
            Console.WriteLine(myVar);
        }
    }
}
```

![image-20240916102509962](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916102509962.png)

## 类型在C#语言中的作用

![image-20240916102604607](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916102604607.png)

## C#语言的类型系统--五大数据类型

![2a1e480894b4e0fe6a028ae0d20a6f8](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/2a1e480894b4e0fe6a028ae0d20a6f8.png)

## 变量

![13216518a4be8b514b28bea0f688be6](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/13216518a4be8b514b28bea0f688be6.png)


变量是以变量名为对应的内存地址为起点，以其数据类型所要求的存储空间为长度的一块内存区域（变量名中要么存储的是值类型的值，或者引用类型的地址）。

### 静态变量

- 静态变量属于类，而不是类的特定实例。
- 静态变量在类加载时被初始化，只有一份存储空间，所有类的实例共享同一份静态变量。
- 静态变量使用static关键字声明，在内存中存储在静态存储区域。
- 可以通过类名直接访问静态变量，无需实例化对象。
- 通常用于存储类级别的信息，如常量、计数器等。

![image-20240916102721417](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916102721417.png)

### 实例变量

- 实例变量属于类的实例（对象），每个对象都有自己的实例变量副本。
- 实例变量在创建对象时被分配内存空间，并随着对象的销毁而释放。
- 实例变量不使用static关键字声明，每个对象都有自己的实例变量。
- 必须通过对象实例来访问实例变量。

![image-20240916102747541](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916102747541.png)

### 数组元素

```
数组元素（Array Element）：数组是一种数据结构，用于存储相同类型的多个元素。数组元素指的是数组中的单个数据项，通过索引访问。例如，对于整型数组 int[] numbers = {1, 2, 3, 4, 5};，numbers[0] 表示数组 numbers 中的第一个元素，其值为 1。
```



### 值参数

```
值参数（Value Parameter）：值参数是一种参数传递方式，将参数的值传递给方法。在方法内部对值参数的修改不会影响到原始值。在方法签名中，参数前没有 ref 或 out 关键字的参数均为值参数。
```



### 引用参数

```
引用参数（Reference Parameter）：引用参数是一种参数传递方式，将参数的引用传递给方法，使得在方法内部对参数的修改会影响到原始值。在方法签名中，参数前加上 ref 关键字表示引用参数。
```



### 输出参数

```
输出参数（Output Parameter）：输出参数是一种特殊的引用参数，用于从方法中返回多个值。在方法签名中，参数前加上 out 关键字表示输出参数，调用方法时必须为输出参数赋值。
```



### 局部变量

```
局部变量（Local Variable）：局部变量是在方法、构造函数或代码块内部声明的变量，只在声明的作用域内有效。局部变量在声明时必须初始化，可以在声明时或稍后赋值。
```



## 值类型、引用类型声明的变量在内存中的分配

值类型在内存分配的时候会根据该类型所需的空间来分配字节，引用类型在内存分配的时候只会分配四个字节，没有引用实例时该字节的二进制都是零，当创建实例后会把堆内存的地址放进该变量中，在堆内存中分配空间时才会计算该变量真正所需要的空间。

## 变量的默认值

成员变量分配好空间后不赋值时会自动都赋值成0，但本地变量不赋值会报错。

## 装箱和拆箱

装箱：当一个引用类型的变量发现赋予的值时放在栈上的值类型的值，那么其会在堆中的一个空位置上存储这个值，并把该位置的地址赋值给引用类型的变量上。

- 装箱是将值类型转换为引用类型的过程。

- 当将值类型赋值给一个对象类型（如 object）时，会发生装箱操作，将值类型的数据包装在一个堆分配的对象中。
- 装箱会导致性能开销，因为需要在堆上分配内存来存储值类型的数据，并且需要进行数据复制。

拆箱：当一个值类型的变量发现赋予的值时引用类型，那么其会根据地址找到该值赋给自己

- 拆箱是将引用类型转换为值类型的过程。
- 当从对象类型（如 object）中获取值类型数据时，需要进行拆箱操作，将包装在对象中的数据提取出来转换为值类型。
- 拆箱需要进行类型检查和数据复制，可能会导致性能开销。

# 方法

## 方法的由来

![24af3a470d6768dd6504433907e5f4a](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/24af3a470d6768dd6504433907e5f4a.png)

方法的声明与调用

![5b5801eb8e24dbf673c76109ae18ba5](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/5b5801eb8e24dbf673c76109ae18ba5.png)当一个函数以类的成员出现时简称为方法

## 构造器



![2283c2088a96cb5c20da3b3f0b0491b](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/2283c2088a96cb5c20da3b3f0b0491b.png)

### 什么是构造器

在 C# 中，构造器（Constructor）是一种特殊类型的方法，用于在创建类的实例（对象）时初始化对象的状态。构造器的作用是初始化对象的成员变量、属性或执行其他必要的初始化操作。以下是构造器的一些特点：

**命名与特点：**

1. 构造器的名称与类名相同。

2. 构造器没有返回类型，包括 void。
3. 构造器可以重载，即同一个类可以有多个不同参数列表的构造器。

**初始化对象：**

1. 在实例化对象时，构造器会被自动调用，用于初始化对象的状态。
2. 构造器可以初始化对象的成员变量、属性，或执行其他必要的初始化操作。

**默认构造器：**

1. 如果没有显式定义构造器，C# 会提供一个默认构造器（无参数构造器），用于初始化对象。
2. 如果显式定义了带参数的构造器，但没有定义无参数构造器，那么默认构造器就不会被提供。

### 对象的创建

```
当通过 new 关键字创建一个类的实例时，会在内存中分配一块空间来存储该对象的数据。这个空间包括对象的字段、属性和方法等信息。
```



### 构造器的调用

```
在对象创建的过程中，会调用构造器来初始化对象的各个部分。构造器可以有多个重载形式，根据参数列表的不同进行选择调用。
```



### 内存分配

在调用构造器之前，系统会先为对象分配内存空间。构造器负责对这块内存空间进行初始化，包括对字段、属性等成员变量的赋值操作。

### 构造器的执行顺序

如果类的继承关系中包含父类和子类，构造器的执行顺序是先执行父类的构造器，然后执行子类的构造器。这确保了对象的所有部分都能得到正确的初始化。

### 构造器的作用

构造器可以用来初始化对象的状态，进行一些必要的设置操作，确保对象在创建后处于一个合理的状态。

快捷生成构造器

```c#
//创建构造器快捷键
//ctor 按两下tab键
public Student(int initId,string initName)
{
        
}
```



### 声明无参数构造器（也可以称作默认构造器）

```c#
namespace ConstructorExample
{
     class Program
    {
        static void Main(string[] args)
        {
          
            Student stu2 = new Student();
            Console.WriteLine(stu2.ID);
            Console.WriteLine(stu2.Name);


        }
    }

    class Student
    {

        //创建构造器快捷键
        //ctor 按两下tab键
  

        public Student()
        {
            this.ID = 1;
            this.Name = "No name";
        }

        public int ID;
        public string Name;
    }
}

```



### 声明带参数的构造器（实例化一个对象时必须对构造器内的成员赋值）

为了防止声明一个类时忘记对其字段赋值，可以使用带参数的构造函数。

```c#
namespace ConstructorExample
{
     class Program
    {
        static void Main(string[] args)
        {
            Student stu = new Student(2,"Mr.Okay");
            Console.WriteLine(stu.ID);
            Console.WriteLine(stu.Name);
        }
    }

    class Student
    {

        //声明
        public Student(int initId, string initName)
        {
            this.ID = initId;
            this.Name = initName;
        }

        public int ID;
        public string Name;
    }
}

```



## 重载

![7f209be5881d4f9299f4e3d37b9cafc](D:/WeChat/WeChat Files/wxid_81jv92tofrzm22/FileStorage/Temp/7f209be5881d4f9299f4e3d37b9cafc.png)

### 什么是重载

方法的重载（Method Overloading）是指在同一个类中可以定义多个具有相同名称但参数列表不同的方法。通过方法重载，可以在同一个类中使用相同的方法名来执行不同的操作，根据传入的参数列表的不同来确定调用哪个方法。

比如**Console.WriteLine**就是一种重载，可以输出多种类型

### 小知识--Debug操作

![image-20240916104259609](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916104259609.png)


设置断点，当摁f5时会在设置的断点处停止运行

![image-20240916104350445](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916104350445.png)

此时调用堆栈中第一行是该函数，第二行是调用该函数的位置

Step-in是一个语句一个语句进行，Step-over是若该断点在是一个调用函数，直接返回调用函数后的结果，Steo-out是寻找调用该函数的地方。



# 操作符



![image-20240916113134508](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916113134508.png)

越靠上的操作符优先级越高，越靠下的操作符优先级越低

## 什么是操作符？

![image-20240916114244570](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916114244570.png)

操作符不能脱离与它关联的数据类型

```c#
namespace CreateOperator
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int x = 5;
            int y = 4;
            int z = x / y;
            Console.WriteLine(z);

            double a = 5.0;
            double b = 4.0;
            double c = a / b;
            Console.WriteLine(c);
        }
    }
}
```

演示结果

```
2
Mr.Okay
请按任意键继续. . .
```

简记法

```c#
namespace CreateOperator
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Person person1 = new Person();
            Person person2 = new Person();
            person1.Name = "Dear";
            person2.Name = "Dear's wife";
            List<Person> nation = person1+ person2;
            foreach(var p in nation)
            {
                Console.WriteLine(p.Name);
            }

        }
    }

    //创建一个类型
    class Person
    {
        public string Name;
        //public static List<Person> GetMarry(Person p1, Person p2)
        //简记法————如下
        public static List<Person> operator +(Person p1, Person p2)
        {
            List<Person> people = new List<Person>();
            people.Add(p1);
            people.Add(p2);
            for(int i = 0; i < 11; i++)
            {
                Person child = new Person();
                child.Name = p1.Name + "&" + p2.Name + "s child";
                people.Add(child);
            }
            return people;

        }
    }
}

```

演示结果

```text
Dear
Dear's wife
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
Dear&Dear's wifes child
请按任意键继续. . .
```

## 优先级与运算顺序



![image-20240916144610936](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916144610936.png)

同优先级操作符从左到右

列如：

```c#
namespace OperatorPriority
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int x;
            x = 3 + 4 + 5;
            Console.WriteLine(x);
        }
    }
}
```

结果

```
12
请按任意键继续. . .
```

带有赋值功能的操作符的运算顺序都是从右向左

代码：

```c#
namespace OperatorPriority
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int x = 100;
            int y = 200;
            int z = 300;
            x += y += z;

            Console.WriteLine(x);
            Console.WriteLine(y);
            Console.WriteLine(z);
        }
    }
}

```



```
600
500
300
请按任意键继续. . .
```



外层命名空间的子集命名空间（静态成员属于类）

## **各个操作符的作用**

x.y 操作符：访问外层名称空间中的子名称空间，访问名称空间中的类型，访问类型中的静态成员，访问对象的成员。

```c#
//System.IO.File.Create("D:\\HelloWorld.text");
Form myForm = new Form();
myForm.Text = "Hello,World!";
myForm.ShowDialog();
```

f(x) 操作符：方法调用

a[x] 操作符：元素访问操作符

new 操作符：在内存中构造一个类型的实例，并且立刻调用这个实例的实例构造器（或初始化器），如果在new操作符有赋值符号时，会把该实例的内存地址赋给该变量。

在C#中，并不是构造类型时都需要使用new操作符，c#会在常用的类型构建实例时省去new操作符。

new操作符也可用于方法的继承和派生（让子类隐藏父类）

typeof 操作符：查看一个类型的内部结构 

enum 枚举类型

default 操作符：获取一个类型的默认值

checked 操作符：用于检测程序运行时有没有类型溢出

unchecked 操作符：不检测程序运行时有没有类型溢出

checked和unchecked不仅有操作符用法，还有上下文用法

delegate 操作符：一般用于委托，在当作操作符时是用来声明匿名方法，该方法是只执行一次

委托——代码演示：

```c#
namespace OperatorsExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //委托
            Calculator c = new Calculator();
            Action myAction = new Action(c.PrintHello);
            myAction();

        }
    }

    class Calculator
    {
     
        public void PrintHello()
        {
            Console.WriteLine("Hello");
        }
    }
}


结果：Hello
```



default类型

![image-20240916203127566](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240916203127566.png)

var关键字 功能 帮忙声明隐式声明变量

```
namespace OperatorsExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //Dictionary类里的value值类型是Student
            Dictionary<string,Student> stuDic = new Dictionary<string, Student>();
            for(int i = 1;i<= 100; i++)
            {
                Student stu = new Student();
                stu.Name = "s_" + i.ToString();
                stu.Score = 100 + i;
                stuDic.Add(stu.Name, stu);
            }

            Student number6 = stuDic["s_6"];
            Console.WriteLine(number6.Score);

        }
    }


    class Student
    {
        public string Name;
        public int Score;
    }
}

```

new 操作符：在内存中构造一个类型的实例，并且立刻调用这个实例的实例构造器（或初始化器），如果在new操作符有赋值符号时，会把该实例的内存地址赋给该变量。

new操作符和var关键字的用法：是为匿名类型创建对象，用隐式类型变量来引用实例

```C#
namespace OperatorsExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Form myForm = new Form() { Text = "Hello" };
            //是为匿名类型创建对象，用隐式类型变量来引用实例
            var person = new { Name = "Qi", Age = 21 };
            Console.WriteLine(person.Name);
            Console.WriteLine(person.Age);
            Console.WriteLine(person.GetType().Name);

        }
    }
}
```

拓展类与类之间的继承

```c#
namespace OperatorsExample
{
     class Program
    {
        static void Main(string[] args)
        {
            Student stu = new Student();
            stu.Report();
            CsStudent csStu = new CsStudent();
            csStu.Report();
        }
    }

    class Student
    {
        public void Report()
        {
            Console.WriteLine("I'm a student");
        }
    }

    class CsStudent : Student
    {
        //重写 子类隐藏父类
        new public void Report()
        {
            Console.WriteLine("I'm CS student");
        }
    }
}

```





![image-20240917205144654](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240917205144654.png)

检查异常

![image-20240917205317279](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240917205317279.png)



delegate 操作符：一般用于委托，在当作操作符时是用来声明匿名方法，该方法是只执行一次

![image-20240918142917608](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918142917608.png)

改进为

![image-20240918143100492](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918143100492.png)



->操作符

![image-20240918144338378](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918144338378.png)



![image-20240918144910394](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918144910394.png)





下去自己练习

![image-20240918150238104](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918150238104.png)



类型转换

![image-20240918223841804](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918223841804.png)

sizeof 操作符：用于查询结构体类型的字节长度，其也可以检测自定义的结构体类型的字节长度，但要放在不安全的上下文中。

& 操作符：取地址操作符，取某个对象的地址。

* 操作符：找到指针所指向的变量

~ 操作符：取反操作符。

- 操作符：取反操作后+1。

(T)x 操作符：强制转换操作符

<< >> 操作符：<< 操作符补进来的都是0，>>操作符正数补进来的是0，负数补进来的是1
is 操作符：用于检测一个对象是不是某一类型

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;
 
namespace ConsoleApp1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu = new Student();
            bool res = stu is Student;
            Console.WriteLine(res);//True
 
            Student stu2 = null;
            res = stu2 is Student;
            Console.WriteLine(res);//false
 
            res = stu is Animal;
            Console.WriteLine(res);//True
 
            Human human = new Human();
            res = human is Student;
            Console.WriteLine(res);//False
        }
    }
 
    class Animal
    {
        public void Eat()
        {
            Console.WriteLine("Eating...");
        }
    }
 
    class Human : Animal
    {
        public void Think()
        {
            Console.WriteLine("Thinking...");
        }
    }
 
    class Student:Human
    {
        public void Study()
        {
            Console.WriteLine("I want to play");
        }
    }
 }
```



as 操作符（强制类型转化）

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;
 
namespace ConsoleApp1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            object o = new Student();
            Student stu = o as Student;//如果o是Student类型，那么就把该地址赋值给stu，否则赋值null；
            if(stu!=null)
            {
                stu.Study();
            }
        }
    }
 
    class Animal
    {
        public void Eat()
        {
            Console.WriteLine("Eating...");
        }
    }
 
    class Human : Animal
    {
        public void Think()
        {
            Console.WriteLine("Thinking...");
        }
    }
 
    class Student:Human
    {
        public void Study()
        {
            Console.WriteLine("I want to play");
        }
    }
 }
```

?? 操作符：通常情况下值类型不能赋值null，但Nullable<数据类型>声明的变量可以，或者在值类型后加一个? 。

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;
 
namespace ConsoleApp1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Nullable<int> x = null;
            int? y = null;
            Console.WriteLine(x);
            Console.WriteLine(x.HasValue);
 
            x = x ?? 1;//如果x是null值，那就把x赋值成1；
            Console.WriteLine(x);
 
 
        }
    }
 
 }
```

?: 操作符：if，else的简写

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;
 
namespace ConsoleApp1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int x = 80;
            string s = String.Empty;
            s = x >= 60 ? "Pass" : "False";//如果真就返回:前面的值，否则返回:后面的值
            Console.WriteLine(s);
 
        }
    }
 
 }
```



## 类型转换

![image-20240918224028553](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918224028553.png)



### 隐式类型转换

代码演示：

```c#
namespace ConversionExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int x = int.MaxValue;
            //小往大转就隐式
            long y = x;
            Console.WriteLine(y);
        }
    }
}
```

结果

```c#
2147483647
请按任意键继续. . .
```

![image-20240918225344122](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240918225344122.png)

### 子类向父类转换

```c#
namespace ConversionExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Teacher t = new Teacher();
            Human h = t;
            Animal a = h;
            a.Eat();
        }
    }

    class Animal
    {
        public void Eat()
        {
            Console.WriteLine("Eating...");
        }
    }

    class Human : Animal
    {
        public void Think()
        {
            Console.WriteLine("Who I am?");
        }
    }

    class Teacher : Human
    {
        public void Teach()
        {
            Console.WriteLine("I teach programming");
        }
    }
}

```

### 显示类型转换

显示类型转换就是在转换对象中构造一个目标类型对象的构造器

```c#
namespace ConversionExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Stone stone = new Stone();
            stone.Age = 5000;
            //隐式类型转换  Monkey wukongSun = stone;
            Monkey wukongSun = (Monkey)stone;
            Console.WriteLine(wukongSun.Age);
           
        }
    }

    class Stone
    {
        public int Age;
        //隐式类型转换 public static implicit operator Monkey(Stone stone)
        
        //显示类型转换
        public static explicit operator Monkey(Stone stone)
        {
            Monkey m = new Monkey();
            m.Age = stone.Age/500;
            return m;
        }
    }

    class Monkey 
    {
        public int Age;
    }

}

```

tips：将explici改成implicit后就能把显式类型转变成隐式类型。



![image-20240919173709023](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919173709023.png)

# 表达式

表达式是一种专门求值的语法实体，可以有一个或多个操作数和零个或多个操作符组成

![image-20240919202323138](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919202323138.png)



![image-20240919203925716](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919203925716.png)

访问事件 An event access

![image-20240919205833804](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919205833804.png)



索引访问表达式

![image-20240919210510686](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919210510686.png)



# 语句

![image-20240919211832656](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919211832656.png)

c#语句的定义

![image-20240919214540109](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919214540109.png)

参考c#定义文档

虚线以上需要熟练使用

![image-20240919221615631](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240919221615631.png)

const常量的值是不能够被改变的



# 字段

![image-20240920093245877](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240920093245877.png)



C语言参考

![image-20240920093739972](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240920093739972.png)



代码小记：

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace DataMemberExample
{
     class Program
    {
        static void Main(string[] args)
        {
            List<Student> stuList = new List<Student>();
            for(int i = 0; i < 100; i++)
            {
                Student stu = new Student();
                stu.Age = 24;
                stu.Score = i;
                stuList.Add(stu);

            }

            int totalAge = 0;
            int totalScore = 0; 
            foreach(var stu in stuList)//在一组中一个一个迭代
            {
                totalAge += stu.Age;
                totalScore += stu.Score;
            }

            Student.AverageAge = totalAge / Student.Amount;
            Student.AverageScore = totalScore / Student.Amount; 

            Student.ReportAmount();
            Student.ReportAverageAge();
            Student.ReportAverageScore();
            
        }
    }

    class Student
    {
        public int Age;//实例字段，创建的对象可用的成员变量
        public int Score;
        public readonly int id=100;//声明一个动态只读字段，只有一次机会对其赋值，就是在它的构造器中
        //如果在创建的对象中没有对id进行赋值，那么id的值就是100，否则就是在构造器中赋予的值
        public static readonly Color WColor = new Color() { red = 0, blue = 0, green = 0 };//声明一个静态只读字段


        public static int AverageAge;//静态字段，仅类可用
        public static int AverageScore;
        public  static int Amount;

        public Student()
        {
            Student.Amount++;
        }

        public static void ReportAmount()
        {
            Console.WriteLine(Student.Amount);
        }

        public static void ReportAverageAge()
        {
            Console.WriteLine(Student.AverageAge);
        }

        public static void ReportAverageScore()
        {
            Console.WriteLine(Student.AverageScore);
        }
    }
}

```



静态只读字段也不能被复制  功能只能被实例保存

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Xml.Schema;

namespace DataMemberExample
{
     class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(Brush.DefaultColor.Red);
            Console.WriteLine(Brush.DefaultColor.Green);
            Console.WriteLine(Brush.DefaultColor.Blue);
            //静态初始化之后不能被赋值
            //Brush.DefaultColor = new Color() { Red = 0, Green = 0, Blue = 0 };

        }
    }

    struct Color
    {
        public int Red;
        public int Green;
        public int Blue;
    }

    class Brush
    {
        //public static readonly Color DefaultColor = new Color() { Red = 0, Green = 0, Blue = 0 };
        //静态构造函数里静态构造器
        public static readonly Color DefaultColor;
        static Brush()
        {
            Brush.DefaultColor = new Color() { Red = 0, Green = 0, Blue = 0 };
        }



    }
}

```

实例字段的初始化时机实在创建一个对象的时候进行，静态字段的初始化时机实在运行环境加载该数据类型的时候，并且静态字段的初始化执行一次，即第一次被加载的时候。



## set与get方法

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace PropertyExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu1 = new Student();
            stu1.Age = 20;

            Student stu2 = new Student();
            stu2.Age = 20;

            Student stu3 = new Student();
            stu3.Age = 200;

            int avgAge = (stu1.Age + stu2.Age + stu3.Age) / 3;
            Console.WriteLine(avgAge);

        }
    }

    class Student
    {
        public int Age;
    }
}

```

改进之后的方法 set与get方法

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace PropertyExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu1 = new Student();
            stu1.SetAge(20);

            Student stu2 = new Student();
            stu2.SetAge(20);

            Student stu3 = new Student();
            stu3.SetAge(80);

            int avgAge = (stu1.GetAge() + stu2.GetAge() + stu3.GetAge()) / 3;
            Console.WriteLine(avgAge);

        }
    }

    class Student
    {
        //public int Age;
        private int age;
        
        public int GetAge()
        {
            return this.age;
        }

        public void SetAge(int value)
        {
            if(value >= 0 && value <= 120)
            {
                this.age = value;
            }
            else
            {
                throw new Exception("Age value has error.");
            }
        }
    }
}

```

演化

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace PropertyExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Student stu1 = new Student();
                stu1.Age = 20;

                Student stu2 = new Student();
                stu2.Age = 20;

                Student stu3 = new Student();
                stu3.Age = 20;

                int avgAge = (stu1.Age + stu2.Age + stu3.Age) / 3;
                Console.WriteLine(avgAge);
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            

        }
    }

    class Student 
    {
        //public int Age;
        private int age;

        public int Age
        {
            get
            {
                return this.age;
            }

            set
            {
                if (value >= 0 && value <= 120)
                {
                    this.age = value;
                }
                else
                {
                    throw new Exception("Age value has error");
                }
            }
        }
        
        public int GetAge()
        {
            return this.age;
        }

        public void SetAge(int value)
        {
            if(value >= 0 && value <= 120)
            {
                this.age = value;    
            }
            else
            {
                throw new Exception("Age value has error.");
            }
        }
    }
}

```

# 属性

## 什么是属性

![image-20240921100947897](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921100947897.png)

## 属性的声明

### 静态属性

```
静态属性（Static Property）是属于类本身的属性，而不是类的实例。可以通过类名直接访问静态属性，而不需要创建类的实例。静态属性在整个应用程序中只有一份副本，可以用于存储类级别的信息。
```

### 实例属性

```
实例属性（Instance Property）是指属于类的实例（对象）的属性。每个类的实例都有自己的一组属性值，这些属性值可以在实例化对象后进行访问和修改。实例属性与静态属性不同，静态属性属于类本身，而实例属性属于类的实例。
```

### 只读属性

```
只读属性（Read-only Property）是指只提供了 Get 访问器的属性，即只能读取属性值，不能修改。只读属性在初始化后不能再被修改，可以用于提供对象的只读视图。
```

属性也是一种语法

![image-20240921101322023](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921101322023.png)

声明属性时 快捷键：propfull 再按两下tab键

```c#
 //声明属性时 快捷键：propfull 再按两下tab键
 private int myVar;

 public int MyProperty
 {
     get { return myVar; }
     set { myVar = value; }
 }
```

### 静态属性报出异常

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace PropertyExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Student.Amount = -100;//抛异常
                Console.WriteLine(Student.Amount);
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }

    class Student 
    {
        //public int Age;
        private int age;

        public int Age//属性（实例属性）
        {
            get//用于返回成员变量的值
            {
                return this.age;
            }

            set//用于设置成员变量的值
            {
                if (value >= 0 && value <= 120)//上下文关键词，value就是传进来的值，不需要声明
                {
                    this.age = value;
                }
                else
                {
                    throw new Exception("Age value has error");
                }
            }
        }

        private static int amount;

        public static int Amount//静态属性
        {
            get { return  amount; }
            set {
                if (value >= 0)
                {
                    Student.amount = value;
                }
                else
                {
                    throw new Exception("Amount has error");
                }
            }
      }

    }
}

结果：
Amount has error
请按任意键继续. . .
```

### 动态计算值的属性

#### 主动计算canwork的值

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace PropertyExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Student stu1 = new Student();
                stu1.Age = 16;
                Console.WriteLine(stu1.CanWork);
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }

        }
    }

    class Student 
    {

        private int age;
        public int Age
        {
            get { return age; }
            set 
            { age = value;  
            }
        }

        private bool canWork;

        public bool CanWork //动态计算值的属性，并且是主动计算这个值
        {
            get
            {
                if (this.age >= 18)
                {
                    return true;
                }
                else
                {
                    return false;
                }
            }
            
        }
```

#### 被动

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace PropertyExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Student stu1 = new Student();
                stu1.Age = 12;
                Console.WriteLine(stu1.CanWork);
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }

        }
    }

    class Student 
    {

        private int age;
        public int Age
        {
            get { return age; }
            set 
            { age = value;
                //被动引用时
                this.CalcumateCanWork();
            }
        }

        private bool canWork;

        public bool CanWork //动态计算值的属性，并且是主动计算这个值
        {
            get
            {
                return canWork;
            }
            
        }
        

        //以下是被动计算
        private void CalcumateCanWork()
        {
            if (this.age >= 16)
            {
                this.canWork=true;
            }
            else
            {
                this.canWork=false;
            }
        }



    }
}
   
```

# 索引器

![image-20240921155136588](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921155136588.png)

## 什么是索引器

```
在C#中，索引器（Indexer）是一种特殊的属性，允许类的实例像数组一样通过索引来访问和设置对象的元素。通过索引器，可以为类提供类似数组的访问方式，使得对象可以像集合一样进行索引访问。

索引器通常用于实现集合类或类似集合的数据结构，使得可以通过类似数组的语法来访问对象的元素。索引器可以定义多个重载版本，每个版本可以接受不同数量或类型的参数，以便支持不同的索引方式。

```

相关代码

```c#
public class MyCollection
{
    private string[] data = new string[3];
 
    // 索引器的定义
    public string this[int index]
    {
        get { return data[index]; }
        set { data[index] = value; }
    }
}
 
class Program
{
    static void Main()
    {
        MyCollection collection = new MyCollection();
 
        // 设置索引器的值
        collection[0] = "Item 1";
        collection[1] = "Item 2";
        collection[2] = "Item 3";
 
        // 获取索引器的值并输出
        Console.WriteLine(collection[0]);
        Console.WriteLine(collection[1]);
        Console.WriteLine(collection[2]);
    }
}
```

代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace IndexerExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu = new Student();
            stu["Math"] = 90;
            var mathScore = stu["Math"];
            Console.WriteLine(mathScore);
        }
    }


    //索引器声明
    class Student
    {
        private Dictionary<string, int> scoreDictionary = new Dictionary<string, int>();

        public int? this[String subject]
        {
            get
            {
                if (this.scoreDictionary.ContainsKey(subject))
                {
                    return this.scoreDictionary[subject];
                }
                else
                {
                    return null;
                }
            }
            set
            {
                if(value.HasValue == false)
                {
                    throw new Exception("Score cannot be null.");
                }

                if (this.scoreDictionary.ContainsKey(subject))
                {
                    this.scoreDictionary[subject] = value.Value;
                }
                else
                {
                    this.scoreDictionary.Add(subject, value.Value);
                }
            }
        }


    }
}

```

# 常量

![image-20240921163910628](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921163910628.png)

# 参数

## 传值参数

![image-20240921173311574](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921173311574.png)

### 传值参数（值类型）

声明时不带修饰符的形参是值形参。一个值形参对应于一个局部变量，只是它的初始值来自该方法调用所提供的相应实参。

当形参是值形参时，方法调用中的对应实参必须是表达式，并且它的类型可以隐式转换（第 ‎6.1 节）为形参的类型。

允许方法将新值赋给值参数。这样的赋值只影响由该值形参表示的局部存储位置，而不会影响在方法调用时由调用方给出的实参。



代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //实例方法，先创建方法的实例
            Student stu = new Student();
            int y = 100;
            stu.AddOne(y);
            Console.WriteLine(y);
        }
    }

    //方法声明
    class Student
    {
        public void AddOne(int x)//x参数是传值参数，传进来的值会在方法体内部有一个副本，改变的是副本的值，并不会影响方法体外的值 
        {
            x = x + 1;
            Console.WriteLine(x);
        }
    }
}

```



### 传值参数（引用类型）

创建新对象

![image-20240921190537633](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921190537633.png)

不创建对象

![image-20240921215912954](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921215912954.png)

代码演示：

创建对象

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu = new Student() { Name = "Tim" };
            SomeMethod(stu);//打印出来是Tom  因为参数传进来时，SomeMethod方法给Name赋了一个新值，所以打印出来的是Tom
            Console.WriteLine(stu.Name);//这个是方法外部变量所引用的实例没有变

        }

        //生成一个静态声明的方法
        static void SomeMethod(Student stu)
        {
            stu = new Student() { Name = "Tom" };
            Console.WriteLine(stu.Name);
        }
    }

    //方法声明
    class Student
    {
        public string  Name { get; set; }//简化声明的属性
    }
}

```

不创建对象

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu = new Student() { Name = "Tim" };
            UpdateObject(stu);
            Console.WriteLine("HashCode={0},Name={1}",stu.GetHashCode(),stu.Name);
        }

        //生成一个静态声明的方法
        static void UpdateObject(Student stu)
        {
            stu.Name = "Tom";//副作用，side-effect
            Console.WriteLine("HashCode={0},Name={1}",stu.GetHashCode(),stu.Name);
        }
    }

    //方法声明
    class Student
    {
        public string  Name { get; set; }//简化声明的属性
    }
}

```

结果

```c#
HashCode=46104728,Name=Tom
HashCode=46104728,Name=Tom
请按任意键继续. . .
```



### 传值参数的代码解释

## 引用参数

![image-20240921221934247](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921221934247.png)

### 引用参数（值类型）

![image-20240921225017228](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921225017228.png)

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int y = 1;
            IWantSideEffect(ref y);
            Console.WriteLine(y);
            
        }

        static void IWantSideEffect(ref int x)
        {
            x = x + 100;
        }
    }
}

```



### 传值参数（引用类型）

创建新对象

![image-20240921225642067](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921225642067.png)

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Student outterStu = new Student() { Name = "Tim" };
            Console.WriteLine("HashCode={0},Name={1}", outterStu.GetHashCode(), outterStu.Name);
            Console.WriteLine("------------------------------");
            IWantSideEffect(ref outterStu);
            //方法体外部的变量HashCode的值和内部的属性
            Console.WriteLine("HashCode={0},Name={1}", outterStu.GetHashCode(), outterStu.Name);


        }

        static void IWantSideEffect(ref Student stu)
        {
            stu = new Student() { Name = "Tom" };
            Console.WriteLine("HashCode={0},Name={1}",stu.GetHashCode(),stu.Name);
        }
        
    }

    class Student
    {
        public string Name { get; set; }
    }

    
}

```

不创建新对象

![image-20240921231902200](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921231902200.png)

### 引用参数的代码解释

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //对象的HashCode自始至终一直都没有改变，只是对值发生了改变
            Student outterStu = new Student() { Name = "Tim" };
            Console.WriteLine("HashCode={0},Name={1}", outterStu.GetHashCode(), outterStu.Name);
            Console.WriteLine("------------------------------");
            //SomeSideEffect(ref outterStu);
            //和值参数进行对比--值参数
            SomeSideEffect(outterStu);
            //方法体外部的变量HashCode的值和内部的属性
            Console.WriteLine("HashCode={0},Name={1}", outterStu.GetHashCode(), outterStu.Name);
        }

        //值参数的形参stu和实参outterStu所指向的地址是不同的，但他们所指向的地址中存储着相同类型的地址，即实例在堆内存中的地址.
        //引用参数的形参stu和实参outterStu多指向的是同一个地址，并且他们都存储着对象在堆内存中的地址.
        
        //static void SomeSideEffect(ref Student stu)
        //值参数
        static void SomeSideEffect(Student stu)
        {
            stu.Name = "Tom";
            Console.WriteLine("HashCode={0},Name={1}",stu.GetHashCode(),stu.Name);
        }
        
    }

    class Student
    {
        public string Name { get; set; }
    }
}

```

## 输出形参

有副作用的。。。。

```
用 out 修饰符声明的形参是输出形参。类似于引用形参，输出形参不创建新的存储位置。相反，输出形参表示的存储位置恰是在该方法调用中作为实参给出的那个变量所表示的存储位置。
当形参为输出形参时，方法调用中的相应实参必须由关键字 out 并后接一个与形参类型相同的 variable-reference（第 ‎5.3.3 节）组成。变量在可以作为输出形参传递之前不一定需要明确赋值，但是在将变量作为输出形参传递的调用之后，该变量被认为是明确赋值的。
在方法内部，与局部变量相同，输出形参最初被认为是未赋值的，因而必须在使用它的值之前明确赋值。
在方法返回之前，该方法的每个输出形参都必须明确赋值（在方法体之内必须要有明确的赋值）。
声明为分部方法（第 ‎10.2.7 节）或迭代器（第 ‎10.14 节）的方法不能有输出形参。

```

![image-20240921233508053](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921233508053.png)

### 输出参数（值类型）

![image-20240921233858370](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240921233858370.png)



代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            Console.WriteLine("Please input first number.");
            string arg1 = Console.ReadLine();
            double x = 0;
            //准备bool类型的变量来接收返回值
            bool b1 = double.TryParse(arg1, out x);//tryparse解析文本是否为double类型，即直接把内容输出给x？，并返回一个bool值
            if (b1 == false)
            {
                Console.WriteLine("input error!");
                return;//return作用：上面如果输入错误，就直接结束不用执行下面一步了
            }
            
            Console.WriteLine("Please input second number");
            string arg2 = Console.ReadLine();
            double y = 0;
            bool b2 = double.TryParse(arg2, out y);
            if(b2 == false)
            {
                Console.WriteLine("input error!");
                return;
            }
            //两次解析都结束了，说明拿到了两个double的值。
            double z = x + y;
            Console.WriteLine("{0}+{1}={2}",x,y,z);
        }

    }
}

```

值类型输出参数

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            double x = 0;
            //数字是正确的，换成字符串
            //bool b = DoubleParser.TryParse("789", out x);//结果790
            bool b = DoubleParser.TryParse("ABC", out x);//0
            if (b == true)
            {
                Console.WriteLine(x + 1);
            }
            else
            {
                Console.WriteLine(x);
            }
        }

    }

    class DoubleParser
    {
        public static bool TryParse(string input,out double result)
        {
            try
            {
                result = double.Parse(input);
                return true;
            }
            catch
            {
                result = 0;
                return false;
            }
        }
    }
}

```

### 输出参数（引用类型）

![image-20240922125345256](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922125345256.png)

引用类型输出参数

代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            Student stu = null;
            bool b = StudentFactory.Create("Tim", 34, out stu);
            if(b == true)
            {
                Console.WriteLine("Student {0}, age is {1}",stu.Name,stu.Age);
            }

        }

    }

    class Student
    {
        public int Age { get; set; }
        public string Name { get; set; }
    }

    //工厂模式
    class StudentFactory
    {
        public static bool Create(string stuName, int stuAge, out Student result)
        {
            result = null;
            if (string.IsNullOrEmpty(stuName))
            {
                return false;
            }
            if (stuAge < 20 || stuAge > 80)
            {
                return false;
            }
            //result接收
            result = new Student() { Name = stuName, Age = stuAge };
            return true;
        }
    }
    }

```

## 数组参数

![image-20240922141556974](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922141556974.png)

代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            //为了调用CalculateSum方法，需要先声明一个数组，显得语法啰嗦，可以加
            int[] myIntArray = new int[] { 1, 2, 3 };
            int result = CalculateSum(myIntArray);
            Console.WriteLine(result);//6
            //一下操作也可以，但需要再方法中加一个修饰词params
            int result1 = CalculateSum1(2,3,4);
            Console.WriteLine(result1);//9
            
            //String类型有个实例方法split()可以分割字符串
            string str = "Tim;Tom,Yaqi.Lisa";
            string[] result = str.Split(';',',','.');
            foreach(var name in result)//列表无法精确的只能通过遍历，并且数组的空间是固定的
            {
                Console.WriteLine(name);
            }

        }

        static int CalculateSum(int[] intArray)
        {
            int sum = 0;
            foreach(var item in intArray)
            {
                sum += item;

            }
            return sum;
        }

        static int CalculateSum1(params int[] intArray)
        {
            int sum = 0;
            foreach(var item in intArray)
            {
                sum += item;
            }
            return sum;
        }
    }
    }
```

**String类型有个实例方法split()可以分割字符串**

## 具名参数

- 参数的位子不再受约束

两个优点

1.   提高了代码的可读性
2.   当我们为参数加上名字之后，参数的位置就不会受参数的顺序的约束了,即参数的位置不受参数顺序的约束

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            //不具名参数
            PrintInfo("Tim", 34);

            //具名参数
            //两个优点
            //1.提高了代码的可读性
            //2.当我们为参数加上名字之后，参数的位置就不会受参数的顺序的约束了,即参数的位置不受参数顺序的约束
            PrintInfo(age: 34, name: "tim");
            PrintInfo(name: "Tim", age: 34);

        }

        static void PrintInfo(string name,int age)
        {
            Console.WriteLine("Hello {0}, you are {1}",name,age);
        }

    }
    }

```



## 可选参数

- 参数因为具有默认值而变得“可选”
- 不推荐使用可选参数

代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            PrintInfo();

        }

        static void PrintInfo(string name="Tim",int age=34)//参数在方法中已声明，具有默认值
        {
            Console.WriteLine("Hello {0}, you are {1}",name,age);
        }

    }
}
```

拓展方法(this参数)

![image-20240922145721441](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922145721441.png)

代码演示：

```C#
using System;
using System.Collections.Generic;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            double x = 3.14159;
            double y = Math.Round(x,4);//3.1416
            Console.WriteLine(y);
            double z = x.Round(4);
            Console.WriteLine(z);//3.1416
        }
    }
    static class DoubleExtension
    {
        public static double Round(this double input, int digits)
        {
            double result = Math.Round(input, digits);
            return result;
        }
    }
}

```

LINQ方法

代码演示：

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data.SqlTypes;
using System.Linq;
using System.Runtime.InteropServices;
using System.Security.Permissions;
using System.Text;
using System.Threading.Tasks;

namespace ParametersExample
{
    internal class Program
    {
        static void Main(String[] args)
        {
            List<int> myList = new List<int>() { 11, 12, 13, 14, 15, };

            bool result = myList.All(i => i > 10);//LINQ方法--拓展方法--All--属于Enumerable静态类
            Console.WriteLine(result);
        }

        static bool AllGreaterThanTen(List<int> list)//集合中所有值大于10
        {
            foreach(var item in list)
            {
                if (item <= 10)
                {
                    return false;
                }
            }
            return true;
        }
    }
    
}


```

![image-20240922152656471](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922152656471.png)



小结：

![image-20240922152809878](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922152809878.png)



# 委托

## 什么是委托

```
在C#中，委托（Delegate）是一种类型，用于表示对一个或多个方法的引用。委托可以看作是函数指针的类型安全版本，它允许将方法作为参数传递、存储和调用。委托可以用于实现事件处理、回调函数、多播委托等功能。

委托的定义类似于方法的签名，它指定了方法的返回类型和参数列表。通过委托，可以将一个方法绑定到委托实例上，然后通过委托实例调用该方法。委托可以是单播的（绑定一个方法）或多播的（绑定多个方法）

```

![image-20240922174025965](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922174025965.png)

### 代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Policy;
using System.Text;
using System.Threading.Tasks;

namespace DelegateExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Calculator cal = new Calculator();
            Action act = new Action(cal.Report);
        }
    }

    class Calculator
    {
        public void Report()
        {
            Console.WriteLine("I have 3 methods");
        }

        public int Add(int a,int b)
        {
            int result = a + b;
            return result;
        }

        public int Sub(int a,int b)
        {
            int result = a - b;
            return result;
        }
    }
}

```

代码

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Policy;
using System.Text;
using System.Threading.Tasks;

namespace DelegateExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Calculator cal = new Calculator();
            Action act = new Action(cal.Report);
            cal.Report();
            act.Invoke();
            act();

            Func<int, int, int> func1 = new Func<int, int, int>(cal.Add);
            Func<int, int, int> func2 = new Func<int, int, int>(cal.Sub);

            int x = 100;
            int y = 200;
            int z = 0;

            //z = func1.Invoke(x, y);//间接调用
            z = func1(x, y);//使用委托时都有指针的写法
            Console.WriteLine(z);
            //z = func2.Invoke(x, y);//间接调用
            z = func2(x, y);
            Console.WriteLine(z);
        }
    }

    class Calculator
    {
        public void Report()
        {
            Console.WriteLine("I have 3 methods");
        }

        public int Add(int a,int b)
        {
            int result = a + b;
            return result;
        }

        public int Sub(int a,int b)
        {
            int result = a - b;
            return result;
        }
    }
}
   
```

## 委托的声明（自定义委托）

![image-20240922221502879](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922221502879.png)

### 代码演示：

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Policy;
using System.Text;
using System.Threading.Tasks;

namespace DelegateExample
{
    public delegate double Calc(double a, double b);//声明自定义委托类型
                                                   //自定义委托类型,可以放在类里，也可以放在类外
                                                   //放在类外是所有的类都可以使用
                                                   //放在类里是只能这个类使用

    internal class Program
    {
        static void Main(string[] args)
        {
            Calculator cal = new Calculator();
            Calc calc1 = new Calc(cal.Add);
            Calc calc2 = new Calc(cal.Sub);
            Calc calc3 = new Calc(cal.Mul);
            Calc calc4 = new Calc(cal.Div);//通过委托间接调用方法

            double a = 100;
            double b = 200;
            double c = 0;

            //仿指针就把.Invoke 去掉
            c = calc1.Invoke(a, b);
            Console.WriteLine(c);
            c = calc2.Invoke(a, b);
            Console.WriteLine(c);
            c = calc3.Invoke(a, b);
            Console.WriteLine(c);
            c = calc4.Invoke(a, b);
            Console.WriteLine(c);

        }
    }

    class Calculator
    {
        

        public double  Add(double a, double b)
        {
            return a + b;
        }

        public double Sub(double a,double b)
        {
            return a-b;
        }

        public double Mul(double a, double b)
        {
            return a * b;
        }
        public double Div(double a, double b)
        {
            return a / b;
        }
    }
}

```

## 委托的一般使用

![image-20240922224045591](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240922224045591.png)



![image-20240923092847236](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240923092847236.png)



```
模板方法： 模板方法是一种设计模式，其中定义了一个算法的框架，而具体步骤的实现由子类来完成。在C#中，可以使用委托来实现模板方法。父类定义一个包含委托作为参数的模板方法，子类实现具体的方法并将其作为委托传递给父类的模板方法。这样可以在父类中定义算法的框架，而具体步骤的实现由子类决定。

回调方法： 回调方法是一种机制，其中一个方法将另一个方法作为参数传递，以便在适当的时候调用该方法。在C#中，可以使用委托来实现回调方法。一个常见的应用是事件处理，其中事件触发时会调用注册的委托方法。通过回调方法，可以实现事件驱动的编程模型，让不同部分之间实现解耦合。

```

自己设置断点理解

### 模版方法：代码

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel.Design.Serialization;
using System.Linq;
using System.Security.Policy;
using System.Text;
using System.Threading.Tasks;

namespace DelegateExample
{
    public delegate double Calc(double a, double b);

    internal class Program
    {
        static void Main(string[] args)
        {
            ProductFactory productFactory = new ProductFactory();
            WrapFactory wrapFactory = new WrapFactory();

            Func<Product> func1 = new Func<Product>(productFactory.MakePizza);
            Func<Product> func2 = new Func<Product>(productFactory.MakeToyCar);

            Box box1 = wrapFactory.WrapProduct(func1);
            Box box2 = wrapFactory.WrapProduct(func2);

            Console.WriteLine(box1.Product.Name);
            Console.WriteLine(box2.Product.Name);
        }
    }

    class Product
    {
        public string Name { get; set; }
    }

    class Box
    {
        public Product Product { get; set; }
    }

    //工厂类--工厂类里有个装盒子的方法，返回值是（Box盒子），需要用到的参数是（返回值为Product的委托）
    class WrapFactory
    {
        public Box WrapProduct(Func<Product> getProduct)
        {
            Box box = new Box();
            Product product = getProduct.Invoke();//间接调用，没有通过方法直接调用，而是通过一个委托去调用
            box.Product = product;
            return box;
        }
    }

    //生产各种产品的工厂类
    class ProductFactory
    {
        public Product MakePizza()
        {
            Product product = new Product();
            product.Name = "Pizza";
            return product;
        }

        public Product MakeToyCar()
        {
            Product product = new Product();
            product.Name = "ToyCar";
            return product;
        }
    } 
}
```



### 用接口的方法写

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel.Design.Serialization;
using System.Linq;
using System.Security.Policy;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace DelegateExample
{
    public delegate double Calc(double a, double b);

    internal class Program
    {
        static void Main(string[] args)
        {
            IProductFactory pizzaFactory = new PizzaFactory();
            IProductFactory toycarFactory = new ToyCarFactory();
            
            WrapFactory wrapFactory = new WrapFactory();


            Box box1 = wrapFactory.WrapProduct(pizzaFactory);
            Box box2 = wrapFactory.WrapProduct(toycarFactory);

            Console.WriteLine(box1.Product.Name);
            Console.WriteLine(box2.Product.Name);
        }
    }

    //接口
    interface IProductFactory
    {
        Product Make();
    }

    class PizzaFactory:IProductFactory
    {
        public Product Make()
        {
            Product product = new Product();
            product.Name = "Pizza";
            return product;
        }
    }

    class ToyCarFactory : IProductFactory
    {
        public Product Make()
        {
            Product product = new Product();
            product.Name = "ToyCar";
            return product;
        }
    }

    class Product
    {
        public string Name { get; set; }
    }

    class Box
    {
        public Product Product { get; set; }
    }

    //工厂类--工厂类里有个装盒子的方法，返回值是（Box盒子），需要用到的参数是（返回值为Product的委托）
    class WrapFactory
    {
        public Box WrapProduct(IProductFactory productFactory)
        {
            Box box = new Box();
            Product product = productFactory.Make();//间接调用，没有通过方法直接调用，而是通过一个委托去调用
            box.Product = product;
            return box;
        }
    }
}

```

### 回调方法：

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel.Design.Serialization;
using System.Linq;
using System.Security.Policy;
using System.Text;
using System.Threading.Tasks;

namespace DelegateExample
{
    public delegate double Calc(double a, double b);

    internal class Program
    {
        static void Main(string[] args)
        {
            ProductFactory productFactory = new ProductFactory();
            WrapFactory wrapFactory = new WrapFactory();

            Func<Product> func1 = new Func<Product>(productFactory.MakePizza);
            Func<Product> func2 = new Func<Product>(productFactory.MakeToyCar);

            Logger logger = new Logger();
            Action<Product> log = new Action<Product>(logger.Log);

            Box box1 = wrapFactory.WrapProduct(func1,log);
            Box box2 = wrapFactory.WrapProduct(func2,log);

            Console.WriteLine(box1.Product.Name);
            Console.WriteLine(box2.Product.Name);
        }
    }

    //创建一个日志
    class Logger
    {
        public void Log(Product product)
        {
            Console.WriteLine("Product '{0}' created at {1}.price is{2}.",product.Name,DateTime.UtcNow,product.Price);
        }
    }

    class Product
    {
        public string Name { get; set; }
        public double Price { get; set; }
    }

    class Box
    {
        public Product Product { get; set; }
    }

    //工厂类--工厂类里有个装盒子的方法，返回值是（Box盒子），需要用到的参数是（返回值为Product的委托）
    class WrapFactory
    {
        public Box WrapProduct(Func<Product> getProduct, Action<Product> logCallback)
        {
            Box box = new Box();
            Product product = getProduct.Invoke();//间接调用，没有通过方法直接调用，而是通过一个委托去调用
            if (product.Price >= 50)//大于等于50的记录
            {
                logCallback(product);
            }
            box.Product = product;
            return box;
        }
    }

    //生产各种产品的工厂类
    class ProductFactory
    {
        public Product MakePizza()
        {
            Product product = new Product();
            product.Name = "Pizza";
            product.Price = 12;
            return product;
        }

        public Product MakeToyCar()
        {
            Product product = new Product();
            product.Name = "ToyCar";
            product.Price = 99;
            return product;
        }
    }  
}

```

滥用委托

![image-20240923103658414](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240923103658414.png)





![image-20240923103732653](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240923103732653.png)

## 委托的高级使用

![image-20240923103844571](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240923103844571.png)



### 多播委托：

```
多播委托是一种特殊类型的委托，它可以包含多个目标方法的引用。多播委托允许将多个方法绑定到同一个委托实例上，当该委托被调用时，所有绑定的方法都会被依次执行。多播委托使用加法运算符 '+' 来添加方法，使用减法运算符 '-' 来移除方法。
```



```c#
using System;
using System.Collections.Generic;
using System.ComponentModel.Design.Serialization;
using System.Linq;
using System.Security.Policy;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace DelegateExample
{
    public delegate double Calc(double a, double b);

    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu1 = new Student() { ID = 1, PenColor = ConsoleColor.Yellow };
            Student stu2 = new Student() { ID = 2, PenColor = ConsoleColor.Green };
            Student stu3 = new Student() { ID = 3, PenColor = ConsoleColor.Red };
            Action action1 = new Action(stu1.DoHomework);//一个委托封装一个方法的形式称为单播委托
            Action action2 = new Action(stu2.DoHomework);
            Action action3 = new Action(stu3.DoHomework);

            action1.Invoke();
            action2.Invoke();
            action3.Invoke();

            action1 += action2;// 用一个委托封装多个方法的形式称为多播委托
            action1 += action3;

            action1.Invoke();//结果相同


        }
    }

    class Student
    {
        public int ID { get; set; }
        public ConsoleColor PenColor { get; set; }
        
        public void DoHomework()
        {
            for(int i = 0; i < 5; i++)
            {
                Console.ForegroundColor = this.PenColor;
                Console.WriteLine("Student {0} doing homework {1} hour(s).", this.ID, i);
                Thread.Sleep(1000);
            }
        }
    }
}

```



### 同步调用

```
同步调用是指在调用方法时，程序会等待该方法执行完毕并返回结果后再继续执行后续代码。在同步调用中，调用方会阻塞等待被调用方法的完成，直到被调用方法返回结果后才能继续执行。
```



```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace MultiThreadExample
{
    public delegate double Calc(double a, double b);

    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu1 = new Student() { ID = 1, PenColor = ConsoleColor.Yellow };
            Student stu2 = new Student() { ID = 2, PenColor = ConsoleColor.Green };
            Student stu3 = new Student() { ID = 3, PenColor = ConsoleColor.Red };

            //同步调用--直接
            stu1.DoHomework();
            stu2.DoHomework();
            stu3.DoHomework();

            //第二种同步调用--典型的间接同步调用
            Action action1 = new Action(stu1.DoHomework);
            Action action2 = new Action(stu2.DoHomework);
            Action action3 = new Action(stu3.DoHomework);
            action1.Invoke();//单播委托
            action2.Invoke();
            action2.Invoke();

            action1 += action2;//多播
            action1 += action3;
            action1.Invoke();



            for (int i = 0; i < 10; i++)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("Main thread {0}", i);
                Thread.Sleep(1000);
            }
        }
    }

    class Student
    {
        public int ID { get; set; }
        public ConsoleColor PenColor { get; set; }

        public void DoHomework()
        {
            for (int i = 0; i < 5; i++)
            {
                Console.ForegroundColor = this.PenColor;
                Console.WriteLine("Student {0} doing homework {1} hour(s).", this.ID, i);
                Thread.Sleep(1000);
            }
        }
    }
}
```

### 异步调用

```
异步调用是指在调用方法时，程序不会等待被调用方法执行完毕，而是继续执行后续代码。被调用方法会在另一个线程或任务中执行，并在执行完毕后通知调用方或执行回调方法。异步调用可以提高程序的性能和响应性，特别适用于需要长时间执行的操作，如网络请求、文件读写等。
```



```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace MultiThreadExample
{
    public delegate double Calc(double a, double b);

    internal class Program
    {
        static void Main(string[] args)
        {
            Student stu1 = new Student() { ID = 1, PenColor = ConsoleColor.Yellow };
            Student stu2 = new Student() { ID = 2, PenColor = ConsoleColor.Green };
            Student stu3 = new Student() { ID = 3, PenColor = ConsoleColor.Red };

            Action action1 = new Action(stu1.DoHomework);
            Action action2 = new Action(stu2.DoHomework);
            Action action3 = new Action(stu3.DoHomework);

            action1.BeginInvoke(null, null);//隐式异步调用
            action2.BeginInvoke(null, null);
            action3.BeginInvoke(null, null);

            Console.WriteLine("---------------------------");

            Thread thread1 = new Thread(new ThreadStart(stu1.DoHomework));
            Thread thread2 = new Thread(new ThreadStart(stu2.DoHomework));
            Thread thread3 = new Thread(new ThreadStart(stu3.DoHomework));

            thread1.Start();
            thread2.Start();
            thread3.Start();//显式异步调用

            Console.WriteLine("------------------------------");

            Task task1 = new Task(new Action(stu1.DoHomework));
            Task task2 = new Task(new Action(stu2.DoHomework));
            Task task3 = new Task(new Action(stu3.DoHomework));
            task1.Start();
            task2.Start();
            task3.Start();//使用task的显式异步调用


            for (int i = 0; i < 10; i++)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("Main thread {0}", i);
                Thread.Sleep(1000);
            }
        }
    }

    class Student
    {
        public int ID { get; set; }
        public ConsoleColor PenColor { get; set; }

        public void DoHomework()
        {
            for (int i = 0; i < 5; i++)
            {
                Console.ForegroundColor = this.PenColor;
                Console.WriteLine("Student {0} doing homework {1} hour(s).", this.ID, i);
                Thread.Sleep(1000);
            }
        }
    }
}

```



事件

![image-20240923215530656](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240923215530656.png)

```
 在C#中，事件是一种特殊的委托，用于实现发布-订阅模式。事件提供了一种机制，使一个对象可以通知其他对象发生了特定的动作或状态变化，而其他对象可以订阅该事件以在事件发生时执行相应的操作。
```

![image-20240923222825642](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240923222825642.png)

事件的应用



![image-20240924100306118](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240924100306118.png)

vs里面  小方块--方法  小扳手--属性  小闪电--事件



一个事件有两个处理器的时候 --代码演示

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.CompilerServices;
using System.Text;
using System.Threading.Tasks;
using System.Timers;

namespace EventExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Timer timer = new Timer();
            timer.Interval = 1000;
            Boy boy = new Boy();
            Girl girl = new Girl();
            timer.Elapsed += boy.Action;//事件订阅者 += 操作符 +=操作符左边：事件   +=操作符右边：事件处理器
            timer.Elapsed += girl.Action;
            timer.Start();
            Console.ReadLine();
        }
    }

    class Boy
    {
        //alt+enter自动生成事件处理器--Action方法
        internal void Action(object sender, ElapsedEventArgs e)
        {
            Console.WriteLine("Jump!");
        }
    }

    class Girl
    {
        internal void Action(object sender, ElapsedEventArgs e)
        {
            Console.WriteLine("Sing!");
        }
    }
}

```

演示结果：

```
Jump!
Sing!
Jump!
Sing!
Jump!
Sing!
```



事件一星

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.CompilerServices;
using System.Text;
using System.Threading.Tasks;
using System.Timers;

using System.Windows.Forms;

namespace EventExample
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Form form = new Form();//事件拥有着
            Controller controller = new Controller(form);//事件的响应者
            form.ShowDialog();
        }
    }

    class Controller
    {
        private Form form;

        //构造器快捷键 ctor
        public Controller(Form form)
        {
            if(form != null)
            {
                this.form = form;
                this.form.Click += this.FormClicked;//this--代表Controller的实例 - - - 事件 - - 事件订阅

            }
        }

        private void FormClicked(object sender, EventArgs e)//事件的处理器
        {
            this.form.Text = DateTime.Now.ToString();
        }
    }

    
}

```



![image-20240925151107728](C:/Users/庞亚琪/AppData/Roaming/Typora/typora-user-images/image-20240925151107728.png)
