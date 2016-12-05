AOP

1.前置通知（Before）：在目标方法被调用之前调用通知功能； 2.后置通知（After）：在目标方法完成之后调用通知，此时不会关心方法的输出是什么； 3.返回通知（After-returning）：在目标方法成功执行之后调用通知； 4.异常通知（After-throwing）：在目标方法抛出异常后调用通知； 5.环绕通知（Around）：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为。

Spring借助AspectJ的切点表达式语言来定义Spring切面




 
 
 
 
 

1、编写切点

创建切点：
![spring1](https://github.com/hhjuliet/study/blob/master/img/spring1.jpg)

```Java
package concert;

public interface Performance{
	public void perform();
}
```
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

