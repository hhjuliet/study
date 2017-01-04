###一、概念

　　	Annontation是Java5开始引入的新特征。中文名称一般叫注解。它提供了一种安全的类似注释的机制，用来将任何的信息或元数据（metadata）与程序元素（类、方法、成员变量等）进行关联。

　　更通俗的意思是为程序的元素（类、方法、成员变量）加上更直观更明了的说明，这些说明信息是与程序的业务逻辑无关，并且是供指定的工具或框架使用的。

​	Annontation像一种修饰符一样，应用于包、类型、构造方法、方法、成员变量、参数及本地变量的声明语句中。



###二、原理

　　Annotation其实是一种接口。通过Java的反射机制相关的API来访问annotation信息。相关类（框架或工具中的类）根据这些信息来决定如何使用该程序元素或改变它们的行为。

　　annotation是不会影响程序代码的执行，无论annotation怎么变化，代码都始终如一地执行。

　　Java语言解释器在工作时会忽略这些annotation，因此在JVM	中这些annotation是“不起作用”的，只能通过配套的工具才能对这些annontaion类型的信息进行访问和处理。

　　Annotation与interface的异同：

　　1.	Annotation类型使用关键字@interface而不是interface。

　　这个关键字声明隐含了一个信息：它是继承了java.lang.annotation.Annotation接口，并非声明了一个interface

　　2.	Annotation类型、方法定义是独特的、受限制的。

　　Annotation 类型的方法必须声明为无参数、无异常抛出的。这些方法定义了annotation的成员：方法名成为了成员名，而方法返回值成为了成员的类型。而方法返回值类型必须为primitive类型、Class类型、枚举类型、annotation类型或者由前面类型之一作为元素的一维数组。方法的后面可以使用 default和一个默认数值来声明成员的默认值，null不能作为成员默认值，这与我们在非annotation类型中定义方法有很大不同。

　　Annotation类型和它的方法不能使用annotation类型的参数、成员不能是generic。只有返回值类型是Class的方法可以在annotation类型中使用generic，因为此方法能够用类转换将各种类型转换为Class。

　　3.	Annotation类型又与接口有着近似之处。

　　它们可以定义常量、静态成员类型（比如枚举类型定义）。Annotation类型也可以如接口一般被实现或者继承。



###三、应用场合

　　annotation一般作为一种辅助途径，应用在软件框架或工具中，在这些工具类中根据不同的 annontation注解信息采取不同的处理过程或改变相应程序元素（类、方法及成员变量等）的行为。

　　例如：Junit、Struts、Spring等流行工具框架中均广泛使用了annontion。使代码的灵活性大提高。



###四、常见标准的Annotation

　　从java5版本开始，自带了三种标准annontation类型,

　　**Override

　　java.lang.Override 是一个marker annotation类型，它被用作标注方法。它说明了被标注的方法重载了父类的方法，起到了断言的作用。如果我们使用了这种annotation在一个没有覆盖父类方法的方法时，java编译器将以一个编译错误来警示。

　　这个annotaton常常在我们试图覆盖父类方法而确又写错了方法名时加一个保障性的校验过程。

　　**Deprecated

　　Deprecated也是一种marker annotation。当一个类型或者类型成员使用@Deprecated修饰的话，编译器将不鼓励使用这个被标注的程序元素。所以使用这种修饰具有一定的 “延续性”：如果我们在代码中通过继承或者覆盖的方式使用了这个过时的类型或者成员，虽然继承或者覆盖后的类型或者成员并不是被声明为 @Deprecated，但编译器仍然要报警。

　　注意：@Deprecated这个annotation类型和javadoc中的 @deprecated这个tag是有区别的：前者是java编译器识别的，而后者是被javadoc工具所识别用来生成文档（包含程序成员为什么已经过时、它应当如何被禁止或者替代的描述）。

　　**SuppressWarnings

　　此注解能告诉Java编译器关闭对类、方法及成员变量的警告。

　　有时编译时会提出一些警告，对于这些警告有的隐藏着Bug，有的是无法避免的，对于某些不想看到的警告信息，可以通过这个注解来屏蔽。

　　SuppressWarning不是一个marker annotation。它有一个类型为String[]的成员，这个成员的值为被禁止的警告名。对于javac编译器来讲，被-Xlint选项有效的警告名也同样对@SuppressWarings有效，同时编译器忽略掉无法识别的警告名。

　　annotation语法允许在annotation名后跟括号，括号中是使用逗号分割的name=value对用于为annotation的成员赋值：

代码：

    @SuppressWarnings(value={"unchecked","fallthrough"})

    public void lintTrap() { /* sloppy method body omitted */ }

在这个例子中SuppressWarnings annotation类型只定义了一个单一的成员，所以只有一个简单的value={...}作为name=value对。又由于成员值是一个数组，故使用大括号来声明数组值。

注意：我们可以在下面的情况中缩写annotation：当annotation只有单一成员，并成员命名为"value="。这时可以省去"value="。比如将上面的SuppressWarnings annotation进行缩写：

代码：

    @SuppressWarnings({"unchecked","fallthrough"})

如果SuppressWarnings所声明的被禁止警告个数为一个时，可以省去大括号：

    @SuppressWarnings("unchecked")


###五、开发注解

​	在一般的开发中，只需要通过阅读相关的API文档来了解每个注解的配置参数的含义，并在代码中正确使用即可。在有些情况下，可能会需要开发自己的注解。这在库的开发中比较常见。注解的定义有点类似接口。下面的代码给出了一个简单的描述代码分工安排的注解。通过该注解可以在源代码中记录每个类或接口的分工和进度情况。

    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.TYPE)
    public @interface Assignment {
        String assignee();
        int effort();
        double finished() default 0;
    } 

@interface用来声明一个注解，其中的每一个方法实际上是声明了一个配置参数。方法的名称就是参数的名称，返回值类型就是参数的类型。可以通过default来声明参数的默认值。在这里可以看到@Retention和@Target这样的元注解，用来声明注解本身的行为。@Retention用来声明注解的保留策略，有CLASS、RUNTIME和SOURCE这三种，分别表示注解保存在类文件、JVM运行时刻和源代码中。只有当声明为RUNTIME的时候，才能够在运行时刻通过反射API来获取到注解的信息。@Target用来声明注解可以被添加在哪些类型的元素上，如类型、方法和域等。

元注解是指注解的注解。包括  @Retention @Target @Document @Inherited四种。



> @Retention: 定义注解的保留策略

1. @Retention: 定义注解的保留策略

2. @Retention(RetentionPolicy.SOURCE)   //注解仅存在于源码中，在class字节码文件中不

3. @Retention(RetentionPolicy.CLASS)     // 默认的保留策略，注解会在class字节码文件中存在，但运行时无法获得，

4. @Retention(RetentionPolicy.RUNTIME)  // 注解会在class字节码文件中存在，在运行时可以通过反射获取到，很多情况使用这种注解。

   ​

> @Target：定义注解的作用目标

1. @Target(ElementType.TYPE)   //接口、类、枚举、注解

2. @Target(ElementType.FIELD) //字段、枚举的常量

3. @Target(ElementType.METHOD) //方法

4. @Target(ElementType.PARAMETER) //方法参数

5. @Target(ElementType.CONSTRUCTOR)  //构造函数

6. @Target(ElementType.LOCAL_VARIABLE)//局部变量

7. @Target(ElementType.ANNOTATION_TYPE)//注解

8. @Target(ElementType.PACKAGE) ///包   

   ​

 由以上的源码可以知道，他的elementType 可以有多个，一个注解可以为类的，方法的，字段的等等

> @Document：说明该注解将被包含在javadoc中 

> @Inherited：说明子类可以继承父类中的该注解





转载

http://www.cnblogs.com/mandroid/archive/2011/07/18/2109829.html

http://www.infoq.com/cn/articles/cf-java-annotation

http://blog.csdn.net/yixiaogang109/article/details/7328466
