1.使用spring mvc不用带.do等后缀区分静态文件不被spring mvc拦截

   1.1.在web.xml中配置

	   <servlet>
			<servlet-name>springMVC</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:springMVC-servlet.xml</param-value>
			</init-param>
			<load-on-startup>1</load-on-startup>
		</servlet>
		<servlet-mapping>
			<servlet-name>springMVC</servlet-name>
			<url-pattern>/</url-pattern>
		</servlet-mapping>
   1.2.在springMVC-servlet.xml中配置

    <mvc:default-servlet-handler />
  
在springMVC-servlet.xml中配置<mvc:default-servlet-handler />后，会在Spring MVC上下文中定义一个org.springframework.web.servlet.resource.DefaultServletHttpRequestHandler，它会像一个检查员，对进入DispatcherServlet的URL进行筛查，如果发现是静态资源的请求，就将该请求转由Web应用服务器默认的Servlet处理，如果不是静态资源的请求，才由DispatcherServlet继续处理。

一般Web应用服务器默认的Servlet名称是"default"，因此DefaultServletHttpRequestHandler可以找到它。如果你所有的Web应用服务器的默认Servlet名称不是"default"，则需要通过default-servlet-name属性显示指定：

<mvc:default-servlet-handler default-servlet-name="所使用的Web服务器默认使用的Servlet名称" />

2.视图解析默认目录放在了WEB-INF/views目录下，如何再返回到webapp根目录下的视图


  	return new ModelAndView("redirect:/index.html");

3.spring mvc使用拦截器的情况下，不拦截静态文件和指定的请求

	 <mvc:interceptors>
			<mvc:interceptor>
				<mvc:mapping path="/**" />
				<mvc:exclude-mapping path="/" />
				<mvc:exclude-mapping path="/login" />
				<mvc:exclude-mapping path="/logout" />
				<mvc:exclude-mapping path="/index.html" />
				<mvc:exclude-mapping path="/**/*.js" />
				<mvc:exclude-mapping path="/**/*.img" />
				<mvc:exclude-mapping path="/**/*.css" />
				<mvc:exclude-mapping path="/**/*.min.css" />
				<mvc:exclude-mapping path="/**/*.png" />
				<mvc:exclude-mapping path="/**/*.jpg" />
				<mvc:exclude-mapping path="/**/*.gif" />
				<mvc:exclude-mapping path="/**/*.eot" />
				<mvc:exclude-mapping path="/**/*.svg" />
				<mvc:exclude-mapping path="/**/*.ttf" />
				<mvc:exclude-mapping path="/**/*.woff" />
				<mvc:exclude-mapping path="/**/*.woff2" />
				<bean
					class="拦截器的全限定名">
				</bean>
			</mvc:interceptor>
		</mvc:interceptors>