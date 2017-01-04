reflection用途
reflection通常用来检查同一系统中的其他代码。
例如，你有一个对象在java中是不知道类型的，你想要在它上面调用一个'doSomething'的方法，java的静态系统不支持这个，除非这个对象继承自一个已知的接口，但是使用反射机制，你的代码可以检视这个对象，并找出是否真的有方法叫做'doSomething'然后调用它如果你需要的话。

给出一个java中的代码实例：
```java
Method method = foo.getClass().getMethod("doSomething", null);
method.invoke(foo, null);
```



http://www.oracle.com/technetwork/articles/java/javareflection-1536171.html

reflection是一种java语言的特性，它允许一个执行的java程序去自我检测，并操纵程序内部的属性。例如，java可以去获取所有成员的名字并把他们展示出来。

这种能力通过自身去测试和操纵一个java类可能听起来一般，但是其他编程语言，是没有这个feature的，在Pascal，c或c++程序中获取在程序中定义的方法是不可能的。

一种可确认的使用reflection的机制是在javaBean中，软件组件可以通过一个创建工具被可视化的操纵，这个工具使用reflection机制使用反射机制去获取java组件的属性正如他们被动态加载。



java reflection能做的几件事情



1.查看目标方法有哪些函数

```java
 try {
          Class c = Class.forName("java.lang.String");
          Method m[] = c.getDeclaredMethods();
          for (int i = 0; i < m.length; i++)
          	System.out.println(m[i].toString());
   
    	  Class class1 = Class.forName("Proxy.Main");//这里一定要带上包名，即使是同一个包下的类
    	  for (int i = 0; i < m.length; i++)
          	System.out.println(m[i].toString());
 }catch (Throwable e) {
          System.err.println(e);
}
```

2.判断是否为实例

```java
try {
    Class class1 = Class.forName("Proxy.Main");
    boolean b1 = class1.isInstance(new Integer(37));
    System.out.println(b1);
    boolean b2 = class1.isInstance(new Main());
    System.out.println(b2);
}catch (Throwable e) {
    System.err.println(e);
}
```

3.找出一个类的方法、参数、异常

```java

public class Main {
  private int f1(Object p,int x)throws NullPointerException{
    if (p == null) {
      throw new NullPointerException();
    }
    return x;
  }
  public static void main(String[] args){
    
    try {
      Class class1 = Class.forName("Proxy.Main");

      Method methodList[] = class1.getDeclaredMethods();

      for(int i=0; i<methodList.length; i++){
          Method method = methodList[i];
          System.out.println("name = "+method.getName());
          System.out.println("dec1 class = "+method.getClass());

          Class pvec[] = method.getParameterTypes();//获取参数

          for(int j=0; j<pvec.length; j++){
            System.out.println("param #"+j+" "+pvec[j]);
          }

          Class evec[]= method.getExceptionTypes();//获取异常
          for(int j=0;j<evec.length; j++){
            System.out.println("exception #"+j+" "+evec[j]);
          }

          System.out.println("return type = "+method.getReturnType());
          System.out.println("----------------------------");
      }
    }catch (Throwable e) {
        System.err.println(e);
    }
  }
}
```



4.获取构造函数信息

```java
public class Main {
  public static void main(String[] args){
    try {
      Class class1 = Class.forName("Proxy.Proxy");

      Constructor constructorList[] = class1.getDeclaredConstructors();
      for(int i=0; i<constructorList.length; i++){
        Constructor constructor = constructorList[i];
        System.out.println("name = " + constructor.getName());
        System.out.println("decl class = " +constructor.getDeclaringClass());
        Class pvec[] = constructor.getParameterTypes();
        for (int j = 0; j < pvec.length; j++)
          System.out.println("param #" + j + " " + pvec[j]);
        Class evec[] = constructor.getExceptionTypes();
        for (int j = 0; j < evec.length; j++)
          System.out.println("exc #" + j + " " + evec[j]);
        System.out.println("-----");
      }
    } catch (Exception e) {
      // TODO: handle exception
      System.err.println(e);
    }
  }
}

public class Proxy {
	public Proxy(){
		
	}
	public Proxy(int i, double d){
		
	}
}
```

5.获取函数变量

```java
public class Main {
  public static void main(String[] args){
   try {
       Class class1 = Class.forName("Proxy.Proxy");
       Field fieldList[] = class1.getDeclaredFields();

       for(int i=0; i<fieldList.length; i++){
         Field field = fieldList[i];
         System.out.println("name = "+field.getName());
         System.out.println("delclass = "+field.getDeclaringClass());//获取定义类
         System.out.println("type = "+field.getType());//获取类型
         int mod = field.getModifiers();//获取private public protected属性
         System.out.println("modifier = "+Modifier.toString(mod));
         System.out.println("--------------------------------------------");
       }
     } catch (Exception e) {
       // TODO: handle exception
       System.err.println(e);
     }
  }
}

public class Proxy {
 	private RealSubject realSubject;
	public Proxy(){
		
	}
	public Proxy(int i, double d){
		
	}
}

```

6.通过方法名调用方法，也可以通过getconstructor调用constructor，

```java
public class Main {
  public static void main(String[] args){
   try {
     Class class1 = Class.forName("Proxy.Main");
     Class parType[] = new Class[2];

     parType[0] = Integer.TYPE;
     parType[1] = Integer.TYPE;

     Method method = class1.getMethod("add", parType);
     
     Main main = new Main();
     Object argList[] = new Object[2];

     argList[0] = new Integer(37);
     argList[1] = new Integer(47);

     Object reObject = method.invoke(main, argList);
     Integer retval =  (Integer)reObject;
     System.out.println(retval);


   } catch (Exception e) {
     // TODO: handle exception
     System.err.println(e);
   }
  }
  
  public int add(int a, int b)
  {
    return a + b;
  }
}
```

7.修改参数，可以修改public参数，private参数，final参数...后面两个要设置setAccessible为true

```java
public class Main {
  public static void main(String[] args){
    try {
      Class class1 = Class.forName("Proxy.Proxy");
      Proxy proxy = new Proxy();
      /*public int*/
      Field field = class1.getField("publicint");
      System.out.println("origin publicint is: "+proxy.publicint);
      field.setInt(proxy,11);
      System.out.println("origin publicint is: "+proxy.publicint);

      /*Field fieldfailed = class1.getField("privateint");
			fieldfailed.setAccessible(true);
			System.out.println("origin privateint is: "+fieldfailed.get(proxy));
			fieldfailed.setInt(proxy, 111);
			System.out.println("origin privateint is: "+fieldfailed.get(proxy));*/

      Field field1 = class1.getDeclaredField("privateint");//getField只能获取public Field，getDeclare可获取private Field...
      field1.setAccessible(true);
      System.out.println("origin privateint is: "+field1.get(proxy));
      field1.setInt(proxy, 111);
      System.out.println("origin privateint is: "+field1.get(proxy));

      Field field2 = class1.getDeclaredField("finalint");
      field2.setAccessible(true);
      System.out.println("origin privateint is: "+field2.get(proxy));
      field2.setInt(proxy, 1111);
      System.out.println("origin privateint is: "+field2.get(proxy));//成功修改final值

      //proxy.Request();

    } catch (Exception e) {
      // TODO: handle exception
      System.err.println(e);
    }
  }
}
```

```java

public class Proxy {
	
	private RealSubject realSubject;
	public int publicint = 10;
	private int privateint = 100;
	private final int finalint = 1000;
	
	public Proxy(){
		
	}
	public Proxy(int i, double d){
		
	}
	public void Request(){
		preRequest();
		realSubject = new RealSubject();
		System.out.println("aa is ： "+realSubject.getaa());
		afterRequest();
		//finalint = 1110;//final int 不能被修改值，The final field Proxy.finalint cannot be assigned错误
	}
	private void afterRequest(){
		System.out.println("after call aa");
	};
	private void preRequest(){
		System.out.println("before call aa");
	};
}


```

8.array的用法

```java
public class Main {
  public static void main(String[] args){
      try {
        Class class1 = Class.forName("java.lang.String");
        Object array = Array.newInstance(class1, 10);
        Array.set(array, 5, "this is test");
        String string = (String)Array.get(array,5);
        System.out.println(string);
      } catch (Exception e) {
        // TODO: handle exception
        System.err.println(e);
      }
  }
}
```

以上是一个简答的array的用法

```java
public class Main {
  public static void main(String[] args){
     try { 
       int tests[] = {1, 2, 3};
       //int tests [][] = {{1,2,3},{2,4,6},{3,6,9},{4,8,12}};
       Object array = Array.newInstance(Integer.TYPE, tests);//定义一个三维数组
       Object arrayobject = Array.get(array, 0);

       Class class1 = arrayobject.getClass().getComponentType();

       System.out.println(class1);

       arrayobject = Array.get(arrayobject, 1);
       Array.setInt(arrayobject, 2, 37);

       int arrcast[][][] = (int[][][])array;
       System.out.println(arrcast[0][1][2]);

     } catch (Exception e) {
       // TODO: handle exception
       System.err.println(e);
     }
  }
}
```

java 反射机制支持动态的通过名字获取类和数据结构的信息，并且能随心所欲的操纵这些信息。