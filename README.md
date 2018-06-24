# ssm
## 基于Eclpise动态Web项目风格的SSM（Spring+Sping MVC+Mybatis）三大框架整合项目
### 主要流程如下：
  1. 首先浏览器上访问路径 /listCategory
  2. tomcat根据web.xml上的配置信息，拦截到了/listCategory，并将其交由DispatcherServlet处理。
  3. DispatcherServlet 根据springMVC的配置，将这次请求交由CategoryController类进行处理，所以需要进行这个类的实例化。
  4. 在实例化CategoryController的时候，注入CategoryServiceImpl。
    (自动装配实现了CategoryService接口的的实例，只有CategoryServiceImpl实现了CategoryService接口，所以就会注入CategoryServiceImpl)
  5. 在实例化CategoryServiceImpl的时候，又注入CategoryMapper。
  6. 根据ApplicationContext.xml中的配置信息，将CategoryMapper和Category.xml关联起来了。
  7. 这样拿到了实例化好了的CategoryController,并调用listCategory方法。
  8. 在listCategory方法中，访问CategoryService,并获取数据，并把数据放在"cs"上，接着服务端跳转到listCategory.jsp。
  9. 最后在listCategory.jsp 中显示数据。
### 补充几点：
  1. springMVC.xml中配置了相关语句，扫描com.how2java.controller包,以注解驱动，使得访问路径与方法的匹配可以通过注解配置。
    同时，配置相关语句视图定位到/WEB/INF/jsp这个目录下。
  2. com.how2java.test包中注释掉了之前写的单元测试和分页功能。本项目可以拓展出对Category类的添加、删除、更新、查询等功能，
    但为了简单展示，只写了查询数据库中所有Category并在页面展示的功能。
  3. 分页功能：每次只显示五条数据，增加“首页”“上一页”“下一页”“末页”选项。可以增加Page类，修改相关文件配置实现该功能。
    在本项目中使用了PageHelper插件来实现，在applicationContext.xml中增加了PageHelper插件配置。
    CategoryController在调用categoryService.list()之前，执行：PageHelper.offsetPage(page.getStart(),5)，
    并通过int total = (int) new PageInfo<>(cs).getTotal()获取总数。
