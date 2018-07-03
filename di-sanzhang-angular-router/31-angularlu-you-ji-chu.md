1．基本概念

Angular中的路由应用比较广泛，主要是允许我们通过不同的URL访问不同的内容，可实现多视图的单页web应用（single page web application，SPA）。

Angular路由功能是一个纯前端的解决方案，与我们熟悉的后台路由不太一样。后台路由是通过不同的URL会路由到不同的控制器上（controller），再渲染（render）到页面（HTML）。Angular的前端路由是提前对指定的应用（ng-app）定义路由规则（routeProvider），然后通过不同的URL告诉应用（ng-app）加载哪个页面（HTML），再渲染到应用（ng-app）视图（ng- view）中。AngularJS的前端路由虽然URL输入不一样，页面展示不一样，但它实际是完成的单页应用（ng-app）视图（ng-view）的局部刷新。

例如我们的URL通常形式为http://jtjds.cn/first/page，但在单页web应用中angular通过\#+标记实现，即如下所示：

http://192.168.1.109:8000/main.html\#/page1

http://192.168.1.109:8000/main.html\#/page2

http://192.168.1.109:8000/ main.html\#/page3

当我们点击以上任一一个链接时，向服务器请求的地址都是http://192.168.1.109:8000/main.htm，而\#号后的内容在向服务器端请求时会被浏览器忽略掉，所以我们在客户端实现\#号后面的功能实现即可。简单来说，路由通过\#＋标记帮助我们区分不同逻辑页面，并将其绑定到对应的控制器上。其原理图如图3.1所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image001.png)![](/assets/3a.JPG)

图3.1路由原理图



2．路由相关对象介绍



表3-1路由对象及功能

| **名称** | **功能** |
| :--- | :--- |
| Routes | 路由配置，保存着哪个URL对应展示哪个组件，以及在哪个RouterOutlet中展示组件 |
| RouterOutlet | 在Html中标记路由内容呈现位置的点位符指令 |
| Router | 负责在运行时执行路由的对象，可以通过调用其navigate（）和navigateByUrl（）方法来导航到一个指定的路由 |
| RouterLink | 在Html中声明路由导航用的指令 |
| ActivateRoute | 当前激活的路由对象，保存着当前路由的信息，如路由地址、路由参数等 |

表3-1中的5个路由对象在angular应用中的位置如图3.2所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image003.jpg)![](/assets/32.JPG)

图3.2路由对象在Angular应用中的位置

每个angular应用都是由多个组件组成的，比如图3.2中的应用是由AppComponent、A和B组件组成的，每一个组件都有一个模板和一个控制器，应用启动后，首先展现AppComponent组件的模板，所有的组件都被封装在一个模块里，而路由配置即Routes就是存在于模块中的，Routes是由一组配置信息所组成的，每个配置信息至少包含下面两个属性：

lpath：用来指明浏览器中的URL；

lcomponent：用来指明相应的组件。

图3.2中当浏览器中的地址为/user时就展现组件A，当浏览器中的地址为/order时就展现组件B。通常在AppComponent组件的模板中会包含多个div，当浏览器中的地址为/user时，组件A的内容就会展现在AppComponent组件的模板中的RouterOutlet位置上（即在AppComponent模板中使用RouterOutlet指令来指定组件A的位置）。若此时要展现组件B的内容，那么此时可以在AppComponent模板通过RouteLink指令指定一个链接来改变浏览器上的地址。另外也可以在控制器中调用Router对象的navigate（）方法来改变浏览器中的地址从而实现路由的转换，最后可以在路由时通过URL来传递一些数据（比如在path属性的/user后加一个？name=\*\*\*），此时这些数据就会保存在ActivatedRoute对象中。比如说从组件A路由到组件B时，可以通过组件B中的ActivatedRoute对象来获取URL中携带的参数。下面用一个简单的例子来说明路由配置在angular中的应用。展示效果如图3.3所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image004.png)![](/assets/33.JPG)

图3.3路由配置示例运行效果

