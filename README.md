AOP

1.前置通知（Before）：在目标方法被调用之前调用通知功能； 2.后置通知（After）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么； 3.返回通知（After-returning）：在目标方法成功执行之后调用通知； 4.异常通知（After-throwing）：在目标方法抛出异常后调用通知； 5.环绕通知（Around）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为。

Spring借助AspectJ的切点表达式语言来定义Spring切面




 
 
 
 
 

1、编写切点

创建切点：

```Java
package concert;

public interface Performance{
	public void perform();
}
```
![aspectj](/img/aspectj1.jpg)

<div style="display:inline">
	<table>
		<tr>
			<th>AspectJ指示器</th>
			<th>描　　述</th>
		</tr>
		<tr>
			<td>arg()</td>
			<td>限制连接点匹配参数为指定类型的执行方法</td>
		</tr>
		<tr>
			<td>@args()</td>
			<td>限制连接点匹配参数由指定注解标注的执行方法</td>
		</tr>
		<tr>
			<td>execution()</td>
			<td>用于匹配是连接点的执行方法</td>
		</tr>
		<tr>
			<td>this()</td>
			<td>限制连接点匹配AOP代理的bean引用为指定类型的类</td>
		</tr>
		<tr>
			<td>target</td>
			<td>限制连接点匹配目标对象为指定类型的类</td>
		</tr>
		<tr>
			<td>@target()</td>
			<td限制连接点匹配特定的执行对象，这些对象对应的类要具有指定类型的注解>限制连接点匹配特定的执行对象，这些对象对应的类要具有指定类型的注解</td>
		</tr>
		<tr>
			<td>within()</td>
			<td>限制连接点匹配指定的类型</td>
		</tr>
		<tr>
			<td>@within()</td>
			<td>限制连接点匹配指定注解所标注的类型（当使用Spring AOP时，方法定义在由指定的注解所标注的类里）</td>
		</tr>
		<tr>
			<td>@annotation</td>
			<td>限定匹配带有指定注解的连接点</td>
		</tr>
	</table>
</div>
切面：

编写切点：
</br>
<h5>
Spring Aop：Spring自己原生的Aop，只能用一个词来形容：难用。你需要实现大量的接口，继承大量的类，所以spring aop一度被千夫所指。这于他的无侵入，低耦合完全冲突。不过Spring对开源的优秀框架，组建向来是采用兼容，并入的态度。所以，后来的Spring 就提供了Aspectj支持，也就是我们后来所说的基于纯POJO的Aop。
区别：spring Aop采用的动态织入，而Aspectj是静态织入。</br>

静态织入：指在编译时期就织入，即：编译出来的class文件，字节码就已经被织入了。</br>

动态织入又分静动两种，静则指织入过程只在第一次调用时执行；动则指根据代码动态运行的中间状态来决定如何操作，每次调用Target的时候都执行。有不清楚的同学，可以自己补下基础的代理知识。</br></h5>
</br>
<h2>Aspectj</h2>
基础概念：</br>

Identifiable points in the execution of the system（标识点）：系统在程序运行时暴露的，可能包括方法的执行、对象的创建、或者异常的抛出等等等...在系统中称为join point（spring aop中也叫标识点）...</br>
A construct for selecting join points（构造选择标识点的表达式）：实现横切的概念，例如，仅选择public方法去切...称为切点pointcut</br>
A construct to alter program behavior（构造程序的行为）：切点选择以后，描述这些切点的一些特定行为，称为advice，添加before、around、after..</br>
Constructs to alter static structure of the system（改变系统静态结构）：有效的实现横切的功能，例如，需要引入日志去追踪每个类，创建内部类型的声明inter-type declaration，使得这样的修改成为可能。在某些情况下，你可能需要监测特定的状态，典型的情况是特定切入点的存在，在执行这个系统之前，允许织入时的声明weave-time declaration的构建使这个成为可能...相同的是，所有这些机器和static crosscutting相关，因为他们对静态结构的影响...和动态改变系统执行的行为相反...</br>
A module to express all crosscutting constructs表达所有横切构造的模型：</br>
