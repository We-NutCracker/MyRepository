# pom.xml引入springjar包

```
//创建的Maven包里要有Maven dependencies库
<dependencies>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-context</artifactId>
    	<version>5.3.9</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-aop</artifactId>
    	<version>5.3.9</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-beans</artifactId>
    	<version>5.3.9</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-core</artifactId>
    	<version>5.3.9</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-expression</artifactId>
    	<version>5.3.9</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-jcl</artifactId>
    	<version>5.3.9</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-web</artifactId>
    	<version>5.3.9</version>
    </dependency>
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-webmvc</artifactId>
    	<version>5.3.9</version>
    </dependency>
  </dependencies>
```



# ***1：1.1使用beans.xml文件显示配置***

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">
	<!-- 使用Xml显示的配置bean -->
	<bean id="adress" class="MS.MS_1.Adress">
		<property name="adress" value="山东省聊城市"></property>
	</bean>
 	<bean id="student" class="MS.MS_1.Student"><!-- scope是作用域有 singleton单例默认机制,prototype原型模式每次从容器get时都会产生一个新对象,//主要用于Web request,session,application -->
 		<!-- 基础类型 -->
 		<property name="name" value="阿野"></property>
 		<!-- bean注入 -->
		<property name="adress" ref="adress"></property>
		<!-- 数组类型注入 -->
		<property name="book">
			<list>
			<value>西游记</value>
				<value>水浒传</value>
				<value>三国演义</value>
			</list>
		</property>
		<!-- list类型注入 -->
		<property name="hobbie">
			<list>
				<value>听歌</value>
				<value>看电影</value>
			</list>
		</property>
			<!-- Map类型注入 -->
		<property name="card">

			<map>
				<entry key="ID" value="123456 "></entry>
				<entry key="CREID" value="123456789 "></entry>
			</map>
		</property>
			<!-- Set类型注入 -->
			<property name="games">
				<set>
					<value>王者荣耀</value>
					<value>LoL</value>
				</set>
			</property>
			<!-- null类型注入 -->
			<property name="deskmate" value="null">	
			</property>
			<!-- property类型注入 -->
			<property name="info">
				<props>
				<prop key="学号">2019214148</prop>
				</props>
			</property>
	</bean>
</beans>
```

# 1.2**使用注解配置xml自动隐式装配**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">
	<!-- Spring自动隐式配置beans -->
			<!-- 
		autowire 自动装配 
		byName会自动在容器上下文中查找和自己对象set方法后面的值对应的beanid并且不区分大小写
		id要唯一并且要和自动注入的属性的set方法的值一致
		byType会找和自己对象属性类型相同的bean 
		类型全局唯一并且要和自动注入的属性的类型方法的值一致
		 -->
		<bean id="cat" class="MS.MS_2.cat"></bean>
		<bean id="dog" class="MS.MS_2.dog"></bean>
		<bean id="people" class="MS.MS_2.people" autowire="byName">
		<property name="name" value="阿野"></property>
		</bean>
</bean>
</beans>
```

```

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
		<!-- 使用注解实现自动装配
		1.导入约束 context约束
		2配置约束的支持< context:annotation-config/> 开启注解的语句
		 -->
		 <context:annotation-config/>
		<bean id="cat0" class="MS.MS_2.cat"></bean>
		<bean id="dog0" class="MS.MS_2.dog"></bean>
		<bean id="dog1" class="MS.MS_2.dog"></bean>
		<bean id="people" class="MS.MS_2.people" >
		</bean>
</beans>
```

people类有两个宠物属性

```
public class people {
	@Autowired //可以在属性上使用也可以在set方法上使用 可以不写set方法但是需要自动装配的属性存在且满足先bytype后byname
	private cat cat;
	@Autowired
	(required = false)//允许存空，默认为true如果bean里不设置就会报错
	@Qualifier(value = "dog0")//当狗存在两个注入时自动装配不能完成可以使用@Qualifier指定唯一bean对象注入
	private dog dog;
	private String name;
	public cat getCat() {
		return cat;
	}
//	public void setCat(cat cat) {
//		this.cat = cat;
//	}
}
```

## 1.2.1：Spring中常用注解

```
常用注解
1：@Autowired 自动装配先类型后名字
可以在属性上使用也可以在set方法上使用 可以不写set方法但是需要自动装配的属性存在且满足先bytype后byname，如果不能唯一装配则需要用@Qualifier(value = "bean的id")//当狗存在两个注入时自动装配不能完成可以使用。
@Qualifier指定唯一bean对象注入。
@Nullable:注解说明字段可以为null。
@Resource:自动装配名字类型。
@Component：组件,放在类上，说明被Spring管理了在bean上注册了。
Component衍生的组按mvc分类
1controller @Controller 
2service @service
3dao @Repository
@Value（""）为属性赋值
@Scope("")单例singleton 原型prototype 
二：指定要扫描的包
<context:component-scan base-package="groupid.proid.classid"/>
```

# EX:注解Annotation

```
格式：注解是以“@注释名”在代码中存在的可以带参数
作用：不是程序本身，可以对程序作出解释，可以被其他程序（如编译器等）读取
内置注解
@Override 重写的注解
@Deprecated 不推荐使用
@SuppressWarning 镇压警告
元注解
@Target(value = {ElementType.TYPE,ElementType.METHOD})表示注解可以用在哪些地方，如类，方法
@Retention(value = RetentionPolicy.RUNTIME)表示在什么地方有效，runtime>class>sources
@Documented表示该注解被包含在javadoc中
@Inherited表示子类可以继承父类中的该注解
自定义注解
@interface
public class MyAnnocation {
	@MyAnnp(name = "阿野")//有默认值时可以不写参数
	public void test()
	{
		
	}
}

@Target(value = {ElementType.TYPE,ElementType.METHOD})
@Retention(value = RetentionPolicy.RUNTIME)
@Documented
@Inherited
@interface MyAnnp
{
	//注解的参数:参数类型+参数名（）; 
	String name()default"";
	int age()default 0;
	int id()default -1;//默认值为-1代表不存在
	
}
```

# EX:反射

```
Class 大写是类
class 小写是关键字
```

反射机制允许程序在执行期借助ReflectionAPI取得任何类的内部消息，并能够直接操作任意对象的内部属性及方法
Class 类的一个实例表示 Java 的一种数据类型，包括类、接口、枚举、注解（Annotation）、数组、基本数据类型和 void。Class 没有公有的构造方法，Class 实例是由 JVM 在类加载时自动创建的。先加载父类在加载子类。
加载-链接-初始化：将class文件加载到内存中并生成Class对象->链接时验证类信息，为静态变量分配内存并初始化，将引用值替换为内存地址，这些内存都分配在方法区->初始化执行类构造器<clinit>()方法，先初始化父类.
类加载器：系统类加载器（当前类与jar包）->扩展类加载器(jre/lib/ext的jar包)->根系统器（c/c++编写无法读取）

```
java.lang.reflect 包提供了反射中用到类，主要的类说明如下：
Constructor 类：提供类的构造方法信息。
Field 类：提供类或接口中成员变量信息。
Method 类：提供类或接口成员方法信息。
Array 类：提供了动态创建和访问 Java 数组的方法。
Modifier 类：提供类和成员访问修饰符信息。 
```

## 获取类运行时的结构

## user类

```
public class reflection {
//通过反射获取class对象
	public static void main(String[] args) throws ClassNotFoundException {
		Class<?> a=Class.forName("Reflection.user");
		//一个类在内存中只有一个Class对象
		//类被加载后，结构都被封装进Class，只要对象类型相同就是一个Class对象
		System.out.println(a);
		@SuppressWarnings("rawtypes")
		Class a1=user.class;
		System.out.println(a1);
		@SuppressWarnings("rawtypes")
		Class bClass=a.getSuperclass();
		System.out.println(bClass);
		user user=new user();
		System.out.println(user);
	}
}
class user
{
	private String name;
	private int id;
	private int age;
	public user() {}
	public user(String name, int id, int age) {
		this.name = name;
		this.id = id;
		this.age = age;
	}
	@Override
	public String toString() {
		return "user [name=" + name + ", id=" + id + ", age=" + age + "]";
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	
}
```

```
public class test_1 {
	public static void main(String[] args) {
		try {
			Class class1=Class.forName("Reflection.user");//获得Class对象
			System.out.println(class1.getName());//包名+类名
			System.out.println(class1.getSimpleName());//类名
			Field[] field=class1.getDeclaredFields();//getfileds只能寻找public，getDeclaredFields找到所有，获得指定变量在括号内传入参数名
			for(Field field2:field)
			{
				System.out.println(field2);
			}
			Method[] methods=class1.getDeclaredMethods();//同理getmethods和getDeclaredMethods前者找public后者找所有的，获得指定方法在括号内传入方法名和方法参数类型如String.class
			for(Method method:methods )
			{
				System.out.println(method);
			}
			Constructor[] constructors=class1.getDeclaredConstructors();//同理getConstructors和getDeclaredConstructors前者找public后者找所有的，获得指定方法在括号内传入方法名和方法参数类型
		//注意获取指定时方法后没有s
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

## 动态创建对象执行方法

```
public class test_2 {
	@SuppressWarnings("rawtypes")
	public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, SecurityException, IllegalArgumentException, InvocationTargetException, NoSuchFieldException, NoSuchMethodException {
		Class c1=Class.forName("Reflection.user");
		//构造一个对象
		@SuppressWarnings("deprecation")
		Object aObject=c1.newInstance();//默认使用无参构造器
		System.out.println(aObject);
		System.out.println("--------------");
		//使用构造器创建对象
		@SuppressWarnings({ "unchecked" })
		Constructor constructor=c1.getConstructor(String.class,int.class,int.class);
		user user=(user) constructor.newInstance("阿野",001,18);
		System.out.println(user);
		System.out.println("--------------");
		//通过反射调用普通方法
		@SuppressWarnings({ "deprecation" })
		user user2=(user)c1.newInstance();
		@SuppressWarnings("unchecked")
		Method setMethod=c1.getDeclaredMethod("setName", String.class);
		setMethod.invoke(user2, "doll");//invoke即使用反射得到的方法来使用类的方法，指定哪个对象和输入的参数
		System.out.println(user2.getName());
		System.out.println("--------------");
		user user3=(user)c1.newInstance();
		Field name=c1.getDeclaredField("name");
		//不能直接操作私用属性，需要获取权限或者类似user2调用set方法
		name.setAccessible(true);//获取权限使私有属性可以使用，setAccessible在方法，属性和构造器中都有
		name.set(user3, "织女");
		System.out.println(user3.getName());
		System.out.println("--------------");
;	}
}
```

## 获取泛型类型

如map<string,int>

获取后得到mao类型可以进一步获取真实类型即string和int

## 获取注解类型

### 1获得注解 

Class.getAnnocation(.class)还要强转为注解类型

### 2获得指定注解

使用反射获得属性或方法c，然后c.getAnnocation(.class)

![image-20211107155254138](C:\Users\women\AppData\Roaming\Typora\typora-user-images\image-20211107155254138.png)

# 2：使用Java的方式配置Spring

```
@Configuration//代表这是一个配置类，包含@Component
@ComponentScan("MS_4.Config.config")开启扫描来注入
public class config {
	//注册一个bean,方法的名字=bean id,方法的返回值=bean class
	@Bean
	public user getuser()
	{
		return new user();
	}
}
当有多个配置类时可以使用@Import(.class)来导入配置
```

测试类

```
package MS.MS_4;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;

import MS.MS_4.Config.config;
import MS.MS_4.pojo.user;

public class AppTest 
{
	public static void main(String[] args) {
ApplicationContext context=new  AnnotationConfigApplicationContext(config.class);//这里new AnnotationConfigApplicationtext(类不加引号)
		user User=(user) context.getBean("getUser");
		System.out.print(User.getName());
}
}
```

# 3：代理模式

## 3.1为什么要用代理模式？

**中介隔离作用：**在某些情况下，一个客户类不想或者不能直接引用一个委托对象，而代理类对象可以在客户类和委托对象之间起到中介的作用，其特征是代理类和委托类实现相同的接口。

**开闭原则，增加功能：**代理类除了是客户类和委托类的中介之外，我们还可以通过给代理类增加额外的功能来扩展委托类的功能，这样做我们只需要修改代理类而不需要再修改委托类，符合代码设计的开闭原则。代理类主要负责为委托类预处理消息、过滤消息、把消息转发给委托类，以及事后对返回结果的处理等。代理类本身并不真正实现服务，而是同过调用委托类的相关方法，来提供特定的服务。真正的业务功能还是由委托类来实现，但是可以在业务功能执行的前后加入一些公共的服务。例如我们想给项目加入缓存、日志这些功能，我们就可以使用代理类来完成，而没必要打开已经封装好的委托类

```
//public static Object newProxyInstance(ClassLoader loader,
//Class<?>[] interfaces,
//InvocationHandler h) {
//Objects.requireNonNull(h);
//
//final Class<?> caller = System.getSecurityManager() == null
//? null
//: Reflection.getCallerClass();
//
///*
//* Look up or generate the designated proxy class and its constructor.
//*/
//Constructor<?> cons = getProxyConstructor(caller, loader, interfaces);
//
//return newProxyInstance(caller, cons, h);
//}
```

