<h2 id='mvc'>Web MVC framework框架</h2>
<h3 id='mvc-introduction'>Spring Web MVC框架简介</h3>
Spring MVC的核心是`DispatcherServlet`，该类作用非常多，分发请求处理，配置处理器映射，处理视图view，本地化，时间区域和主题，也支持文件上传。默认的处理器依赖于`@Controller`和`RequestMapping`注解，提供了大量的灵活的处理方法。spring3.0中就介绍过了，`@Controller`机制，可通过SpringＭＶＣ提供的`@PathVariable`注解和其他功能,创建`RESTful`web网站和应用。
```text
在spring MVC中一个关键的设计就是“开闭原则”，即对扩展开放对修改闭合原则。Spring 一些核心类的方法
是`final`方法。开发者不能重写这些方法，来增加自己的行为。这些都不是随意决定，而是意在符合“开闭原则”。


预知此原则详情，参看 Seth Ladd(这应该就是马丁大叔)等人的
*Expert Spring Web MVC and Web Flow*，尤其是要看“A Look At Desig” 章节，在第一版的117页

* [Bob Martin, The Open-Closed Principle (PDF)](http://www.objectmentor.com/resources/articles/ocp.pdf)

使用Spring MVC时不能对final 方法织入增强。比如，你不能对`AbstractController.setSynchronizeOnSession()`方法织入增强。关于AOP代理和之所以不能对final方法织入增强，请参看[Section 9.6.1, “Understanding AOP proxies”](#aop-understanding-aop-proxies)
```

在Spring Web MVC中，可以使用任意对象作为请求命令或者是表单回写对象;无需实现框架指定接口或者是基类。spring数据绑定非常灵活：比如，它将类型不匹配处理为验证错误，该错误由应用抛出，而不是作为系统错误。因此，无需你复制你的业务对象属性、非常简单的就能处理form表单中未定义的字串，或者是转换Strings的类型。当然，最好是直接绑定你的业务对象。

Spring的视图解决方案非常灵活。一个`Controller`通常是负责将数据转换成model map，并选择一个view name，但是它也能直接向response 流中写入来完成request.View视图名字解决方案是可配置的，实现途径多种多样：通过文件扩展或者Accetp header content type，通过bean名字，一个properties属性文件,甚至是自定义的`ViewResolver`实现。model模型（MVC中的M）是一个`Map`接口，它是视图技术的基础。可以直接集成基于渲染技术的模板，像JSP,Velocity和Freemarker,或者直接生成XML,JSON,Atom和许多其他类型的内容。model Map将会进行简单的转换为合适的格式，像JSP中的reqeust attributes,Velocity模板的model。

<h4 id='mvc-features'>Spring Web MVC的功能</h4>
```text
Spring Web Flow

Spring Web Flow (SWF)意在管理web应用页面流程的。 

SWF支持现有框架集成，比如Spring MVC和JSF，Servelt环境和Port了环境都行。如果你有也业务，需要将model模型转换为纯粹的request model，那么SWF也许是最好的解决方案 。

SWF允许你捕获逻辑页面流程作为字包含模块，该模块将会在不同的场景中重用，比如，用户向导就是通过控制导航用来驱动业务处理。

For more information about SWF, consult the Spring Web Flow website.
有关SWF更多的信息，参阅[Spring Web Flow网站 ](http://projects.spring.io/spring-webflow/)
```

Spring的 web模块包含很多特有的web 支持功能：
* 清晰的角色分离：controller, validator, command object, form object, model object, DispatcherServlet, handler mapping, view resolver等等，都存在相关的专用对象
* 强大的、简单的配置。配置能力包括易于跨context引用，比如在web controller中引用业务对象。
* 适应能力强、非侵入，非常灵活。controller的方法签名随意定义，参数可用注解用来解决给定的场景（比如@RequestPram,@RequestHeader,@PathVariable等等）
* 重用业务代码，无需重复。使用已经存在的业务对象或者form 对象，无需复制或者继承指定的框架基类。
* 自定义数据绑定和验证。类型不匹配作为应用级别验证错误并保持现其值，本地时间和数字绑定等等用来替代现有的转换机制，现有转换机制是指：仅有String的form对象和业务对象之间互相转换
* 自定义handler mapping处理映射和视图解决方案。Handler mapping和视图解决方案策略，从简单的到复杂的，以及特定的解决策略，都行。Spring和其他mvc框架相比，更灵活。
* Flexible model transfer. Model transfer with a name/value Map supports easy integration with any view technology.
* Customizable locale, time zone and theme resolution, support for JSPs with or without Spring tag library, support for JSTL, support for * * * Velocity without the need for extra bridges, and so on.
* A simple yet powerful JSP tag library known as the Spring tag library that provides support for features such as data binding and themes. The custom tags allow for maximum flexibility in terms of markup code. For information on the tag library descriptor, see the appendix entitled Chapter 39, spring.tld
* A JSP form tag library, introduced in Spring 2.0, that makes writing forms in JSP pages much easier. For information on the tag library descriptor, see the appendix entitled Chapter 40, spring-form.tld
* Beans whose lifecycle is scoped to the current HTTP request or HTTP Session. This is not a specific feature of Spring MVC itself, but rather of the WebApplicationContext container(s) that Spring MVC uses. These bean scopes are described in Section 5.5.4, “Request, session, and global session scopes”


<h3 id='mvc-introduction-pluggability'>其他MVC实现的可拔插集成</h3>
在有些项目中，非SPring的 MVC实现是可取的。很多团队希望利用已经存在的技术和工具，比如JSF.
如果不想使用Spring’s Web MVC，但是想使用Spring其他的东西，那么就可以使用Spring集成你选择的MVC框架，非常容易。通过`ContextLoaderListener`启动Spring root application Context （Spring上下文），在任意的action对象中通过`ServletContext`属性访问上下文环境。无插件，无集成。在web层的view中，像使用类库一样使用Spring,root application context应用上下文作为Spring的访问入口。
反正就是一句话，不用Spring MVC，照样可以使用Spring管理bean ,注册Service

<h3 id='mvc-servlet>DispatcherServlet</h3>
Spring’s web MVC framework像许多其他的web MVC框架一样，request驱动，以一个Servlet为中心，该Servlet分发请求给controller ，并提供web引用开发相关的工具。Spring的`DispatcherServlet`不仅仅是只干这些。它和Spring IoC容器完全无缝集成，因此可以使用spring所有的功能。 
下图展示Spring Web MVC `DispatcherServlet`处理request的流程。细心的读者会看到，`DispatcherServlet`就是*表示层设计模式*(这个模式是Spring MVC共享给其他主流web框架的)
![DispatchServlet](http://docs.spring.io/spring/docs/4.2.0.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/mvc.png)

Spring MVC中处理request的流程（架构示意图）

`DispatcherServlet`是一个`Servlet`，它继承自`HttpServlet`，像下面这样的在`web.xml`中的声明。还得映射需要交由`DispatcherServlet`处理的requests,同样也是在`web.xml`中使用URL映射。这是一个标准的Java EE Servlet配置；下面的样例展示了`DispatcherServlet`声明和映射:
```xml
<web-app>
    <servlet>
        <servlet-name>example</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>example</servlet-name>
        <url-pattern>/example/*</url-pattern>
    </servlet-mapping>

</web-app>
```

在上面的样例中，所有的以`/example`开头的request将会由一个叫`example`的`DispatcherServlet`实例处理。在Servlet3.0+的环境中，还可以通过编程式的方式配置DispatcherServlet。下面的代码好上面的xml配置是等效的:
```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
        ServletRegistration.Dynamic registration = container.addServlet("dispatcher", new DispatcherServlet());
        registration.setLoadOnStartup(1);
        registration.addMapping("/example/*");
    }

}
```

`WebApplicationInitializer `是由Spring MVC提供的一个接口，Spring MVC还可以确保你基于代码的配置可以被探测并自动初始化一个Servlet 3容器。有个抽象基类实现类此接口，叫`AbstractDispatcherServletInitializer`,可以更容易的注册`DispatcherServlet `，只需要简单的指定映射即可，详情请参看[Code-based Servlet container initialization](#mvc-container-config)。

上面只是配置Spring Web MVC的第一步。还需要通过使用Spring Web MVC framework配置各种beans。

就像[Section 5.15, “Additional Capabilities of the ApplicationContext”](#context-introduction)所讲的,Spring中的`ApplicationContext `实例是有作用域的。在 Web MVC框架中，每一个`DispatcherServlet`都有自己的`WebApplicationContext`，该`WebApplicationContext`继承了根`WebApplicationContext`，因此，子`WebApplicationContext`可以访问父容器中定义的所有的bean。TODO。These inherited beans can be overridden in the servlet-specific scope, and you can define new scope-specific beans local to a given Servlet instance.这些集成来的bean可以在servlet-specific作用域内被覆盖，也可以为Servlet实例指定新的作用域bean。

**Figure 20.1. Context hierarchy in Spring Web MVC**
![](http://docs.spring.io/spring/docs/4.2.0.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/mvc-contexts.gif)

`DispatcherServlet`初始化时，Spring MVC会查找并加载名为[servlet-name]-servlet.xml的文件，默认查找的目录是web应用的`WEB-INF`目录，加载完成后就创建xml中定义的beans，会覆盖容器中所有的重名bean。

考虑下面`DispatcherServlet`Servlet配置（在web.xml文件中）
```
<web-app>
    <servlet>
        <servlet-name>golfing</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>golfing</servlet-name>
        <url-pattern>/golfing/*</url-pattern>
    </servlet-mapping>
</web-app>
```
根据上面的配置，必须得在应用中存在`/WEB-INF/golfing-servlet.xml`文件。这个文件中包含了所有Spring Web MVC指定的组件（beans）。配置文件的位置也是可以修改的，通过Servlet初始化参数，下面详细讲解。

若只有一个`DispatcherServlet`，且只有一个配置文件，那么就可以不用设置Servlet 初始化参数`contextConfigLocation`。
像下面这样讲解如何通过Servlet参数设置来修改配置文件位置:
```xml
<web-app>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/root-context.xml</param-value>
    </context-param>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value></param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>
```

`WebApplicationContext`是简单的`ApplicationContext`的扩展，`WebApplicationContext`有一些web应用所必须的方法。它和`ApplicationContext `的不同之处就是对于主题处理的能力[(see Section 20.9, “Using themes”)](#mvc-themeresolver),和并且它知道它和哪一个Servlet项关联(有一个ServletContext的引用)。`WebApplicationContext`被`ServletContext`绑定，如果你需要访问它，使用`RequestContextUtils`类的静态方法可以检索到`WebApplicationContext`

<h4 id='mvc-servlet-special-bean-types'>WebApplicationContext中的特殊bean</h4>
`DispatcherServlet `使用特殊的bean处理request和相应的view。这些bean是Spring  MVC的一部分。可以通过简单的配置选择`WebApplicationContext`中的特殊bean。如果你啥都不配置，也没关系，Spring都为这些bean指定了默认的实现。接下来看看这些特殊bean。

Bean type	| Explanation
----------- | -------------
HandlerMapping | Maps incoming requests to handlers and a list of pre- and post-processors (handler interceptors) based on some criteria the details of which vary by HandlerMapping implementation. The most popular implementation supports annotated controllers but other implementations exists as well.
HandlerAdapter | Helps the DispatcherServlet to invoke a handler mapped to a request regardless of the handler is actually invoked. For example, invoking an annotated controller requires resolving various annotations. Thus the main purpose of a HandlerAdapter is to shield the DispatcherServlet from such details.
HandlerExceptionResolver | Maps exceptions to views also allowing for more complex exception handling code.
ViewResolver | Resolves logical String-based view names to actual View types.
LocaleResolver & LocaleContextResolver
Resolves the locale a client is using and possibly their time zone, in order to be able to offer internationalized views
ThemeResolver | Resolves themes your web application can use, for example, to offer personalized layouts
MultipartResolver | Parses multi-part requests for example to support processing file uploads from HTML forms.
FlashMapManager | Stores and retrieves the "input" and the "output" FlashMap that can be used to pass attributes from one request to another, usually across a redirect.

<h4 id='mvc-servlet-config'>DispatcherServlet默认配置</h4>
`DispatcherServlet `中使用的特殊bean的默认实现，其信息配置在`org.springframework.web.servlet`包中的`DispatcherServlet.properties`。
特殊bean默认实现的存在都是有道理的。很快你就会指定这些bean的自定义实现。比如，有个非常常用的配置，修改`InternalResourceViewResolver `类的`prefix `来设置view 文件的目录。

Regardless of the details, the important concept to understand here is that once you	configure a special bean such as an InternalResourceViewResolver in your WebApplicationContext, you effectively override the list of default implementations that would have been used otherwise for that special bean type. For example if you configure an InternalResourceViewResolver, the default list of ViewResolver implementations is ignored.

In Section 20.16, “Configuring Spring MVC” you’ll learn about other options for configuring Spring MVC including MVC Java config and the MVC XML namespace both of which provide a simple starting point and assume little knowledge of how Spring MVC works. Regardless of how you choose to configure your application, the concepts explained in this section are fundamental should be of help to you.

<h4 id='mvc-servlet-sequence'>DispatcherServlet 处理顺序</h4>
After you set up a DispatcherServlet, and a request comes in for that specific DispatcherServlet, the DispatcherServlet starts processing the request as follows:

* The WebApplicationContext is searched for and bound in the request as an attribute that the controller and other elements in the process can use. It is bound by default under the key DispatcherServlet.WEB_APPLICATION_CONTEXT_ATTRIBUTE.
* The locale resolver is bound to the request to enable elements in the process to resolve the locale to use when processing the request (rendering the view, preparing data, and so on). If you do not need locale resolving, you do not need it.
* The theme resolver is bound to the request to let elements such as views determine which theme to use. If you do not use themes, you can ignore it.
* If you specify a multipart file resolver, the request is inspected for multiparts; if multiparts are found, the request is wrapped in a MultipartHttpServletRequest for further processing by other elements in the process. See Section 20.10, “Spring’s multipart (file upload) support” for further information about multipart handling.
* An appropriate handler is searched for. If a handler is found, the execution chain associated with the handler (preprocessors, postprocessors, and controllers) is executed in order to prepare a model or rendering.
* If a model is returned, the view is rendered. If no model is returned, (may be due to a preprocessor or postprocessor intercepting the request, perhaps for security reasons), no view is rendered, because the request could already have been fulfilled.
* Handler exception resolvers that are declared in the WebApplicationContext pick up exceptions that are thrown during processing of the request. Using these exception resolvers allows you to define custom behaviors to address exceptions.

The Spring DispatcherServlet also supports the return of the last-modification-date, as specified by the Servlet API. The process of determining the last modification date for a specific request is straightforward: the DispatcherServlet looks up an appropriate handler mapping and tests whether the handler that is found implements the LastModified interface. If so, the value of the long getLastModified(request) method of the LastModified interface is returned to the client.

You can customize individual DispatcherServlet instances by adding Servlet initialization parameters ( init-param elements) to the Servlet declaration in the web.xml file. See the following table for the list of supported parameters.

**Table 20.2. DispatcherServlet initialization parameters**

Parameter | Explanation
--------- | -----------
contextClass | Class that implements WebApplicationContext, which instantiates the context used by this Servlet. By default, the XmlWebApplicationContext is used.
contextConfigLocation | String that is passed to the context instance (specified by contextClass) to indicate where context(s) can be found. The string consists potentially of multiple strings (using a comma as a delimiter) to support multiple contexts. In case of multiple context locations with beans that are defined twice, the latest location takes precedence.
namespace | Namespace of the WebApplicationContext. Defaults to [servlet-name]-servlet.

<h3 id='mvc-controller'>实现Controller</h3>
Controllers提供了访问应用的入口。Controllers解析request并转换为model模型，模型向view视图提供数据。Spring高度抽象了controller，这样开发者可通过各种方式创建controller。
Spring 2.5开始，可以使用注解创建controller，比如`@RequestMapping, @RequestParam, @ModelAttribute`等等。这些注解既可用于Spring MVC也可用于 Portlet MVC。这种方式无需继承指定基类或者实现指定接口。此外，无需依赖`Servlet `API或者`Portlet `API，但是可以非常方便的访问他们。

![](http://docs.spring.io/spring/docs/4.2.0.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/tip.png)
> 大量的web都是使用注解的，比如* MvcShowcase, MvcAjax, MvcBasic, PetClinic, PetCare*等等，不信你看[https://github.com/spring-projects/](https://github.com/spring-projects/)

```java
@Controller
public class HelloWorldController {

    @RequestMapping("/helloWorld")
    public String helloWorld(Model model) {
        model.addAttribute("message", "Hello World!");
        return "helloWorld";
    }
}
```

`@Controller`和`@RequestMapping`注解不对方法的名和签名做限制，你可以随意，呃，注意要符合java规范。示例中的方法接受了一个`Model`，返回了一个字串view，除此之外Spring还提供了很多的方法参数、返回值，预知详情，请关注文档更新进度。`@Controller`和`@RequestMapping`还有很多注解构成了Spring MVC实现，本章将详细讲解他们在Servlet环境中的用法。

<h4 id='mvc-ann-controller'>使用@Controller定义一个controller</h4>
`@Controller`注解的类意味着该类是MVC中的C角色,无需继承C角色积累，也无需引用ServletAPI*译注，比如HttpServletReqeust,HttpServletResponse*，若有需要，也能非常容易的引用ServletAPI。

` @Controller`注解表示类作为conroler控制器代码层，dispatcher扫描@Controller注解类并探测`@RequestMapping` 注解然后做映射。

可以在 dispatcher’s上下文环境中，明确的定义注解了`@Controller`类的Spring bean。然而，使用`@Controller`注解的类，可自动探测并自动注册。

为了开启自动探测，得在配置中增加组件扫描功能。在XML 中使用`spring-context`schema:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.springframework.samples.petclinic.web"/>

    <!-- ... -->

</beans>
```

<h4 id='mvc-ann-requestmapping'>使用@RequestMapping做映射</h4>
`@RequestMapping`注解可以将URL映射为类入口或者方法入口，比如URL `/appointments`由`AppointmentsController `类处理。通常，类级别注解`@RequestMapping`映射reqeust路径，方法级别`@RequestMapping`用来指明要处理reqeust 路径下的哪些HTTP方法，比如POST/GET/PUT/DELETE等等，或者是指明要处理哪些参数条件。
看样例:
```java
@Controller
//所有"/appointments"路径请求由本controller处理
@RequestMapping("/appointments")
public class AppointmentsController {

    private final AppointmentBook appointmentBook;

    @Autowired
    public AppointmentsController(AppointmentBook appointmentBook) {
        this.appointmentBook = appointmentBook;
    }

    @RequestMapping(method = RequestMethod.GET)
	// HTTP GET方法请求的"/appointments"由此方法处理
    public Map<String, Appointment> get() {
        return appointmentBook.getAppointmentsForToday();
    }

    @RequestMapping(value="/{day}", method = RequestMethod.GET)
	//URI模板模式
    public Map<String, Appointment> getForDay(@PathVariable @DateTimeFormat(iso=ISO.DATE) Date day, Model model) {
        return appointmentBook.getAppointmentsForDay(day);
    }

    @RequestMapping(value="/new", method = RequestMethod.GET)
	//处理HTTP GET方法请求的"/appointments/new"
    public AppointmentForm getNewForm() {
        return new AppointmentForm();
    }

    @RequestMapping(method = RequestMethod.POST)
	// HTTP POST方法请求的"/appointments"由此方法处理
    public String add(@Valid AppointmentForm appointment, BindingResult result) {
        if (result.hasErrors()) {
            return "appointments/new";
        }
        appointmentBook.addAppointment(appointment);
        return "redirect:/appointments";
    }
}
```
上例中多处使用了`@RequestMapping`。第一处是类注解,不翻了，全写注释里了。

`getForDay()`方法展示了`@RequestMapping`另一种用法:URI模板，详情参看[mvc-ann-requestmapping-uri-templates](#mvc-ann-requestmapping-uri-templates)

`@RequestMapping`类注解不是必须的。若不写的话，不利于路径规划。看样例:
```java
@Controller
public class ClinicController {

    private final Clinic clinic;

    @Autowired
    public ClinicController(Clinic clinic) {
        this.clinic = clinic;
    }

    @RequestMapping("/")
    public void welcomeHandler() {
    }

    @RequestMapping("/vets")
    public ModelMap vetsHandler() {
        return new ModelMap(this.clinic.getVets());
    }

}
```
上例中未指定GET vs. PUT, POST等方法，因为默认情况下，`@RequestMapping`将会处理相关路径下的所有的HTTP方法。使用这种`@RequestMapping(method=GET)`方式才能精准的映射。

<h5 id='mvc-ann-requestmapping-proxying'>@Controller和AOP代理</h5>
有些情况下，controller也许会有AOP代理装饰。比如，在controller上直接定义`@Transactional`注解。这种情况，推荐使用类注解。然而，如果controller需要实现一个非Spring 回调接口(也就是`InitializingBean, *Aware`等等),则需要明确的配置基于类的代理。比如，使用了`<tx:annotation-driven />`就得改为`<tx:annotation-driven proxy-target-class="true" />`。

<h5 id='mvc-ann-requestmapping-31-vs-30'>Spring 3.1中为@RequestMapping方法新增的支持类</h5>
Spring 3.1 introduced a new set of support classes for @RequestMapping methods called RequestMappingHandlerMapping and RequestMappingHandlerAdapter respectively. They are recommended for use and even required to take advantage of new features in Spring MVC 3.1 and going forward. The new support classes are enabled by default by the MVC namespace and the MVC Java config but must be configured explicitly if using neither. This section describes a few important differences between the old and the new support classes.

Prior to Spring 3.1, type and method-level request mappings were examined in two separate stages — a controller was selected first by the DefaultAnnotationHandlerMapping and the actual method to invoke was narrowed down second by the AnnotationMethodHandlerAdapter.

With the new support classes in Spring 3.1, the RequestMappingHandlerMapping is the only place where a decision is made about which method should process the request. Think of controller methods as a collection of unique endpoints with mappings for each method derived from type and method-level @RequestMapping information.

This enables some new possibilities. For once a HandlerInterceptor or a HandlerExceptionResolver can now expect the Object-based handler to be a HandlerMethod, which allows them to examine the exact method, its parameters and associated annotations. The processing for a URL no longer needs to be split across different controllers.

There are also several things no longer possible:

Select a controller first with a SimpleUrlHandlerMapping or BeanNameUrlHandlerMapping and then narrow the method based on @RequestMapping annotations.
Rely on method names as a fall-back mechanism to disambiguate between two @RequestMapping methods that don’t have an explicit path mapping URL path but otherwise match equally, e.g. by HTTP method. In the new support classes @RequestMapping methods have to be mapped uniquely.
Have a single default method (without an explicit path mapping) with which requests are processed if no other controller method matches more concretely. In the new support classes if a matching method is not found a 404 error is raised.
The above features are still supported with the existing support classes. However to take advantage of new Spring MVC 3.1 features you’ll need to use the new support classes.

<h5 id='mvc-ann-requestmapping-uri-templates'>URI模板模式</h5>
*URI模板*大大的方便了@RequestMapping方法中URL配置。
URI 模板是类URI字串，包含一个或多个变量名，为变量设置值时，它就成了URI。在[proposed RFC](http://bitworking.org/projects/URI-Templates/)中定义了是如何参数化的。比如，URI模板`http://www.example.com/users/{userId}`包含一个变量userId,设置userId变量的值为*fred*，`http://www.example.com/users/fred`。

在方法参数上使用 `@PathVariable`注解，将会绑定URI中变量的值到参数上:
```java
@RequestMapping(value="/owners/{ownerId}", method=RequestMethod.GET)
public String findOwner(@PathVariable String ownerId, Model model) {
    Owner owner = ownerService.findOwner(ownerId);
    model.addAttribute("owner", owner);
    return "displayOwner";
}
```

URI模板`/owners/{ownerId}`声明了变量`ownerId`。当ctroller处理该request时候，Spring MVC将会从请求路径中取出ownerId变量值，将变量值绑定到参数上。比如，请求`/owners/fred`时，变量ownerId就是`fred`。

![DispatchServlet](http://docs.spring.io/spring/docs/4.2.0.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/tip.png)
在处理`@PathVariable`过程中，Spring MVC 以 by name方式从URI模板中匹配变量，`@PathVariable`可以指定name:
```java
@RequestMapping(value="/owners/{ownerId}", method=RequestMethod.GET)
public String findOwner(@PathVariable("ownerId") String theOwner, Model model) {
    // implementation omitted
}
```

如果URI模板中变量name和方法参数name相同，则无需配置@PathVariable的name。只要是编译时未去除调试信息，Spring MVC就能匹配与参数重名的URI模板变量:
```java
@RequestMapping(value="/owners/{ownerId}", method=RequestMethod.GET)
public String findOwner(@PathVariable String ownerId, Model model) {
    // implementation omitted
}
```

一个方法可以有多个`@PathVariable`
```java
@RequestMapping(value="/owners/{ownerId}/pets/{petId}", method=RequestMethod.GET)
public String findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {
    Owner owner = ownerService.findOwner(ownerId);
    Pet pet = owner.getPet(petId);
    model.addAttribute("pet", pet);
    return "displayPet";
}
```

当`@PathVariable`注解用于`Map<String, String>`参数，所有的URI 模板中的变量都会置入map。

URI模板可以是`@RequestMapping`类注解和方法注解的合集，比如URL `/owners/42/pets/21`将会调用`findPet()`方法
```java
@Controller
@RequestMapping("/owners/{ownerId}")
public class RelativePathUriTemplateController {

    @RequestMapping("/pets/{petId}")
    public void findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {
        // implementation omitted
    }

}
```

`@PathVariable`参数可以是任何简单类型，`int, long, Date`等等,Spring 自动转换合适的类型，转换不成功时，抛异常`TypeMismatchException `。不过，可以注册自定义转换器。详情参看[the section called “Method Parameters And Type Conversion”](#mvc-ann-typeconversion) and [the section called “Customizing WebDataBinder initialization”](#mvc-ann-webdatabinder).

<h5 id='mvc-ann-requestmapping-uri-templates-regex'>URI 模板模式与正则表达式</h5>
有时需要对URI模板变量进行精准的控制，比如`"/spring-web/spring-web-3.0.5.jar"`如何定义各个部分变量
` @RequestMapping`支持URI模板变量中正则表达式，`{varName:regex}`语法，前面是变量名字，后面是正则表达式。
```java
@RequestMapping("/spring-web/{symbolicName:[a-z-]+}-{version:\\d\\.\\d\\.\\d}{extension:\\.[a-z]+}")
    public void handle(@PathVariable String version, @PathVariable String extension) {
        // ...
    }
}
```

<h5 id='mvc-ann-requestmapping-patterns'>路径模式</h5>
`@RequestMapping`也支持ant风格路径，比如`/myPath/*.do`。URI模板变量和ant风格路径也可以组合使用，`/owners/*/pets/{petId}`。

<h5 id='mvc-ann-requestmapping-pattern-comparison'>路径模式比较</h5>
当URL匹配了多种模式，各种模式会有一个匹配度的排序，也就是说哪些模式更精准的匹配，就会选用哪些模式。

具有较低计数量的模板变量和通配符更特殊、更具体。比如，`/hotels/{hotel}/*`比`/hotels/{hotel}/**`更具体、更优先。

如果两个模式具有相同的计数量，俺么更短的胜出。比如，`/foo/bar*`比`/foo/*`更长，因此更具体

当两个模式具有相同的计数和长度，具有更少的通配符的胜出。`/hotels/{hotel}`比`/hotels/*`更优先。

还有一些其他的规则
* **默认的映射模式**`/**`比任何其他模式都具有更低的优先级，比如，`/api/{a)}/{b}/{c}`与之相比更具体更优先
* **前缀模式**`/public/**`比其他任何没有双通配符的模式具有更低的优先级，比如，`/public/path3/{a}/{b}/{c}`比它更具体更优先

更多详情参看`AntPathMatcher`中的`AntPatternComparator`。注意PathMatcher 可以自定义，参看[ Section 20.16.9, “Path Matching”](#mvc-config-path-matching)

<h5 id='mvc-ann-requestmapping-placeholders'>路径模式和占位符</h5>
`@RequestMapping`注解支持${...}占位符，占位符读取的是本地properties、系统properties、环境变量。若是需要根据配置文件来改变controller映射路径，此办法就可以大显身手了。占位符的更多细节，请参看`PropertyPlaceholderConfigurer`类的javadocs。

<h5 id='mvc-ann-requestmapping-suffix-pattern-match'>前缀匹配路径模式</h5>
Spring MVC默认执行`".*"`前缀模式匹配，因此`/person`也会匹配`/persion.*`。这样就可以通过文件扩展名指明内容类型，比如`/pserson.pdf`,`/person.xml`等等。
由此产生一个让人困惑的地方，就是当路径最后的部分是一个URI变量，比如`/persion/{id}`。当request请求`/psersion/1.json`，既能匹配路径变量 id=1 ，也能匹配扩展名".json"，当id包含一个点的时候，比如`/person/joe@email.com`，spring将不会认为`joe@email.com`是id，但是，`com`不是文件扩展名。

要解决此问题，得配置Spring MVC 的前缀模式匹配和注册的文件扩展名协商处理 。For more on this, first see [Section 20.16.4, “Content Negotiation” ](#mvc-config-content-negotiation)and then [Section 20.16.9, “Path Matching” ](#mvc-config-path-matching)展示了如何开启前缀匹配和如何只用注册的前缀匹配。

<h5 id='mvc-ann-matrix-variables'>Matrix Variables矩阵变量</h5>
URI规范，是在路径中可能含有键值对。在规范中并未包含特殊项。SpringMVC就能搞这些特殊项。
矩阵变量可以出现在任意路径中，每一个矩阵变量有";"分号分隔。比如:
`"/cars;color=red;year=2012"`，多个值的话使用","逗号分隔，`"color=red,green,blue"`，或者使用重复的变量名`"color=red;color=green;color=blue"`。 

If a URL is expected to contain matrix variables, the request mapping pattern must represent them with a URI template. This ensures the request can be matched correctly regardless of whether matrix variables are present or not and in what order they are provided.

下例演示解析矩阵变量"q":
```java
// GET /pets/42;q=11;r=22

@RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET)
public void findPet(@PathVariable String petId, @MatrixVariable int q) {

    // petId == 42
    // q == 11

}
```

因为路径中的任意段都可能会包含矩阵变量，有些场景下你需要更特殊的用法去识别变量:
```java
// GET /owners/42;q=11/pets/21;q=22

@RequestMapping(value = "/owners/{ownerId}/pets/{petId}", method = RequestMethod.GET)
public void findPet(
        @MatrixVariable(value="q", pathVar="ownerId") int q1,
        @MatrixVariable(value="q", pathVar="petId") int q2) {

    // q1 == 11
    // q2 == 22

}
```
矩阵变量也可以定义为可选，并设置默认值
```java
// GET /pets/42

@RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET)
public void findPet(@MatrixVariable(required=false, defaultValue="1") int q) {

    // q == 1

}
```

矩阵变量可以置入一个Map:
```java
// GET /owners/42;q=11;r=12/pets/21;q=22;s=23

@RequestMapping(value = "/owners/{ownerId}/pets/{petId}", method = RequestMethod.GET)
public void findPet(
        @MatrixVariable Map<String, String> matrixVars,
        @MatrixVariable(pathVar="petId"") Map<String, String> petMatrixVars) {

    // matrixVars: ["q" : [11,22], "r" : 12, "s" : 23]
    // petMatrixVars: ["q" : 11, "s" : 23]

}
```

注意，若要开启矩阵变量功能，必须设置`RequestMappingHandlerMapping`的属性`removeSemicolonContent `为`false`。该值默认为`true`。

MVC Java config and the MVC namespace都提供了开启矩阵变量的选项。

若是Java config，[Advanced Customizations with MVC Java Config](#mvc-config-advanced-java)章节讲解了如何设置`RequestMappingHandlerMapping `.

若是MVC namespace命名空间，`<mvc:annotation-driven>`元素的`enable-matrix-variables`属性则应该设置为`true`。默认他是`false`。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:annotation-driven enable-matrix-variables="true"/>

</beans>
```

<h5 id='mvc-ann-requestmapping-consumes'>消费媒体类型</h5>
可以设置映射的消费媒体类型，类型可以指定多个。那么，只有reqeust的*Content-Type*请求头匹配映射中设置的消费类型，才会由mapping映射处理该request。
```
@Controller
@RequestMapping(value = "/pets", method = RequestMethod.POST, consumes="application/json")
public void addPet(@RequestBody Pet pet, Model model) {
    // implementation omitted
}
```

消费类型可以使用非!运算表达式，*!text/plain*，意思是除了text/plain类型，其他所有的类型都可以匹配。

消费类型条件支持方法映射中配置，也支持类映射中配置。一般情况下，在类注解和方法注解中配置了条件，方法注解中的条件将会覆盖类注解中的条件，但是，消费类型条件是继承、扩展。

<h5 id='mvc-ann-requestmapping-produces'>生产媒体类型</h5>
和消费媒体类型差不多。如果*Accept *reqeust header匹配了配置的生产媒体类型，则@RequestMapping处理request。
```java
@Controller
@RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET, produces="application/json")
@ResponseBody
public Pet getPet(@PathVariable String petId, Model model) {
    // implementation omitted
}
```
非运算符也支持。
方法注解中的生产媒体类型配置也是I扩展类注解中生产媒体配置。 

<h5 id='mvc-ann-requestmapping-params-and-headers'>Reqeust参数和Header Value</h5>
使用参数条件，可reqeust参数匹配更精准，比如`"myParam","!myParam",或者"myParam=myValue`,第一二个是检查存在/不存在，第三个是为了检查是否为指定值。参看样例：
```java
@Controller
@RequestMapping("/owners/{ownerId}")
public class RelativePathUriTemplateController {

    @RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET, params="myParam=myValue")
    public void findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {
        // implementation omitted
    }

}
```

对于request header的存在/不存在，或者匹配是否为指定的值也可使用类似的方法:
```java
@Controller
@RequestMapping("/owners/{ownerId}")
public class RelativePathUriTemplateController {

    @RequestMapping(value = "/pets", method = RequestMethod.GET, headers="myHeader=myValue")
    public void findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {
        // implementation omitted
    }

}
```

![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/tip.png)
> 虽然可以用request header检查*Content-Type*和*Accept header*的值（比如，"content-type=text/*"匹配"text/plain"和"text/html"），但是推荐使用*consumes*和*produces*条件检查媒体类型相关header value，媒体类型条件检查就是为了干这个用的。

<h4 id='mvc-ann-methods'>定义@RequestMapping 处理方法</h4>
`@RequestMapping`方法非常灵活，几乎不受任何限制。支持的方法参数和返回值类型，在下面详述。除`BindingResult `类型参数外，大多数参数次序随意。
![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/tip.png)
> Spring 3.1引入了一组`@RequestMapping`方法的支持类，分别是`RequestMappingHandlerMapping`和`RequestMappingHandlerAdapter`。

<h5 id='mvc-ann-arguments'>支持的方法参数类型</h5>
系列是支持的方法参数
* ServletAPI中的Rquest或者Response对象。比如`ServletRequest`或者`HttpServletReqeust`。
* Session 对象：比如`HttpSession`。此类型的参数将会注入响应的session，因此，此参数永远不为null。

![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/tip.png)
> Session访问也许不是线程安全的，尤其是在Servlet花逆境中。加入允许多个Request可并发的访问session,可考虑使用`RequestMappingHandlerAdapter`的"synchronizeOnSession"属性为"true"

* `org.springframework.web.context.request.WebRequest`或者`org.springframework.web.context.request.NativeWebRequest`
* org.springframework.web.context.request.WebRequest or org.springframework.web.context.request.NativeWebRequest. Allows for generic request parameter access as well as request/session attribute access, without ties to the native Servlet/Portlet API.
* java.util.Locale for the current request locale, determined by the most specific locale resolver available, in effect, the configured LocaleResolver / LocaleContextResolver in an MVC environment.
* java.util.TimeZone (Java 6+) / java.time.ZoneId (on Java 8) for the time zone associated with the current request, as determined by a LocaleContextResolver.
* java.io.InputStream / java.io.Reader for access to the request’s content. This value is the raw InputStream/Reader as exposed by the Servlet API.
* java.io.OutputStream / java.io.Writer for generating the response’s content. This value is the raw OutputStream/Writer as exposed by the Servlet API.
* org.springframework.http.HttpMethod for the HTTP request method.
* java.security.Principal containing the currently authenticated user.
* @PathVariable annotated parameters for access to URI template variables. See the section called “URI Template Patterns”.
* @MatrixVariable annotated parameters for access to name-value pairs located in URI path segments. See the section called “Matrix Variables”.
* @RequestParam annotated parameters for access to specific Servlet request parameters. Parameter values are converted to the declared method argument type. See the section called “Binding request parameters to method parameters with @RequestParam”.
* @RequestHeader annotated parameters for access to specific Servlet request HTTP headers. Parameter values are converted to the declared method argument type. See the section called “Mapping request header attributes with the @RequestHeader annotation”.
* @RequestBody annotated parameters for access to the HTTP request body. Parameter values are converted to the declared method argument type using HttpMessageConverters. See the section called “Mapping the request body with the @RequestBody annotation”.
* @RequestPart annotated parameters for access to the content of a "multipart/form-data" request part. See Section 17.10.5, “Handling a file upload request from programmatic clients” and Section 17.10, “Spring’s multipart (file upload) support”.
* HttpEntity<?> parameters for access to the Servlet request HTTP headers and contents. The request stream will be converted to the entity body using HttpMessageConverters. See the section called “Using HttpEntity”.
* java.util.Map / org.springframework.ui.Model / org.springframework.ui.ModelMap for enriching the implicit model that is exposed to the web view.
* org.springframework.web.servlet.mvc.support.RedirectAttributes to specify the exact set of attributes to use in case of a redirect and also to add flash attributes (attributes stored temporarily on the server-side to make them available to the request after the redirect). RedirectAttributes is used instead of the implicit model if the method returns a "redirect:" prefixed view name or RedirectView.
* Command or form objects to bind request parameters to bean properties (via setters) or directly to fields, with customizable type conversion, depending on @InitBinder methods and/or the HandlerAdapter configuration. See the webBindingInitializer property on RequestMappingHandlerAdapter. Such command objects along with their validation results will be exposed as model attributes by default, using the command class class name - e.g. model attribute "orderAddress" for a command object of type "some.package.OrderAddress". The ModelAttribute annotation can be used on a method argument to customize the model attribute name used.
* org.springframework.validation.Errors / org.springframework.validation.BindingResult validation results for a preceding command or form object (the immediately preceding method argument).
* org.springframework.web.bind.support.SessionStatus status handle for marking form processing as complete, which triggers the cleanup of session attributes that have been indicated by the @SessionAttributes annotation at the handler type level.
* org.springframework.web.util.UriComponentsBuilder a builder for preparing a URL relative to the current request’s host, port, scheme, context path, and the literal part of the servlet mapping.


The Errors or BindingResult parameters have to follow the model object that is being bound immediately as the method signature might have more that one model object and Spring will create a separate BindingResult instance for each of them so the following sample won’t work:

**Invalid ordering of BindingResult and @ModelAttribute. **
```java
@RequestMapping(method = RequestMethod.POST)
public String processSubmit(@ModelAttribute("pet") Pet pet, Model model, BindingResult result) { ... }
```

Note, that there is a Model parameter in between Pet and BindingResult. To get this working you have to reorder the parameters as follows:
```java
@RequestMapping(method = RequestMethod.POST)
public String processSubmit(@ModelAttribute("pet") Pet pet, BindingResult result, Model model) { ... }
```

![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/note.png)
>JDK 1.8’s java.util.Optional is supported as a method parameter type with annotations that have a required attribute (e.g. @RequestParam, @RequestHeader, etc. The use of java.util.Optional in those cases is equivalent to having required=false.

<h5 id='mvc-ann-return-types'>支持的返回值类型</h5>
支持下列返回值:
* A ModelAndView object, with the model implicitly enriched with command objects and the results of @ModelAttribute annotated reference data accessor methods.
* A Model object, with the view name implicitly determined through a RequestToViewNameTranslator and the model implicitly enriched with command objects and the results of @ModelAttribute annotated reference data accessor methods.
* A Map object for exposing a model, with the view name implicitly determined through a RequestToViewNameTranslator and the model implicitly enriched with command objects and the results of @ModelAttribute annotated reference data accessor methods.
* A View object, with the model implicitly determined through command objects and @ModelAttribute annotated reference data accessor methods. The handler method may also programmatically enrich the model by declaring a Model argument (see above).
* A String value that is interpreted as the logical view name, with the model implicitly determined through command objects and @ModelAttribute annotated reference data accessor methods. The handler method may also programmatically enrich the model by declaring a Model argument (see above).
* void if the method handles the response itself (by writing the response content directly, declaring an argument of type ServletResponse / HttpServletResponse for that purpose) or if the view name is supposed to be implicitly determined through a RequestToViewNameTranslator (not declaring a response argument in the handler method signature).
* If the method is annotated with @ResponseBody, the return type is written to the response HTTP body. The return value will be converted to the declared method argument type using HttpMessageConverters. See the section called “Mapping the response body with the @ResponseBody annotation”.
* An HttpEntity<?> or ResponseEntity<?> object to provide access to the Servlet response HTTP headers and contents. The entity body will be converted to the response stream using HttpMessageConverters. See the section called “Using HttpEntity”.
* An HttpHeaders object to return a response with no body.
* A Callable<?> can be returned when the application wants to produce the return value asynchronously in a thread managed by Spring MVC.
* A DeferredResult<?> can be returned when the application wants to produce the return value from a thread of its own choosing.
* A ListenableFuture<?> can be returned when the application wants to produce the return value from a thread of its own choosing.
* Any other return type is considered to be a single model attribute to be exposed to the view, using the attribute name specified through @ModelAttribute at the method level (or the default attribute name based on the return type class name). The model is implicitly enriched with command objects and the results of @ModelAttribute annotated reference data accessor methods.

<h5 id='mvc-ann-requestparam'>绑定request参数到方法参数上</h5>
`@RequestParam`注解可以绑定request参数到方法参数上。
看样例:
```java
@Controller
@RequestMapping("/pets")
@SessionAttributes("pet")
public class EditPetForm {

    // ...

    @RequestMapping(method = RequestMethod.GET)
    public String setupForm(@RequestParam("petId") int petId, ModelMap model) {
        Pet pet = this.clinic.loadPet(petId);
        model.addAttribute("pet", pet);
        return "petForm";
    }

    // ...

}
```
使用次注解的参数，默认是必须的，但是可以设置参数为可选，设置`@RequestParam`的`required`属性为`false`（比如：`@RequestParam(value="id",required=false)`）。

如果方法参数类型不是`String`，那么Spring将会自动进行类型转换。详情参看[the section called “Method Parameters And Type Conversion”](#mvc-ann-typeconversion).

当`@RequestParam`注解用在`Map<String,String>`或者`MultiValueMap<String,Strng`参数上，那么所有的reqeust 参数都将会绑定到map中。

<h5 id='mvc-ann-requestbody'>使用注解@RequestBody映射request body</h5>
`@RequestBody`方法参数注解表名，HTTP request body绑定到方法参数值上，看样例:
```java
@RequestMapping(value = "/something", method = RequestMethod.PUT)
public void handle(@RequestBody String body, Writer writer) throws IOException {
    writer.write(body);
}
```

可以使用`HttpMessageConverter`转换request body到方法参数上，`HttpMessageConverter `负责将HTTP reqeust消息转换成为对象，也负责将对象转换成HTTP response body。`RequestMappingHandlerAdapter `支持`@RequestBody`注解和下列默认的`HttpMessageConverters`

* `ByteArrayHttpMessageConverter `转换字节数组
* `StringHttpMessageConverter `转换字串
* `FormHttpMessageConverter`负责form表单数据与MultiValueMap<String,String>之间的转换
* `SourceHttpMessageConverter`负责XML与Source之间的互相转换

详情请参看[Message Converters](#rest-message-conversion)。同事也得注意，如果使用了MVC命名空间或者MVC Java config，默认会注册很多message converter。详情参看[Section 20.16.1, “Enabling the MVC Java Config or the MVC XML Namespace”](#mvc-config-enable) 

若需要读写XML，得使用`MarshallingHttpMessageConverter `配置指定的`Marshaller `和`Unmarshaller`实现，详情参看`org.springframework.oxm`包。 看样例代码
```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <util:list id="beanList">
            <ref bean="stringHttpMessageConverter"/>
            <ref bean="marshallingHttpMessageConverter"/>
        </util:list>
    </property
</bean>

<bean id="stringHttpMessageConverter"
        class="org.springframework.http.converter.StringHttpMessageConverter"/>

<bean id="marshallingHttpMessageConverter"
        class="org.springframework.http.converter.xml.MarshallingHttpMessageConverter">
    <property name="marshaller" ref="castorMarshaller" />
    <property name="unmarshaller" ref="castorMarshaller" />
</bean>

<bean id="castorMarshaller" class="org.springframework.oxm.castor.CastorMarshaller"/>
```

`@RequestBody`方法参数也能被注解为`@Valid`，这样，他就会被配置的`Validator `实例校验。Spring MVC会自动配置一个JSR-303 validator。

就像`@ModielAttribute`参数，`Errors`参数可用于校验errors。如果未声明该参数，则会抛出`MethodArgumentNotValidException `异常。异常有`DefaultHandlerExceptionResolver`处理，它会发送`400`error给客户端。

![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/note.png)
> 通过MVC命名空间或者MVC java config配置message转换和配置validator，详情请参看[Section 17.16.1, “Enabling the MVC Java Config or the MVC XML Namespace”](#mvc-config-enable) 

<h5 id='mvc-ann-responsebody'>通过@ResponseBody注解映射response body</h5>
`@ResponseBody`注解与`@RequestBody`相似。该注解是一个方法注解，作用是将返回值直接写入HTTP response流。(既不是Model,也不是视图),比如:
```java
@RequestMapping(value = "/something", method = RequestMethod.PUT)
@ResponseBody
public String helloWorld() {
    return "Hello World";
}
```

上例中，将`Hello World`字串直接写入到response stream响应流中。

使用`@RequestBody`时，Spring使用`HttpMessageConverter`转换对象为response body响应体。详情参看[Message Converters](#rest-message-conversion)

<h5 id='#mvc-ann-restcontroller'>使用@RestController注解创建REST Controller</h5>
Controller实现REST API非常常用，REST 风格Ctroller仅能提供JSON、XML、自定义MediaType content媒体类型。使用`RESTController`类注解，可以非常方便的实现RESTful风格API，用来替代`@RequestMapping`和`@Responsebody`的配合。

`@RestController`是`@ResponseBody`和`@Controller`注解的组合，该注解具有扩展性，也许在将来的发布版中会增加额外的功能。

作为常规`@Controller`，`@RestController`通常配合`@ControllerAdvice`使用。详情请参看 [the section called “Advising controllers with the @ControllerAdvice annotation”](#mvc-ann-controller-advice)

<h5 id='#mvc-ann-httpentity'>使用HttpEntity</h5>
`HttpEntity`与`@RequestBody`、`@Resonsebody`非常像。除了访问request和response body，`HttpEntity`（和response专用子类`ResponseEntity`）也可以访问request和response的headers,像这样:
```java
@RequestMapping("/something")
public ResponseEntity<String> handle(HttpEntity<byte[]> requestEntity) throws UnsupportedEncodingException {
    String requestHeader = requestEntity.getHeaders().getFirst("MyRequestHeader"));
    byte[] requestBody = requestEntity.getBody();

    // 用request header和 body干一些事儿

    HttpHeaders responseHeaders = new HttpHeaders();
    responseHeaders.set("MyResponseHeader", "MyValue");
    return new ResponseEntity<String>("Hello World", responseHeaders, HttpStatus.CREATED);
}
```

上面的样例中获取了request header中`MyRequestHeader`的值，读取body置入一个byte array数组。将`MyResponseHeader `设置如了response，将字串`Hello World`写入的response流中，并设置了response的status code为201(Created)

如同`@RequestBody`和`@ResponseBody`，Spring使用`HttpMessageConverter `进行request和response流之间转换。详情请参看[Message Converters.](#rest-message-conversion)

<h5 id='mvc-ann-modelattrib-methods'>在方法上使用@ModelAttribute</h5>
`@ModelAttribute`注解可以用在方法上或者在方法参数上。本章将方法上注解的用法，下章讲解方法参数上注解的用法。
方法上注解`@ModelAttribute`则表示方法的目的是增加一个或者多个model attributes。所有`@RequestMapping`注解方法支持的参数类型，`@ModelAttribute`注解的方法都支持，但是`@ModelAttribute`方法不能直接从request映射。在同一controller中`ModelAttribute`方法会在`@RequestMapping`方法之前调用。看看它的用法
```java
// 增加单个 attribute
//方法的返回值将添加到 model中，key键是"account"
//也可以自定义key键，如此@ModelAttribute("myAccount")

@ModelAttribute
public Account addAccount(@RequestParam String number) {
    return accountManager.findAccount(number);
}

// 添加多个 attributes

@ModelAttribute
public void populateModel(@RequestParam String number, Model model) {
    model.addAttribute(accountManager.findAccount(number));
    // add more ...
}
```

`@ModelAttribute`方法主要的应用场景，将常用的attribute属性填充到model模型中，比如填充下拉菜单项、宠物类型、或者检索常用对象（像Account）用于在HTML表单中描述数据。后面的用法在下章中讨论。

`@ModelAttribute`注解方法有2中写法，第一种，方法通过返回值给model增加了一个attribute，,第二种，方法接收一个`Model`并且增加了多个model attribute。根据实际情况选择使用。

单个Controller可以有多个`@ModelAttribute`方法。在同一controller中的`@RequestMapping`方法调用之前，会执行所有的`@ModelAtrribute`方法。

`@ModelAttribute`方法也可以定义在`@ControllerAdvice`注解的类中，这样的方法则会应用于多个controller。详情才看 [the section called “Advising controllers with the `@ControllerAdvice` annotation”](#mvc-ann-controller-advice) 

![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/tip.png)
> 若未明确指定model attribute name时候，Spring会怎么干？这种情况下，默认会基于其类型指定其name。比如，如果方法返回的对象类型为`Account`，默认名字是`account`。可以通过指定`@ModelAttribute`注解的value来为其指定name。如果直接给`Model`添加attributes，得使用合适的重载方法`addAttribute(..)`，也就是有或者没有指定attribute name。

`@ModelAtribute`注解也可以在`@RequestMapping`方法上使用。这种情况下，`@RequestMapping`方法返回值将会作为model attribute，而不是作为视图name。视图name来自于视图name命名惯例，就像是void方法。详情参看[Section 17.13.3, “The View - RequestToViewNameTranslator”.](#mvc-coc-r2vnt)

<h5 id='mvc-ann-modelattrib-method-args'>在方法参数上使用@ModelAttribute</h5>
前面提到`@ModelAttribute`可用于方法，也可用于方法参数上。本章讲解在方法参数上的用法。

在方法参数上注解`@ModelAttribute`则表明参数应该从model中检索出。 若model中不存在，参数将会被实例化并增加到model中。若存在于model中，Spring将会给参数的field域赋值，值来自于从request中解析出相匹配的值。这就是Spring MVC 中的数据绑定，它是非常有用的机制，将使你免于手动从form field表单域中逐一的取值。
```java
@RequestMapping(value="/owners/{ownerId}/pets/{petId}/edit", method = RequestMethod.POST)
public String processSubmit(@ModelAttribute Pet pet) { }
```
上例中Pet的实例从哪儿来?有以下来源:
* 或许使用了`@SessionAttributes`，则Pet实例已经在model中了。详情参看[the section called “Using @SessionAttributes to store model attributes in the HTTP session between requests”](#mvc-ann-sessionattrib)
* 或许在同一个controller中使用了`@ModelAttribute`注解的方法，所以Pet实例已经存在于model中了，详情参看上一章
* 来自于Spring数据绑定，使用URI模板变量配合转换器方式获取，下面详细讲述。
* 通过Pet空构造获取实例

`@ModelAttribute`注解方法是从数据库检索attribute的常用手段，也可以使用`@SessionAttribute`将数据存储于session中。在某些场景中，可以非常方便的通过URI template variable模板变量配合类型转换器的方式来检索attribute。看样例:
```java
@RequestMapping(value="/accounts/{account}", method = RequestMethod.PUT)
public String save(@ModelAttribute("account") Account account) {

}
```
model attribute的name(也就是"account")匹配URI template variable模板变量。如果你注册了`Converter<String, Account>`，该转换器的实现能把`String字串`accout变成`Account`实例（*译注，比如接收一个主键，从数据库中根据主键查出数据置入Account类实例*），那么上例中的方法将不需要`@ModelAttribute`注解也可以工作。

接下来是数据绑定。`WebDataBinder`类解析request中参数的名字(包括query字串参数和form field表单域)，并通过name匹配model attribute的field域。经过必要的类型转换之后(将String转换成目标域类型)，匹配的域将会赋值。数据绑定和验证在[Chapter 7, Validation, Data Binding, and Type Conversion](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/validation.html)讲解。controller级别的自定义数据绑定处理在[the section called “Customizing WebDataBinder initialization”.](#mvc-ann-webdatabinder)讲解。

数据绑定处理时候，也许会报错，比如必须的field域未找到、类型转换错误等。若要检查这些错误，可在`@ModelAttribute`参数后紧接着增加一个`BindingResult`参数。
```java
@RequestMapping(value="/owners/{ownerId}/pets/{petId}/edit", method = RequestMethod.POST)
public String processSubmit(@ModelAttribute("pet") Pet pet, BindingResult result) {

    if (result.hasErrors()) {
        return "petForm";
    }

    // ...

}
```

使用`BindingResult `时，可以探测到error，当发现error时，通常会跳转到统一的视图，即异常页，Spring的`<errors>`form 标签可以渲染此类信息。

在数据绑定处理时，可以使用同一个`BindingResult`调用自定义的validator校验器,`BindingResult `用于记录数据绑定error。数据绑定和校验errors能够在同一个地方累计然后一并返回给user:
```java
@RequestMapping(value="/owners/{ownerId}/pets/{petId}/edit", method = RequestMethod.POST)
public String processSubmit(@ModelAttribute("pet") Pet pet, BindingResult result) {

    new PetValidator().validate(pet, result);
    if (result.hasErrors()) {
        return "petForm";
    }

    // ...

}
```
或者，还可以使用JSR-303`@Valid`注解自动校验:
```java
@RequestMapping(value="/owners/{ownerId}/pets/{petId}/edit", method = RequestMethod.POST)
public String processSubmit(@Valid @ModelAttribute("pet") Pet pet, BindingResult result) {

    if (result.hasErrors()) {
        return "petForm";
    }

    // ...

}
```

详情参看[See Section 7.8, “Spring Validation”](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/validation.html#validation-beanvalidation) and [Chapter 7, Validation, Data Binding, and Type Conversion](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/validation.html)

<h5 id='mvc-ann-sessionattrib'>使用@SessionAttribute在HTTP会话中存储model attribute</h5>
类注解`@SessionAttributes`声明用于指定handler的session attributes。一般情况下，声明用于存储在session中的model attribute的name或type，用于在接下来的request中传递数据。

看样例，指定一个model attribute name:
```java
@Controller
@RequestMapping("/editPet.do")
@SessionAttributes("pet")
public class EditPetForm {
    // ...
}
```

<h5 id='mvc-ann-redirect-attributes'>指定redirect和flash属性</h5>
默认情况下，在redirect URL中，所有的model attribute都应该作为URI template variable模板变量暴露。上下的属性，包括原始类型、原始类型的集合/数组自动拼接到query 参数中。 

在controller中，也许会包含额外的attribute，主要是用于渲染的目的（比如下拉菜单）。在redirect重定向场景中，为了精准控制，可通过在`@RequestMapping`方法中声明一个`RedirectAttribute`类型的参数，使用该参数在`RedirectView`中增加attribute。此时，若controller方法发生redirect重定向，`RedirectAttributes`的内容就会被使用，否则，将使用`Model`的内容。

`RequestMappingHandlerAdapter `提供一个标志位`"ignoreDefaultModelOnRedirect"`，表示默认的`Model`内容在controller 方法redirect重定向时不会被使用。同时，controller方法应该声明一个`RedirectAttributes`类型的attribute，否则，将没有attribute传递给`RedirecView`。MVC xml配置和MVC Java config中，该值默认都是`false`，这是为了向后兼容。当然了，新的应用中还是推荐设置为`true`。

`RedirectAttributes `接口也可用于flash attribute。不像其他redirect属性那样，拼接到redirect URL后，flash attribute会保存在HTTP session中（因此无需再URL中出现）。当flash attribute从session中移除时候，controller的model模型将为目标redirect URL自动接收这些flash attribute。详情参看 [Section 17.6, “Using flash attributes” ](#mvc-flash-attributes)

<h5 id='mvc-ann-form-urlencoded-data'>使用"application/x-www-form-urlencoded"数据</h5>
之前讲解了`@ModelAttribute`，用于支持从浏览器客户端发出表单提交request请求。它也支持非浏览器提交的request。当使用HTTP PUT request请求，情况则大不相同。浏览器可通过HTTP GET或者HTTP POST提交表单数据。非浏览器客户端则可通过HTTP PUT提交表单数据。因为Servlet规范要求`ServletRequest.getParameter*()`系列方法只能通过HTTP POST访问表单数据，而不是HTTP PUT，呃，处理起来有点困难。

为了支持HTTP PUT和PATCH request请求，`spring-web`模块提供了`HttpPutFormContentFilter`过滤器，在`web.xml`中配置
```xml
<filter>
    <filter-name>httpPutFormFilter</filter-name>
    <filter-class>org.springframework.web.filter.HttpPutFormContentFilter</filter-class>
</filter>

<filter-mapping>
    <filter-name>httpPutFormFilter</filter-name>
    <servlet-name>dispatcherServlet</servlet-name>
</filter-mapping>

<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
```

上面的filter拦截了内容类型为`application/x-form-urlencoded`的HTTP/PUT/PATCH的request，从request body读取form数据，并且封装`ServletRequest`，这样可以通过`ServletRequest.getParameter*()`系列方法解析form数据。

[http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/note.png](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/note.png)
> 由于`HttpPutFormContentFilter `消费了request body，所以不应该在给PUT和PATCH的URL配置处理器，比如依赖消费媒体类型`application/x-www-form-urlencoded`配置的转换器。它包括了`@RequestBody MultiValueMap<String, String> and HttpEntity<MultiValueMap<String, String>>`

<h5 id='mvc-ann-cookievalue'>使用@CookieValue注解映射cookie值</h5>
`@CookieValue`注解作用是将HTTP cookie值绑定到方法参数上。
假设，有一HTTP request携带了以下cookie
JSESSIONID=415A4AC178C59DACE0B2C9CA727CDD84
如何使用该注解绑定cookie值呢:
```java
@RequestMapping("/displayHeaderInfo.do")
public void displayHeaderInfo(@CookieValue("JSESSIONID") String cookie) {
    //...
}
```
若方法参数类型不是`String`，则会自动进行类型转换。 See[ the section called “Method Parameters And Type Conversion”.
](#mvc-ann-typeconversion)

在Servlet和Portlet环境中的注解handler处理器方法上支持此注解

<h5 id='mvc-ann-requestheader'>@RequestHeader注解绑定header属性</h5>
`@RequestHeader`注解作用是将request header绑定到方法参数上。

现在有一个request header:
Host                    localhost:8080
Accept                  text/html,application/xhtml+xml,application/xml;q=0.9
Accept-Language         fr,en-gb;q=0.7,en;q=0.3
Accept-Encoding         gzip,deflate
Accept-Charset          ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive              300

接下来看看如何获取`Accept-Encoding`和`Keep-Alive` headers:
```java
@RequestMapping("/displayHeaderInfo.do")
public void displayHeaderInfo(@RequestHeader("Accept-Encoding") String encoding,
        @RequestHeader("Keep-Alive") long keepAlive) {
    //...
}
```
若方法参数类型不是`String`，则会自动进行类型转换。 See[ the section called “Method Parameters And Type Conversion”.
](#mvc-ann-typeconversion)
![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/tip.png)
>Spring MVC 类型转换支持逗号分隔的字串转为array/List。 比如使用`@RequestHeader("Accept")`注解的方法参数不仅可以`String`类型，也可以是`String[]`或者`List<String`


在Servlet和Portlet环境中的注解handler处理器方法上支持此注解

<h5 id='mvc-ann-typeconversion'>方法参数和类型转换</h5>
从request parameters参数,path variable路径变量，request header头，cookie value饼干值中解析出的字串值，也许需要类型转换，转换相对应的绑定的方法参数的类型、或者是域类型(比如，将request parameter构造为`@ModelAttribute`)。如果需要绑定的对象类型不是	`String`，Spring会自动进行类型转换。支持所有的简单类型的转换，比如int,long,Date等等。可以通过`WebDataBinder`更进一步的自定义转换过程(see the [section called “Customizing WebDataBinder initialization](#mvc-ann-webdatabinder)”)，或者是通过使用`FormattingConversionService `注册`Formatters`（详情参看"[Spring Field Formatting](#format)"）

<h5 id='mvc-ann-webdatabinder'>自定义WebDataBinder 初始化</h5>
使用Spring的`WebDataBinder`自定义request parameter参数绑定到PropertyEditors,可以在controller内方法上使用`@InitBinder`注解，或者是在`@ControllerAdvice`注解的类上使用`@InitBinder`注解方法,或者是提供自定义`WebBindingInitializer`。详情参看 [“Advising controllers with the `@ControllerAdvice` annotation”](#mvc-ann-controller-advice) section for more details.

<h5 id='mvc-ann-initbinder'>使用@InitBinder自定义数据绑定</h5>
使用`@InitBinder`注解controller的方法是一种在controller类内直接配置数据绑定的方式。`@InitBinder`注解的方法会初始化`WebDataBinder`，该`WebDataBinder`用于数据绑定，将从request解析出来的值填充到相应的绑定对象上。这种init-binder方法不支持command/form 命令/表单对象和相应的验证结果对象，其他的`@Requestmapping`方法的所有的参数都支持。init-binder方法一定不能有返回值，即void方法。通常，参数包括`WebDataBinder`，该参数可以组合`WebRequest`或者`Java.util.Locale`，允许注册context-specific editors上下文编辑器。

下例中使用`@InitBinder`为所有的`Java.util.Date`表单属性配置了`CustomeDateEditor`:
```java
@Controller
public class MyFormController {

    @InitBinder
    public void initBinder(WebDataBinder binder) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
        dateFormat.setLenient(false);
        binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, false));
    }

    // ...

}
```

<h5 id='mvc-ann-webbindinginitializer'>配置自定义WebBindingInitializer</h5>
为了更具体的控制数据绑定初始化，可以提供一个自定义的`WebBindingInitializer`接口实现，该实现设置给一个自定义bean`AnnotationMethodHandlerAdapter`，覆盖其默认配置。

下面的样例摘子PetClinic 应用，讲解如何使用自定义的`WebBindingInitializer`接口实现做配置，`org.springframework.samples.petclinic.web.ClinicBindingInitializer`类配置了一些PetClinic controller需要使用的PropertyEditors

```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="cacheSeconds" value="0" />
    <property name="webBindingInitializer">
        <bean class="org.springframework.samples.petclinic.web.ClinicBindingInitializer" />
    </property>
</bean>
```

`@InitBinder`方法也可以在`@ControllerAdvice`注解的类中定义，在这种情况下，`@InitBinder`会应用于所有匹配上的controller。这是`WebBindingInitializer`的另一个应用场景。详情参看 [the section called “Advising controllers with the @ControllerAdvice annotation” section for more details](#mvc-ann-controller-advice).

<h5 id='mvc-ann-lastmodified'>支持Response Header Last-Modified以实现缓存</h5>
`@RequestMapping`也许是想要支持HTTP Request的头`Last-Modified`，因为在Servlet API中定义了`getLastModified`方法，以便缓存内容。它会计算request的latModified最后编辑的`long`值，与Request Header属性`If-Modified-Since`做对比，如果为发生变化，则会返回304。参看代码:
```java
@RequestMapping
public String myHandleMethod(WebRequest webRequest, Model model) {

    long lastModified = // 1. 应用指定的最后编辑时间

    if (request.checkNotModified(lastModified)) {
        // 2. 如果在request在有效期内，则直接返回，无需处理
        return null;
    }

    // 3. 显然已经过期了，此时执行真正的业务逻辑，为视图准备数据
    model.addAttribute(...);
    return "myViewName";
}

```

有两点要注意:`request.checkNotModified(lastModified)`和返回`null`。`request.checkNotModified(lastModified)`在返回`true`之前将response状态设置为304。后者，返回`null`值，配合前者，会使Spring MVC不对request做进一步处理。

<h5 id='mvc-ann-controller-advice'>使用`@ControllerAdvice`注解增强controllers</h5>
`@ControllerAdvice`注解允许实现类通过classpath扫描方式自动探测。当使用MVC命名空间（即XML配置）或者MVC Java config时候，它会自动开启。
`@ControllerAdvice`注解的类，能包含`@ExceptionHandler, @InitBinder, and @ModelAttribute`注解的方法，这些方法将会应用于所有匹配上的controller.

`@ControllerAdvic`也可以指定声明应用范围
```java
// 应用于所有使用了 @RestController注解的controller
@ControllerAdvice(annotations = RestController.class)
public class AnnotationAdvice {}

//应用于指定包内的所有controller
@ControllerAdvice("org.example.controllers")
public class BasePackageAdvice {}

// 应用于指定的类
@ControllerAdvice(assignableTypes = {ControllerInterface.class, AbstractController.class})
public class AssignableTypesAdvice {}
```
更多详情请参看[java doc文档](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html)

<h5 id='mvc-ann-jsonview'>Jackson 序列化视图的支持</h5>
有时需要将对象序列化成HTTP response body。Spring支持此功能，内置了JSON视图[Jackson's Serialization Views](http://wiki.fasterxml.com/JacksonJsonViews)就是干这个用的。

JSON视图需要在controler的方法上增加`@ResponseBody`注解或者令该方法返回`ResponseEntity`类型值，`@JsonView`用法非常简单，只需设置该注解的一个参数用于指定view class或者view 接口即可。
```java
@RestController
public class UserController {

    @RequestMapping(value = "/user", method = RequestMethod.GET)
    @JsonView(User.WithoutPasswordView.class)
    public User getUser() {
        return new User("eric", "7!jd#h23");
    }
}

public class User {

    public interface WithoutPasswordView {};
    public interface WithPasswordView extends WithoutPasswordView {};

    private String username;
    private String password;

    public User() {
    }

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    @JsonView(WithoutPasswordView.class)
    public String getUsername() {
        return this.username;
    }

    @JsonView(WithPasswordView.class)
    public String getPassword() {
        return this.password;
    }
}
```

![](http://docs.spring.io/autorepo/docs/spring/current/spring-framework-reference/html/images/note.png)
> 虽然`@JsonView`可以指定多个类，但是在controller方法上的`@JsonView`只支持指定一个类。要是非得指定多个类，可通过组合接口实现。

若方法只能返回视图解决方案？？，则只需在model中的指定视图序列化类:
```java
@Controller
public class UserController extends AbstractController {

    @RequestMapping(value = "/user", method = RequestMethod.GET)
    public String getUser(Model model) {
        model.addAttribute("user", new User("eric", "7!jd#h23"));
        model.addAttribute(JsonView.class.getName(), User.WithoutPasswordView.class);
        return "userView";
    }
}
```

<h5 id='mvc-ann-jsonp'>Jackson JSONP的支持</h5>
为了让`ResponseBody`和`ResponseEntity`方法支持[JSONP](http://en.wikipedia.org/wiki/JSONP)，需要声明一个`@ControllerAdvice`bean，该bean继承自`AbstractJsonpResponseBodyAdvice`，需要在构造参数中指明JSONP query 参数名:
```java
@ControllerAdvice
public class JsonpAdvice extends AbstractJsonpResponseBodyAdvice {

    public JsonpAdvice() {
        super("callback");
    }
}
```

若是依赖视图解决方案，当request query参数名中含有`jsonp`或者`callback`参数时，JSONP将自动开启， query参数名也可以通过`jsonpParameterNames`属性自定义。

<h4 id='mvc-ann-async'>异步请求处理</h4>
Spring MVC 3.2 introduced Servlet 3 based asynchronous request processing. Instead of returning a value, as usual, a controller method can now return a java.util.concurrent.Callable and produce the return value from a separate thread. Meanwhile the main Servlet container thread is released and allowed to process other requests. Spring MVC invokes the Callable in a separate thread with the help of a TaskExecutor and when the Callable returns, the request is dispatched back to the Servlet container to resume processing with the value returned by the Callable. Here is an example controller method:
