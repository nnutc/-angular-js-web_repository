1．创建带路由的项目

（1）单击开始→所有程序→Node.js→Node.js command prompt，进入Node.js命令行；

（2）把当前目录改为项目保存位置（本例中是E:\code\web），然后输入new MyRouter –routing命令，此时会自动生成带有Router依赖包的项目。

此时，在项目src/app文件夹下多生成一个app-routing.module.ts路由配置文件，同时在app.component.html模板文件最下方添加了“&lt;router-outlet&gt;&lt;/router-outlet&gt;”这一行代码，该行代码表示导航到某个路由时，指定的组件在主页中显示的位置。

2．路由的基本配置

为了展现如图3.3所示效果，此时需要创建home组件（用于显示home主页信息）和product商品信息（用于显示product商品信息），接着修改app-routing.module.ts文件配置路由，该文件配置后的详细代码如下：

import { NgModule } from '@angular/core';

import { Routes, RouterModule } from '@angular/router';

import {HomeComponent} from './home/home.component';

import {ProductComponent} from './product/product.component';

const routes: Routes = \[

 {path: '', component: HomeComponent, children: \[\] },

 {path: 'product', component: ProductComponent, children: \[\]}

\];

@NgModule\({

 imports: \[RouterModule.forRoot\(routes\)\],

 exports: \[RouterModule\]

}\)

export class AppRoutingModule { }

第5-8行就是配置的路由信息，路由信息通常包含两个属性，本例中第6行“path:’’”表示url为“/”即访问地址为http://localhost:4200/时，显示HomeComponent组件内容；第7行表示url为“/product”即访问地址为http://localhost:4200/product时，显示ProductComponent组件内容；children表示子路由，本例不涉及，后面再详细介绍。

最后在html中用routerLink指令声明路由导航，即本例中修改app.component.html文件，其详细代码如下：

&lt;a \[routerLink\]="\['/'\]"&gt;home主页&lt;/a&gt;

&lt;a \[routerLink\]="\['/product'\]"&gt;product商品信息&lt;/a&gt;

&lt;router-outlet&gt;&lt;/router-outlet&gt;

除了使用routerLink指令来指明路由导航外，还有一种利用Router的navigate（）和navigateByUrl（）方法来导航。下面用一个按钮单击事件来举例说明，即**数据绑定中的事件绑定**来实现：

（1）修改app.component.html文件，其详细代码如下：

&lt;a \[routerLink\]="\['/'\]"&gt;home主页&lt;/a&gt;

&lt;a \[routerLink\]="\['/product'\]"&gt;product商品信息&lt;/a&gt;

&lt;input type="button" value="商品信息" \(click\)="toProductDetails\(\)"&gt;

&lt;router-outlet&gt;&lt;/router-outlet&gt;

上述代码的第3行定义一个Button，并指明单击事件toProductDetails（），该单击事件需要在app.component.ts文件中定义，即通过控制器代码实现，其详细代码如下：

import {Component} from '@angular/core';

import {Router} from '@angular/router';

@Component\({

 selector: 'app-root',

 templateUrl: './app.component.html',

 styleUrls: \['./app.component.css'\]

}\)

export class AppComponent {

 title = 'app';

 constructor\(private router: Router\) {

 }

 toProductDetails\(\) {

 this.router.navigate\(\['/product'\]\);

 }

}

此时运行效果如图3.3右侧所示，界面上会出现一个按钮，当单击按钮时，同样会显示productComponent组件的内容。

在实际应用中，有的用户可能输入一个不存在的路径时（即不是上面的http://localhost:4200或http://localhost:4200/product），需要给用户一个明显的提示页面存在的页面。那么就需要创建一个页面不存在的组件（本例以Error404Component为例），并通过通配符来指定该页面的路由。在路由配置文件app-routing.module.ts文件中增加下面的第4行代码（该行必须写在所有路由的最后一行）：

const routes: Routes = \[

 {path: '', component: HomeComponent, children: \[\] },

 {path: 'product', component: ProductComponent, children: \[\]} ,

 {path: '\*\*', component: Error404Component, children: \[\]}

\];

3．在路由时传递数据

（1）在查询参数中传递数据

路由的路径 =&gt;路由目标组件

/product?id=1&name=2 =&gt; ActivatedRoute.queryParams\[id\]

例如，将应用组件的模板文件app.component.html的第2行代码修改如下，此时去点击商品信息链接时，此时就会带1个id=1的参数跳过去。

&lt;a \[routerLink\]="\['/'\]"&gt;home主页&lt;/a&gt;

&lt;a \[routerLink\]="\['/product'\]" \[queryParams\]="{id:1}"&gt;product商品信息&lt;/a&gt;

&lt;input type="button" value="商品信息" \(click\)="toProductDetails\(\)"&gt;

&lt;router-outlet&gt;&lt;/router-outlet&gt;

为了在商品信息中接收id=1的参数，就需要打开商品信息组件product.component.ts，并添加如下代码：

export class ProductComponent implements OnInit {

 public productId: number;

 constructor\(private routeInfo: ActivatedRoute\) { }

 ngOnInit\(\) {

 this.productId = this.routeInfo.snapshot.queryParams\['id'\];

 }

}

然后在商品信息组件的模板文件中用插值表达式引用productId，运行效果如图3.4所示，代码如下：

1.&lt;p&gt;

2.这是product商品信息，该商品的ID为{{productId}}

3.&lt;/p&gt;

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image001.png)

图3.4在查询参数中传递数据运行效果

（2）在路由路径（URL）中传递数据

定义路由的路径 =&gt;实际路径 =&gt;路由目标组件

{path:/product/:id} =&gt; /product/1 =&gt; ActivatedRoute.params\[id\]

u修改路由配置中的path属性使其可以携带参数

将路由配置文件app-routing.module.ts文件中代码修改成如第4行代码所示：

const routes: Routes = \[

 {path: '', component: HomeComponent, children: \[\] },

 {path: 'product/:id', component: ProductComponent, children: \[\]} ,

 {path: '\*\*', component: Error404Component, children: \[\]}

\];

u修改路由链接使其可以传递参数

将应用组件的模板文件app.component.html的第2行代码进行修改，即在\[routeLink\]的数组中直接带上参数值，代码如下所示：

&lt;a \[routerLink\]="\['/'\]"&gt;home主页&lt;/a&gt;

&lt;a \[routerLink\]="\['/product', 1\]"&gt;product商品信息&lt;/a&gt;

&lt;input type="button" value="商品信息" \(click\)="toProductDetails\(\)"&gt;

&lt;router-outlet&gt;&lt;/router-outlet&gt;

将商品信息组件product.component.ts文件中的第5行代码进行修改，使其可以捕获id参数值，其代码如下所示：

export class ProductComponent implements OnInit {

 public productId: number;

 constructor\(private routeInfo: ActivatedRoute\) { }

 ngOnInit\(\) {

 this.productId = this.routeInfo.snapshot.params\['id'\];

 }

}

u最后在商品信息模板中使用插值表达式获得productId。运行后效果如图3.5所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image002.png)

图3.5路由路径（URL）中传递数据运行效果

若此时点击商品信息按钮，则显示“对不起，该页面不存在！”，此时因为商品信息按钮没有接收到参数，所以需要修改app.component.ts文件中的路由指向值，即如下代码第8行所示：

export class AppComponent {

 title = 'app';

 constructor\(private router: Router\) {

 }

 toProductDetails\(\) {

 this.router.navigate\(\['/product', 2\]\);

 }

}

但此时运行后，发现点击商品信息按钮和商品信息链接时显示的商品Id不会改变，这是由于上面使用了**参数快照**的方法来捕获数据，即“this.productId = this.routeInfo.snapshot.params\['id'\];”语句。为了解决这个问题就需要使用**参数订阅**的方法来捕获数据，即修改商品信息组件product.component.ts文件，其代码如下第5行所示：

export class ProductComponent implements OnInit {

 public productId: number;

 constructor\(private routeInfo: ActivatedRoute\) { }

 ngOnInit\(\) {

 this.routeInfo.params.subscribe\(\(params: Params\) =&gt; this.productId = params\['id'\] \);

 }

}

（3）在路由的配置中传递数据

路由的配置=&gt;路由的目标组件

{path:/product,component:ProductComponent,data:\[{isProd:true}\]}=&gt; ActivatedRoute.data\[0\]\[isProd\]

4．重定向路由

所谓重定向路由是指在用户访问一个特定的地址时，将其重定向到另一个指定的地址。要使用重定向路由通常需要修改路由配置文件app-routing.module.ts，下面的代码第2行表示若输入http://localhost:4200，则重定向到http://localhost:4200/home页面。

const routes: Routes = \[

 {path: '', redirectTo: '/home', pathMatch: 'full'},

 {path: 'home', component: HomeComponent, children: \[\] },

 {path: 'product/:id', component: ProductComponent, children: \[\]} ,

 {path: '\*\*', component: Error404Component, children: \[\]}

\];

5．子路由

通过下面的示例代码来讲述子路由的含义：

const routes: Routes = \[

 {path: 'home', component: HomeComponent,

 children: \[

 { path: '', component: HXXXComponent,},

 { path: '/yyy', component: HYYYComponent,}

 \]

 }

\];

上述代码中若访问路由的home路径时，此时会展示HomeComponent组件的模板，同时这个模板的Routerlet位置会展示HXXXComponent组件的模板；若访问home/yyy路径时，此时屏幕上仍然会展示HomeComponent组件的模板，但是这个模板的Routerlet位置会展示HYYYComponent组件的模板。

下面接着上面的示例实现商品信息功能，如图3.6所示，当点击“product商品信息”链接时，会在商品信息下显示“商品详细描述”和“销售员信息”链接，当点击“商品详细描述”和“销售员信息”链接时，分别显示相应的信息。要实现这样的效果，就需要使用子路由功能。具体实现步骤如下：

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image003.png)

图3.6子路由运行效果图

（1）创建商品详细描述组件（productAllInfo）和销售员信息组件（sellInfo）

（2）修改销售员信息组件（sell-info.component.ts文件）

export class SellInfoComponent implements OnInit {

 public sellId: number;

 constructor\(private routeInfo: ActivatedRoute\) { }

 ngOnInit\(\) {

 this.sellId = this.routeInfo.snapshot.params\['id'\];

 }

}

（3）修改路由配置文件，添加子路由，详细代码如下第5-6行所示：

const routes: Routes = \[

 {path: '', redirectTo: '/home', pathMatch: 'full'},

 {path: 'home', component: HomeComponent, children: \[\] },

 {path: 'product/:id', component: ProductComponent, children: \[

 {path: '', component: ProductAllInfoComponent },

 {path: 'seller/:id', component: SellInfoComponent },

 \]} ,

 {path: '\*\*', component: Error404Component, children: \[\]}

\];

（4）修改product.component.html模板文件，其代码如下：

&lt;p&gt;

这是product商品信息，该商品的ID为{{productId}}

&lt;/p&gt;

&lt;a \[routerLink\]="\['./'\]"&gt;商品详细描述&lt;/a&gt;

&lt;a \[routerLink\]="\['./seller',99\]"&gt;销售员信息&lt;/a&gt;

&lt;router-outlet&gt;&lt;/router-outlet&gt;

6．辅助路由

通过下面步骤来辅助路由：

（1）在组件的模板文件上除了添加如下第1行代码作为主插座外，还需要声明一个如下第2行代码所示的带有name属性的插座；

&lt;router-outlet&gt;&lt;/router-outlet&gt;

&lt;router-outlet name="aux"&gt;&lt;/router-outlet&gt;

（2）在路由配置文件中需要配置在（1）中name声明的插座位置显示的组件；

{path: 'xxx',component: XxxComponent,outlet: "aux"}

{path: 'yyy',component: YyyComponent,outlet: "aux"}

以上代码表示可以在“aux”插座处可以用“xxx”或“yyy”路径来显示XxxComponent组件或XxxComponent组件。

（3）在组件的模板文件中指明导航时在路由到某个地址时，在辅助的路由上需要显示的组件。

&lt;a \[routerLink\]="\['/home',{outlets:{aux: 'xxx'}}\]"&gt;Xxx&lt;/a&gt;

&lt;a \[routerLink\]="\['/product',{outlets:{aux: 'yyy'}}\]"&gt;Yyy&lt;/a&gt;

上述代码的第1行表示当用户点Xxx链接时，在主插座会导航到/home路径指定的位置，即显示/home指定的组件内容；辅助插座aux处会显示XxxComponent组件内容。

例如，现在需要在拍卖系统中增加一个即时聊天功能，让客户可以与商家及时沟通，象这样功能不管是在商品详情页，还是商品展示页都需要。要实现这样的功能需要以下步骤来完成：

（1）在app组件的模板上再定义一个插座来显示聊天面板

打开app.component.html模板文件，在最下面添加如下代码：

&lt;router-outlet name="aux"&gt;&lt;/router-outlet&gt;

（2）单独开发一个聊天室组件，只显示在新定义的插座上

l聊天室组件模板文件chat.component.html代码如下：

&lt;textarea placeholder="请输入聊天内容" class="chat"&gt;&lt;/textarea&gt;

l为了控制聊天室的显示样式，所以需要修改聊天室样式文件，代码如下：

.chat {

 background: green;

 height: 100px;

 width: 30%;

 float: left;

 box-sizing: border-box;

}

由于整个页面被分成左右两个部分，右侧显示聊天室，而左侧即要显示主页信息（home），又要显示商品详情（product），所以需要将主页信息组件模板和商品信息模板要增加个div进行控制，同时添加样式。

（3）通过路由参数控制新插座是否显示聊天面板

打开路由配置文件，添加下列第2行代码：

const routes: Routes = \[

 {path: '', redirectTo: '/home', pathMatch: 'full'},

 {path: 'chat', component: ChatComponent, outlet: 'aux'},

 {path: 'home', component: HomeComponent, children: \[\] },

 {path: 'product/:id', component: ProductComponent, children: \[

 {path: '', component: ProductAllInfoComponent },

 {path: 'seller/:id', component: SellInfoComponent },

 \]} ,

 {path: '\*\*', component: Error404Component, children: \[\]}

\];

在应用组件的模板文件app.component.html中添加如下第4-5行链接代码来控制聊天的组件是否显示：

&lt;a \[routerLink\]="\['/'\]"&gt;home主页&lt;/a&gt;

&lt;a \[routerLink\]="\['/product', 1\]"&gt;product商品信息&lt;/a&gt;

&lt;input type="button" value="商品信息" \(click\)="toProductDetails\(\)"&gt;

&lt;a \[routerLink\]="\[{outlets: {aux: 'chat'}}\]"&gt;开始聊天&lt;/a&gt;

&lt;a \[routerLink\]="\[{outlets: {aux: null}}\]"&gt;结束聊天&lt;/a&gt;

&lt;router-outlet&gt;&lt;/router-outlet&gt;

&lt;router-outlet name="aux"&gt;&lt;/router-outlet&gt;

以上代码的运行效果如图3.7所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image005.jpg)

图3.7辅助路由运行效果（1）

也可以将上述代码的第4行改为如下代码，此时当点击开始聊天时会在主路由插座位置显示home组件内容。运行效果如图3.8所示。

&lt;a \[routerLink\]="\[{outlets: {primary: 'home', aux: 'chat'}}\]"&gt;开始聊天&lt;/a&gt;

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image007.jpg)

图3.8辅助路由运行效果（2）

7．路由守卫

在满足一些条件时才需要打开某个页面，例如：

（1）只有当用户已经登录并拥有某些权限时才能进入某些路由；

（2）一个由多个表单组件组成的向导，例如注册流程，用户只有在当前路由的组件中填写了满足要求的信息才可以导航到下一个路由；

（3）当用户未执行保存操作而试图离开当前导航时提醒用户。

以上情况在angular中是使用一种叫钩子的路由守卫来实现的，通常有以下三种路由守卫：

（1）CanActivate：处理导航到某路由的情况；即不能满足这个守卫的要求时，就不能导航到某个指定的路由。

（2）CanDeactivate：处理从当前路由离开的情况；即不能满足这个守卫的要求时，就不能从当前路由离开。

（3）Resolve：在路由激活之前获取路由数据；这样在进入路由时就能立刻将数据展现个用户。

下面通过扩展前面例子功能来介绍路由守卫的用法，即只能让登录用户进入商品的信息路由。具体实现步骤如下：

（1）在app目录下建立一个guard目录，在guard目录下建立一个typescript文件（login.guard.ts）；

（2）在login.guard.ts中实现CanActivate方法以实现路由守卫，详细代码如下：

import {CanActivate} from '@angular/router';

export class LoginGuard implements CanActivate {

 canActivate\(\) {

 const loginYes: boolean = Math.random\(\) &lt; 0.5;

 if \( !loginYes\) {

 console.log\('对不起，用户未登录'\);

 }

 return loginYes;

 }

}

此处为了模拟用户登录与否，使用了产生一个随机数，如果产生的随机数&lt;0.5，则给loginYes变量返回true，即表示用户登录了；否则在控制打印“对不起，用户未登录”。

（3）修改路由配置文件app-routing.module.ts，把路由守卫加到商品信息的路由上，其代码如下：

import {ProductAllInfoComponent} from './product-all-info/product-all-info.component';

import {ChatComponent} from './chat/chat.component';

import {LoginGuard} from './guard/login.guard';

const routes: Routes = \[

 {path: '', redirectTo: '/home', pathMatch: 'full'},

 {path: 'chat', component: ChatComponent, outlet: 'aux'},

 {path: 'home', component: HomeComponent, children: \[\] },

 {path: 'product/:id', component: ProductComponent, children: \[

 {path: '', component: ProductAllInfoComponent },

 {path: 'seller/:id', component: SellInfoComponent },

 \], canActivate: \[LoginGuard\]} ,

 {path: '\*\*', component: Error404Component, children: \[\]}

\];

@NgModule\({

 imports: \[RouterModule.forRoot\(routes\)\],

 exports: \[RouterModule\],

 providers: \[LoginGuard\]

}\)

export class AppRoutingModule { }

上述代码的第8-11行为商品信息的路由，在第11行添加了canActivate就是为了给该路由添加LoginGuard守卫，即如果返回true，就可以作为登录用户查看商品信息；当然为了实现些功能还需要添加第17行代码，该行代码为angular的依赖注入，这个内容将在后面的章节中介绍。

接着实现离开时的路由守卫，即当用户离开时提醒用户执行操作。其步骤如下：

（1）在guard目录下建立unsaved.guard.tstypescript文件，并在文件中实现CanDeactivate方法以实现路由守卫，其详细代码如下：

import {CanDeactivate} from '@angular/router';

import {ProductComponent} from '../product/product.component';

export class UnsavedGuard implements CanDeactivate&lt;ProductComponent&gt; {

 canDeactivate\(component: ProductComponent\) {

 return window.confirm\('还没有保存你就要离开吗？'\);

 }

}

上述代码的第3行必须使用泛型，该泛型指定的类型就是要离开的组件（本例中要离开的是ProductComponent组件）。

（2）修改路由配置文件app-routing.module.ts，把路由守卫加到商品信息的路由上，具体代码如下：

import {ChatComponent} from './chat/chat.component';

import {LoginGuard} from './guard/login.guard';

import {UnsavedGuard} from './guard/unsaved.guard';

const routes: Routes = \[

 {path: '', redirectTo: '/home', pathMatch: 'full'},

 {path: 'chat', component: ChatComponent, outlet: 'aux'},

 {path: 'home', component: HomeComponent, children: \[\] },

 {path: 'product/:id', component: ProductComponent, children: \[

 {path: '', component: ProductAllInfoComponent },

 {path: 'seller/:id', component: SellInfoComponent },

 \], canActivate: \[LoginGuard\], canDeactivate: \[UnsavedGuard\]} ,

 {path: '\*\*', component: Error404Component, children: \[\]}

\];

@NgModule\({

 imports: \[RouterModule.forRoot\(routes\)\],

 exports: \[RouterModule\],

 providers: \[LoginGuard, UnsavedGuard\]

}\)

export class AppRoutingModule { }

其实上述代码只是在第11行后面添加了canDeactivate的内容，在第17行后面添加了UnsavedGuard，运行效果如图3.9所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image008.png)

图3.9路由守卫效果图

在路由守卫中还有一种情况，就是在激活路由之前获取数据，一旦进入路由后就可以直接在相应的地方展现数据，这种路由就是Resolve路由。其实现方法可以通过下面的步骤完成，即在进入商品信息的路由之前就先读取商品信息，在读好后带着这个信息进入到路由中。

（1）定义Product对象

打开Product.Component.ts文件，在里面定义Product，代码如下：

export class Product {

 constructor\(public id: number, public name: string\) {

 }

}

（2）在guard目录下创建product.resolve.ts文件

import {ActivatedRouteSnapshot, Resolve, Router, RouterStateSnapshot} from '@angular/router';

import {Product} from '../product/product.component';

import {Observable} from 'rxjs/Observable';



export class ProductResolve implements Resolve&lt;Product&gt; {

 constructor\(private router: Router\) {

 }

 resolve\(route: ActivatedRouteSnapshot, state: RouterStateSnapshot\): Observable&lt;Product&gt; \| Promise&lt;Product&gt; \| Product {

 let productId: number = route.params\['id'\];

 if \(productId == 1\) {

 return new Product\(1, 'iphone8'\);

 } else {

 this.router.navigate\(\['/home'\]\)

 return undefined;

 }

 }

}

本例中假设返回的productId是1时为一个正确的Id（此时返回一个Product对象），若不是1就要通过路由将其导航到/home目录下，要导航走就要用到Router，此时就需要从构造函数中注入进来（上述代码中第6-7行）。这里需要读者注意，这个类需要使用装饰器来进行装饰

（3）加入路由

