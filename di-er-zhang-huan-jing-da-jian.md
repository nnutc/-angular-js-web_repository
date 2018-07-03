第二章 Angular项目结构和组件开发

**本讲主要内容**

**1．熟悉Angular项目目录结构**

**2．掌握app.module.ts和app.component.ts源代码的构成及作用**

**3．掌握在项目中引入第三方类库的方法**

**4．掌握基本组件开发和应用**

**5．了解bootstrap\(本教程以3.3.7版本为例\)基本的前端页面开发方法**

**6．掌握使用typescript语言创建类、使用类的方法**

**7．掌握Angular的循环结构、数据绑定（含插值绑定、属性绑定）的使用方法**

**8．掌握父组件给子组件传值的方法（Input装饰器的用法）**

**一、Angular项目目录结构**

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image002.jpg)![](/assets/21.JPG)

图1 Angular项目结构

1．e2e：端到端（End-to-End）的测试目录，包含基本的测试桩。

2．node\_modules：Node.js创建了这个文件夹，并且把package.json中列出的所有第三方模块都存放在该目录中；

3．src：应用源代码目录，开发者写的代码均存储在此目录中，其包含的目录和文件主要有如下几类：

lapp：包含了应用程序的所有组件和模块；一个angular至少包含一个组件和一个模块；开发环境生成项目时，会自动生成下面两个文件：

（1）app.component.ts：组件，它是整个应用的基础；其默认的源代码如下：

1.//从angular的core\(核心\)模块中引入了一个叫Component的装饰器

2.import{ Component } from '@angular/core';

3.//4-7行用Component装饰器定义了一个组件

4.@Component\({

5.selector: 'app-root',/\*表示使用这个组件的名称 \*/

6.templateUrl: './app.component.html',

7.styleUrls: \['./app.component.css'\]

8.}\)

9.//10-12行定义了组件的控制器及数据

10.export classAppComponent {

11.title= 'app';

12.}

selector：CSS选择器，它通知Angular在父级HTML中查找&lt;app-root&gt;标签，创建并插入该组件。例如，如果应用的HTML包含&lt;app-root&gt;&lt;/app-root&gt;，Angular就会把AppComponent的一个实例插入到这个标签中。

templateUrl：组件HTML模板的模块相对地址，本例中为“./app.component.html”。

styleUrls：组件使用的样式文件地址，本例中为“./app.component.css”。

（2）app.module.ts：定义AppModule，Angular模块（无论是根模块还是特性模块）都是一个带有@NgModule装饰器的类；其默认的源代码如下：

1./\*引入组件\*/

2.import { BrowserModule } from '@angular/platform-browser';/\*浏览器解析模块\*/

3.import { NgModule } from '@angular/core'; /\*核心模块\*/

4.import { AppComponent } from './app.component'; /\*自定义的模块\*/

5./\*@NgModule装饰器将AppModule标记为Angular模块类，也叫NgModule类。@NgModule 接受一个元数据对象，告诉Angular如何编译和启动应用。/

6.@NgModule\({

7.declarations: \[/\*引入当前项目运行的组件，自定义组件需要引入并且在这里面配置\*/

8.AppComponent

9.\],

10.imports: \[/\*引入当前项目运行依赖的其他模块\*/

11.BrowserModule

12.\],

13.providers: \[\],/\*定义的服务，此处为空，以后如果有服务可以添加于此\*/

14.bootstrap: \[AppComponent\] /\*默认启动哪个组件，也就是指定应用的主视图或称为根组件，通过引导根AppModule来启动应用，这里一般写的是根组件\*/

15.}\)

16.export class AppModule { }/\*根模块不需要导出任何东西，因为其他组件不需要导入根模块，但一定要写\*/

NgModule是一个装饰器函数，它接收一个用来描述模块属性的元数据对象。其中最重要的属性是：

declarations：声明本模块中拥有的视图类。Angular有三种视图类：组件、指令和管道。

imports：声明要应用运转还需要什么模块。本例中的BrowserModule表示应用要使用浏览器模块，这个模块是开发Web应用时的必选模块；还有例如FormsModule（表单模块）、HttpModule（Http模块）等。在应用中引用了这些模块后，就可以使用这些模块的组件、指令和服务等。

providers：声明模块中提供的服务，在依赖注入章节会详细讲解。

bootstrap：声明应用的主组件是什么，即指定应用的主视图（称为根组件）；只有根模块才能设置bootstrap属性。

lassets：用来存储静态资源的，比如图片；

lenvironments：这个文件夹中包括为各个目标环境准备的文件，它们导出了一些应用中要用到的配置变量；

lindex.html：整个应用程序的根html文件；

lmain.ts：是整个Web应用的入口点，即整个脚本执行的入口点，angular用这个文件来启动整个项目；

lpolyfills.ts：用于导入有些必要的库，主要保证angular应用能够运行在一些老版本的浏览器上；

lstyle.css：用于存放应用的全局样式；

ltest.ts：单元测试的主要入口点，用于进行单元测试；

ltsconfig.{app，spec}.json：typescript编译器的配置文件；tsconfig.app.json是为Angular应用准备的，tsconfig.spec.json是为单元测试准备的；

4．.editconfig：是编辑器（WebStorm、PhpStorm等）的配置文件，与项目无关；

5．.gitignore：是git的配置文件，用来确保某些自动生成的文件不会被提交到源码控制系统中，与项目无关；

6．karma.conf.js：是由Google团队开发的一套前端测试运行框架，用于完成单元自动化测试。

7．angular-cli.json：angular命令行工具的配置文件，比如引用第三方包（如jquery）时需要修改此配置文件；

8．package.json：标准的npm工具配置文件，该文件中指明所开发的应用中所使用到的第三方依赖包；

9．protractor.conf.js：与karma.conf.js类似，用于做自动化测试的配置文件；

10．tslint.json：用于定义typescript代码质量检查规则的，不需要开发者修改；

11．tsconfig.json：typescript编译器的一个配置文件；

12．README.md：项目的基础文档，预先写了CLI命令的信息。

**二、在项目中引入第三方类库**

以jquery为例，介绍将第三方类库安装到本地步骤：

（1）单击开始→所有程序→Node.js→Node.js command prompt，进入Node.js命令行；

（2）把当前目录改为项目保存位置（本例中是d:\angular\_web\newsreport），然后输入npm install jquery –save，此时从网上下载jquery第三方类库包（输入npm install bootstrap –save，则可以安装bootstrap包）；如图2所示。下载完后的第三方类库包保存在开发环境的node\_modules目录下，并且在package.json文件中会增加相应的配置命令。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image004.jpg)![](/assets/22.JPG)

图2安装第三方类库

l在项目中引入库文件——修改.angular-cli.json文件

打开.angular-cli.json文件，从中找到scripts，添加如下代码：

1."styles": \[

2."styles.css",

3."../node\_modules/bootstrap/dist/css/bootstrap.css"

4.\],

5."scripts": \[

6."../node\_modules/jquery/dist/jquery.js",

7."../node\_modules/bootstrap/dist/js/bootstrap.js"

8.\],

l为了在WebStorm开发环境中使用jquery、bootstrap的一些命令，就需要继续安装它们的类型描述文件，这样就可以在typescript中调用jquery、bootstrap里面的相关内容。其安装步骤如下：

（1）单击开始→所有程序→Node.js→Node.js command prompt，进入Node.js命令行；

（2）把当前目录改为项目保存位置（本例中是d:\angular\_web\newsreport），然后输入npm install @types/jquery --save-dev，安装jquery的类型描述文件；输入npm install @types/bootstrap -- save-dev，安装bootstrap的类型描述文件。其运行效果如图3所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image006.jpg)![](/assets/23.JPG)

图3安装类型描述文件

三、创建组件及应用

1．新闻发布系统框架介绍

为了让大家能够快速熟悉angular开发技术，下面以一个竞拍系统的实现过程进行详细介绍，首先来分析一下如图4所示的新闻发布系统主界面，该主界面用angular实现时可以分成7个组件：

（1）App组件（系统自动生成）

（2）搜索表单组件（search）

（3）新闻展示组件（news）

（4）导航栏组件（navbar）

（5）轮播图组件（picture）

（6）页脚组件（footer）

（7）星级评定组件（stars）

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image008.jpg)![](/assets/24.JPG)

图4新闻发布系统界面



2．项目中组件生成方法

在系统生成项目时，App组件已经自动生成，其他组件需要开发者创建，创建组件的方法有两种：

l利用WebStorm集成开发环境创建：即在项目目录的app文件夹上右击，在弹出的菜单中选择New→Angular CLI…后，输入新建组件的名称，本例中的名称为“footer”，如图5所示；

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image010.jpg)![](/assets/25.JPG)

图5新建组件

l利用命令行创建：单击开始→所有程序→Node.js→Node.js command prompt，进入Node.js命令行，输入ng g component stars，表示创建星级评定组件。

3．开发组件

（1）编写app组件的模板文件——app.component.html

根据图4界面所示，整个页面框架分为上中下三个部分，最上面是导航条（加载navbar组件）；最下面是页脚（加载footer组件）。中间是整个页面的核心部分，分为左右两例，左边用1/4区域放置搜索内容（加载search组件）；右边用3/4区域又分成上下两个部分：上半部分显示滚动图（加载picture组件）、下半部分显示产品信息（加载product组件）。详细代码如下：

1.&lt;app-navbar&gt;&lt;/app-navbar&gt;

2.&lt;div class="container"&gt;

3.&lt;div class="row"&gt;

4.&lt;div class="col-md-3"&gt;

5.&lt;app-search&gt;&lt;/app-search&gt;

6.&lt;/div&gt;

7.&lt;div class="col-md-9"&gt;

8.&lt;div class="row"&gt;

9.&lt;app-picture&gt;&lt;/app-picture&gt;

10.&lt;/div&gt;

11.&lt;div class="row"&gt;

12.&lt;app-product&gt;&lt;/app-product&gt;

13.&lt;/div&gt;

14.&lt;/div&gt;

15.&lt;/div&gt;

16.&lt;/div&gt;

17.&lt;app-footer&gt;&lt;/app-footer&gt;

（2）编写导航条组件（navbar）的模板文件——navbar.component.html

导航条在每个网站中几乎都会用到，本案例中开发的导航条要能实现导航条的内容会随着浏览器窗口尺寸的变化能够让导航条中的内容能够自动收放（即当浏览器窗口小到一定程序时，会在导航条右侧出现一个按钮，单击按钮能够出现导航导中内容的菜单），显示效果如图6所示，便于适应不同大小屏幕的智能设备，实现代码如下：

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image012.jpg)![](/assets/26.JPG)

图6导航条显示效果

1.&lt;nav class="navbar navbar-inverse navbar-fixed-top"&gt;

2.&lt;div class="container"&gt;

3.&lt;div class="nav-header"&gt;

4.&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbb"&gt;

5.&lt;span class="icon-bar"&gt;&lt;/span&gt;

6.&lt;span class="icon-bar"&gt;&lt;/span&gt;

7.&lt;span class="icon-bar"&gt;&lt;/span&gt;

8.&lt;/button&gt;

9.&lt;/div&gt;

10.&lt;div class="navbar-header"&gt;

11.&lt;a class="navbar-brand" href="\#"&gt;在线竞拍&lt;/a&gt;

12.&lt;/div&gt;

13.&lt;div class="collapse navbar-collapse navbb"&gt;

14.&lt;ul class="nav navbar-nav"&gt;

15.&lt;li&gt;&lt;a href="\#"&gt;关于我们&lt;/a&gt;&lt;/li&gt;

16.&lt;li&gt;&lt;a href="\#"&gt;联系我们&lt;/a&gt;&lt;/li&gt;

17.&lt;li&gt;&lt;a href="\#"&gt;网站地图&lt;/a&gt;&lt;/li&gt;

18.&lt;/ul&gt;

19.&lt;/div&gt;

20.&lt;/div&gt;

21.&lt;/nav&gt;

第1行表示在加载一个固定在浏览器上方边缘的导航条；第4-8行表示当浏览器窗口变小时，保证在导航条右侧出现一个按钮，并在按钮上画三条线。以上代码均是用bootstrap写成，读者如不熟悉就查阅bootstrap相关资料。

此时运行出的效果中导航条会遮盖下面模块的内容。为了解决这个问题，需要在项目的全局样式文件styles.css中增加如下代码：

1.body{

2.padding-top: 70px;

3.}

（2）编写页脚组件（footer）的模板文件——footer.component.html

由图2.9可以看出，页脚组件显示的内容比较简单，其源代码如下：

1.&lt;div class="container"&gt;

2.&lt;hr&gt;

3.&lt;footer&gt;

4.&lt;div class="row"&gt;

5.&lt;div class="col-lg-12"&gt;

6.&lt;p&gt; copy right@nnutc.edu.cn &lt;/p&gt;

7.&lt;/div&gt;

8.&lt;/div&gt;

9.&lt;/footer&gt;

10.&lt;/div&gt;



（3）编写搜索组件（search）的模板文件——search.component.html

由图2.9可以看出，页脚组件显示的内容比较简单，其源代码如下：

1.&lt;form name="searchForm" role="form"&gt;

2.&lt;div class="form-group"&gt;

3.&lt;label for="newsTitle"&gt;新闻名称：&lt;/label&gt;

4.&lt;input type="text" id="newsTitle" placeholder="新闻名称" class="form-control"&gt;

5.&lt;/div&gt;

6.&lt;div class="form-group"&gt;

7.&lt;label for="newsPrice"&gt;发布时间：&lt;/label&gt;

8.&lt;input type="number" id="newsPrice" placeholder="发布时间" class="form-control"&gt;

9.&lt;/div&gt;

10.&lt;div class="form-group"&gt;

11.&lt;label for="newsCatgoery"&gt;新闻类别：&lt;/label&gt;

12.&lt;select id="newsCatgoery" class="form-control"&gt;&lt;/select&gt;

13.&lt;/div&gt;

14.&lt;div class="form-group"&gt;

15.&lt;button type="submit" class="btn btn-promary btn-block"&gt;搜索&lt;/button&gt;

16.&lt;/div&gt;

17.&lt;/form&gt;

（4）编写轮播组件（picture）的模板文件——picture.component.html

由图4可以看出，轮播组件显示的内容比较复杂，其源代码如下：

1.&lt;div class="carousel slide" data-ride="carousel"&gt;

2.&lt;ol class="carousel-indicators"&gt;

3.&lt;li class="active"&gt;&lt;/li&gt;

4.&lt;li&gt;&lt;/li&gt;

5.&lt;li&gt;&lt;/li&gt;

6.&lt;/ol&gt;

7.&lt;div class="carousel-inner"&gt;

8.&lt;div class="item active"&gt;

9.&lt;img class="slide-image" src="http://via.placeholder.com/800x300" alt=""&gt;

10.&lt;/div&gt;

11.&lt;div class="item"&gt;

12.&lt;img class="slide-image" src="http://via.placeholder.com/800x300" alt=""&gt;

13.&lt;/div&gt;

14.&lt;div class="item"&gt;

15.&lt;img class="slide-image" src="http://via.placeholder.com/800x300" alt=""&gt;

16.&lt;/div&gt;

17.&lt;a class="left carousel-control" href="javascript:$\('.carousel'\).carousel\('prev'\)"&gt;

18.&lt;span class="glyphicon glyphicon-chevron-left"&gt;&lt;/span&gt;

19.&lt;/a&gt;

20.&lt;a class="right carousel-control" href="javascript:$\('.carousel'\).carousel\('next'\)"&gt;

21.&lt;span class="glyphicon glyphicon-chevron-right"&gt;&lt;/span&gt;

22.&lt;/a&gt;

23.&lt;/div&gt;

24.&lt;/div&gt;

第2-6行代码是在轮播图片下方显示三个空心圆点以表示显示哪张图片；第7-16行代码插入三张轮播图片（来源网络，开发时，用户可以指定自己的图片）；第17-19行代码在图片左侧显示轮播按钮；第20-22行代码在图片右侧显示轮播按钮。

（5）在news.component.ts文件用typescript编写新闻信息类——News

①在news.component.ts文件中创建包含news新闻相关信息的类（有typescript语言编写在news.component.ts文件的末尾），其详细代码如下：

1.export class News{

2.constructor\(

3.public id:number,

4.public title:string,

5.public time: string,

6.public rating:number,

7.public descript:string,

8.public categories:Array&lt;string&gt;

9.\){

10.}

11.}

从上述代码可以看出使用export class News创建一个News类，该类有一个构造方法constructor，里面包含了新闻ID（number类型）、新闻标题、发布时间、星级评价、详细描述和新闻类别（一个新闻可以同属于不同的类别，定义为数组类型）。

②在Newscomponent类（即在news.component.ts文件）中用typescript声明一个数组存储在页面上需要展示的数据，其详细代码如下：

1.export class NewsComponent implements OnInit {

2.public news:Array&lt;News&gt;;

3.constructor\(\) { }

4.ngOnInit\(\) {

5.this.news = \[

6.new News\(1, '第1条新闻', '2017-1-2', 3.5, '这是第1个新闻的详细信息', \['普通新闻', '特色新闻'\]\),

7.new News\(2, '第2条新闻', '2017-10-2', 2.5, '这是第2个新闻的详细信息', \['特色新闻'\]\),

8.new News\(3, '第3条新闻', '2017-2-22', 3.5, '这是第3个新闻的详细信息', \['普通新闻'\]\),

9.new News\(4, '第4条新闻', '2017-1-12', 4.5, '这是第4个新闻的详细信息', \['外部新闻'\]\),

10.new News\(5, '第5条新闻', '2016-11-2', 5.0, '这是第5个新闻的详细信息', \['普通新闻'\]\),

11.new News\(6, '第6条新闻', '2015-8-12', 3.5, '这是第6个新闻的详细信息', \['特色新闻', '外部新闻'\]\),

12.\]; }

13.}

上述代码中第2行声明一个News类型的数组news，第6-12行初始化6条新闻信息并放到news数组中。

当然也可以用下列代码向news数组中增加物品信息：

1.this.news.push\(new News\(7, '第7条新闻', '2017-1-12', 4.5, '这是第7个新闻的详细信息', \['外部新闻'\]\),\)

（6）编写商品展示（news）的模板文件——news.component.html

由图4可以看出，新闻展示组件显示的内容比较复杂，需要用angular的语法从（5）中初始化的新闻类中读出新闻信息，并显示出来，其源代码如下：

1.&lt;div \*ngFor="let news of news" class="col-md-4 col-sm-4 col-lg-4"&gt;

2.&lt;div class="thumbnail"&gt;

3.&lt;img src="http://via.placeholder.com/320x150"&gt;

4.&lt;div class="caption"&gt;

5.&lt;h4 class="pull-right"&gt;{{news.time}}&lt;/h4&gt;

6.&lt;h4&gt;&lt;a&gt;{{news.title}}&lt;/a&gt;&lt;/h4&gt;

7.&lt;p&gt;{{news.descript}}&lt;/p&gt;

8.&lt;/div&gt;

9.&lt;div&gt;

10.&lt;app-stars&gt;&lt;/app-stars&gt;

11.&lt;/div&gt;

12.&lt;/div&gt;

13.&lt;/div&gt;

上述代码的第1行\*ngFor="let news of news"表示从news数组中分别取出news元素，这儿的news也就是在Newscomponent类中定义并初始化的news，此两处的名字一定要相同。class="col-md-4 col-sm-4 col-lg-4"表示每一个news占一行的1/3。第5行的{{news.time}}是angular中**数据绑定中的插值绑定方法**。

此时轮播图与新闻卡片之间的距离有点小，可以通过修改app.component.css文件来实现，也就是使用样式属性来进行控制，代码如下：

1..picture-ff{

2.margin-bottom:30px;

3.}

代码中的picture-ff需要在app组件模板文件——app.component.html中修改代码，具体如下所示：

1.&lt;div class="row picture-ff"&gt;

2.&lt;app-picture&gt;&lt;/app-picture&gt;

3.&lt;/div&gt;

（6）编写星级评价展示（news）的模板文件——star.component.html

①显示实心星星![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image014.jpg)：在star.component.html模板文件添加下列代码：

1.&lt;span class="glyphicon glyphicon-star"&gt;&lt;/span&gt;

②显示空心星星![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image016.jpg)：在stars.component.html模板文件添加下列代码：

1.&lt;span class="glyphicon glyphicon-star glyphicon-star-empty"&gt;&lt;/span&gt;

③显示5个星星：若要在前台显示5个星星，就要在后台有包含5个元素的数组，后台写这样数组的方法：

打开stars.component.ts文件，在StarsComponent类中添加下列代码中的第2行（定义boolean的数组）和第6行（初始化数组元素）：

1.export class StarsComponent implements OnInit {

2.public stars: boolean\[\];

3.constructor\(\) {

4.}

5.ngOnInit\(\) {

6.this.stars=\[true,true,true,true,true\]

7.}

8.}

④控制5个星星中有的空心有的实心（用angular中的**数据绑定中的属性绑定方法**实现）：将②中的代码改为如下代码：

1.&lt;span \*ngFor="let star of stars" class="glyphicon glyphicon-star" \[class.glyphicon-star-empty\]="star"&gt;&lt;/span&gt;

\[class.glyphicon-star-empty\]="star"是属性绑定中的一个特例——样式绑定，即将要显示空星星的样式绑定到后台的属性stars数组元素上，即当数组元素的值为true时，显示空心星星。glyphicon-star-empty前面的class表示绑定的东西是一个css样式。

所谓属性绑定就是把html中的标签属性与后台控制器中的属性进行绑定，例如：上面（picture.component.html）文件中设置html标签img属性用的传统方法，代码如下：

1.&lt;img class="slide-image" src="http://via.placeholder.com/800x300" alt=""&gt;

如果要将这个传统的方法改为angular下的属性绑定方法，就需要在控制器（picture.component.ts）文件中定义属性，代码如下：

1.export class PictureComponent implements OnInit {

2.constructor\(\) {

3.}

4.public imgUrl = "http://via.placeholder.com/800x300";

5.ngOnInit\(\) {

6.}

7.}

使用属性绑定的代码如下：

1.&lt;img class="slide-image" \[src\]=imgUrl alt=""&gt;

⑤将新闻组件中的评价值传递给星级评价组件

在星级评价组件StarsComponent.ts文件中使用装饰器将新闻组件中的评价值传递过来，即使用下列两行代码：

1.@Input\(\)

2.public rating =0;

此处需要使用以下语句导入Input包：

import {Component, Input, OnInit} from '@angular/core';

然后在新闻组件的模板news.component.html文件中通过属性绑定的方法将news.rating值传递给星级评价组件，代码如下：

1.&lt;app-stars \[rating\]="news.rating"&gt;&lt;/app-stars&gt;

最后在星级评价组件StarsComponent.html文件中将评价值显示出来，代码如下：

1.&lt;span &gt;{{rating}}星&lt;/span&gt;

⑥根据商品的星级值决定空心星星或实心星星

在星级评价组件StarsComponent.ts文件中增加下列代码的第8-11行，第8行初始化stars空数组元素，第9-11行通过循环来控制每条新闻显示的星为空心还是实心星。例如第一个新闻的rating若为2.5时，该新闻对应的stars数组值为\[false,false,true,true,true\]，那么对应的星显示效果为![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image018.jpg)。

1.export class StarsComponent implements OnInit {

2.@Input\(\)

3.public rating:number=0;

4.public stars: boolean\[\];

5.constructor\(\) {

6.}

7.ngOnInit\(\) {

8.this.stars=\[\]

9.for\(let i=1;i&lt;=5;i++\){

10.this.stars.push\(i&gt;this.rating\);

11.}

12.}

13.}







