main方法是java应用程序，通过JVM直接启动运行的程序。

当一个类中有main方法，执行命令“java 类名”则会启动虚拟机执行该类中的main方法...

由于JVM在运行java应用程序时，会先调用main方法，调用时不实例化这个类的对象，而是直接通过类名调用所以需要限制为public static .

java中main方法，jvm限制不能有返回值，因此返回类型为void。

main方法有一个输入参数，类型为String[]，java规范，main方法必须有一个入参，类型必须为String[]，字符串数据名字，可以自己设定，根据习惯，这个字符串数据的名字一般和SUN java规范中main参数名保持一致，取名为args。


因此，main()方法定义必须是public static void main(String[] 数组名)

http://lavasoft.blog.51cto.com/62575/53263