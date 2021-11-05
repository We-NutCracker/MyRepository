```
pom.xml引入springjar包
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
				<prop key="性别">男</prop>
				</props>
			</property>
	</bean>
</beans>
```

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

```
一：使用注解开发
常用注解
1：@Autowired 自动装配先类型后名字
可以在属性上使用也可以在set方法上使用 可以不写set方法但是需要自动装配的属性存在且满足先bytype后byname，如果不能唯一装配则需要用@Qualifier(value = "bean的id")//当狗存在两个注入时自动装配不能完成可以使用。
@Qualifier指定唯一bean对象注入。
@Nullable:注解说明字段可以为null。
@Resource:自动装配名字类型。
@Component：组件,放在类上，说明被Spring管理了在bean上注册了。
Component衍生的组件
1controller @Controller 
2service @service
3dao @Repository
@Value（""）为属性赋值
@Scope("")单例singleton 原型prototype 
二：指定要扫描的包
<context:component-scan base-package="groupid.proid.classid"/>
```

```
使用Java的方式配置Spring
@Configuration//代表这是一个配置类，包含@Component
public class config {
	//注册一个bean,方法的名字=bean id,方法的返回值=bean class
	@Bean
	public user getuser()
	{
		return new user();
	}
}
```

