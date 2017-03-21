## 一、web.xml

### 1.1 配置文件的配置

>这样可以将一些配置 写在其他配置文件里，而不是堆积在web.xml里面。如果配置成spring/spring-*.xml则会扫描spring文件夹下所有以spring-开头的配置文件。导入到web.xml中。

```
<!-- 配置Spring配置文件的地址,如果有业务的spring配置,则在后面再加上Spring配置的路径 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			 /WEB-INF/config/core/spring/spring-*.xml
		</param-value>
	</context-param>
<!-- 配置Spring配置文件的地址 -->
```


### 1.2 过滤器配置（编码转换）

```
<!-- 过滤器,用来将请求的字符统一转化成 UTF-8 编码 -->
	<filter>
		<filter-name>CharacterEncoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<!-- forceEncoding用来设置是否理会 request.getCharacterEncoding()方法，设置为true则强制覆盖之前的编码格式 -->
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>

	<!-- 过滤器,用来将请求的字符统一转化成 UTF-8 编码 -->
	<filter-mapping>
		<filter-name>CharacterEncoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
<!-- 不进行任何过滤 -->
```

### 1.3 Spring 分发Servlet配置
>配置了.do和.dox则会进入Spring MVC的DispatcherServlet查看是否存在相应 的映射，如果存在相应的映射，则进行后续处理。
```
<!-- spring分发servlet -->
	<servlet>
		<servlet-name>spring</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/config/core/spring/spring-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>spring</servlet-name>
		<url-pattern>*.do</url-pattern>
	</servlet-mapping>

	<servlet-mapping>
		<servlet-name>spring</servlet-name>
		<url-pattern>*.dox</url-pattern>
	</servlet-mapping>
```


### 1.4 事务的配置
>这里主要配置事务的管理。Service层的方法名只有以save、update、remove、do、load开头才能开启事务，从而安全地操作数据库。
```
	<!-- 事务管理(同时管理jdbcTemplate和hibernate事务) -->
	<bean id="transactionManager"
		class="org.springframework.orm.hibernate4.HibernateTransactionManager">
		<!-- 管理hibernate事务 -->
		<property name="sessionFactory">
			<ref bean="sessionFactory" />
		</property>
		<!-- 管理jdbc事务 -->
		<property name="dataSource">
			<ref bean="dataSource" />
		</property>
	</bean>

	<!-- 事务拦截方法配置（通过方法名控制） -->
	<bean id="transactionAttributeSource"
		class="org.springframework.transaction.interceptor.NameMatchTransactionAttributeSource">
		<property name="properties">
			<props>
				<prop key="save*">PROPAGATION_REQUIRED</prop>
				<prop key="update*">PROPAGATION_REQUIRED</prop>
				<prop key="remove*">PROPAGATION_REQUIRED</prop>
				<prop key="do*">PROPAGATION_REQUIRED</prop>
				<prop key="load*">PROPAGATION_REQUIRED</prop>
			</props>
		</property>
	</bean>
```


### 1.5 Spring的Web加载器配置
```
<!-- 配置Spring的web加载器 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```


### 1.6 授权与安全过滤器配置

> 安全过滤器用于拦截请求，根据安全过滤器的实现来规范用户登录与否的操作行为的不同。若用户没有登录，则不能访问相应的页面以及相应的方法。（以.do为后缀的方法；以 .jsp .sac为结尾的页面）

```
<!-- 授权与认证过滤器 -->
	<filter>
		<filter-name>security</filter-name>
		<filter-class>cn.com.hadon.security.SecurityFilter</filter-class>
	</filter>

	<!-- .do会被安全过滤器拦截。而.dox不会，未登陆用户也可以访问到controller里面的方法。 映射是Spring MVC做的。安全拦截是过滤器做的。 -->
	<filter-mapping>
		<filter-name>security</filter-name>
		<url-pattern>*.do</url-pattern>
	</filter-mapping>

	<filter-mapping>
		<filter-name>security</filter-name>
		<url-pattern>*.sac</url-pattern>
	</filter-mapping>

	<filter-mapping>
		<filter-name>security</filter-name>
		<url-pattern>*.jsp</url-pattern>
	</filter-mapping>
```

### 1.7 url转向配置

>url转向配置可以在访问abc.jsp时 装饰城abc.sac?？？？？？？
```
<!-- URL 转向 -->
	<servlet>
		<servlet-name>forward</servlet-name>
		<servlet-class>cn.com.hadon.security.URLForwardServlet</servlet-class>
		<load-on-startup>0</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>forward</servlet-name>
		<url-pattern>*.sac</url-pattern>
	</servlet-mapping>
	<!-- URL 转向 -->
```


### 1.8 一些错误页面的配置

```
<!-- 错误页面 -->
	<error-page>
		<error-code>404</error-code>
		<location>/error_404.jsp</location>
	</error-page>

	<error-page>
		<exception-type>java.lang.Exception</exception-type>
		<location>/error_500.jsp</location>
	</error-page>
<!-- 错误页面 -->
```