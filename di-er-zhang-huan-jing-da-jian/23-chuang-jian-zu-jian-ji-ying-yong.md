1．新闻发布系统框架介绍
为了让大家能够快速熟悉angular开发技术，下面以一个竞拍系统的实现过程进行详细介绍，首先来分析一下如图4所示的新闻发布系统主界面，该主界面用angular实现时可以分成7个组件：
（1）App组件（系统自动生成）
（2）搜索表单组件（search）
（3）新闻展示组件（news）
（4）导航栏组件（navbar）
（5）轮播图组件（picture）
（6）页脚组件（footer）
（7）星级评定组件（stars）
 
图4 新闻发布系统界面

2．项目中组件生成方法
在系统生成项目时，App组件已经自动生成，其他组件需要开发者创建，创建组件的方法有两种：
	利用WebStorm集成开发环境创建：即在项目目录的app文件夹上右击，在弹出的菜单中选择New→Angular CLI…后，输入新建组件的名称，本例中的名称为“footer”，如图5所示；
 
图5 新建组件
	利用命令行创建：单击开始 →所有程序 →Node.js→Node.js command prompt，进入Node.js命令行，输入ng  g component stars，表示创建星级评定组件。
3．开发组件
（1）编写app组件的模板文件——app.component.html
根据图4界面所示，整个页面框架分为上中下三个部分，最上面是导航条（加载navbar组件）；最下面是页脚（加载footer组件）。中间是整个页面的核心部分，分为左右两例，左边用1/4区域放置搜索内容（加载search组件）；右边用3/4区域又分成上下两个部分：上半部分显示滚动图（加载picture组件）、下半部分显示产品信息（加载product组件）。详细代码如下：
<app-navbar></app-navbar>
<div class="container">
  <div class="row">
      <div class="col-md-3">
          <app-search></app-search>
      </div>
      <div class="col-md-9">
          <div class="row">
              <app-picture></app-picture>
          </div>
          <div class="row">
              <app-product></app-product>
          </div>
      </div>
  </div>
</div>
<app-footer></app-footer>
（2）编写导航条组件（navbar）的模板文件——navbar.component.html
导航条在每个网站中几乎都会用到，本案例中开发的导航条要能实现导航条的内容会随着浏览器窗口尺寸的变化能够让导航条中的内容能够自动收放（即当浏览器窗口小到一定程序时，会在导航条右侧出现一个按钮，单击按钮能够出现导航导中内容的菜单），显示效果如图6所示，便于适应不同大小屏幕的智能设备，实现代码如下：
 
图6 导航条显示效果
<nav class="navbar navbar-inverse navbar-fixed-top">
<div class="container">
        <div class="nav-header">
            <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbb">
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
        </div>
        <div class="navbar-header">
            <a class="navbar-brand" href="#">在线竞拍</a>
        </div>
        <div class="collapse navbar-collapse navbb">
            <ul class="nav navbar-nav">
                <li><a href="#">关于我们</a></li>
                <li><a href="#">联系我们</a></li>
                <li><a href="#">网站地图</a></li>
            </ul>
        </div>
    </div>
</nav>
第1行表示在加载一个固定在浏览器上方边缘的导航条；第4-8行表示当浏览器窗口变小时，保证在导航条右侧出现一个按钮，并在按钮上画三条线。以上代码均是用bootstrap写成，读者如不熟悉就查阅bootstrap相关资料。
此时运行出的效果中导航条会遮盖下面模块的内容。为了解决这个问题，需要在项目的全局样式文件styles.css中增加如下代码：
body{
    padding-top: 70px;
}
（2）编写页脚组件（footer）的模板文件——footer.component.html
由图2.9可以看出，页脚组件显示的内容比较简单，其源代码如下：
<div class="container">
  <hr>
  <footer>
    <div class="row">
      <div class="col-lg-12">
        <p> copy right@nnutc.edu.cn </p>
      </div>
   </div>
 </footer>
</div>

（3）编写搜索组件（search）的模板文件——search.component.html
由图2.9可以看出，页脚组件显示的内容比较简单，其源代码如下：
<form name="searchForm" role="form">
  <div class="form-group">
    <label for="newsTitle">新闻名称：</label>
    <input type="text" id="newsTitle" placeholder="新闻名称" class="form-control">
  </div>
  <div class="form-group">
    <label for="newsPrice">发布时间：</label>
    <input type="number" id="newsPrice" placeholder="发布时间" class="form-control">
  </div>
  <div class="form-group">
    <label for="newsCatgoery">新闻类别：</label>
    <select   id="newsCatgoery"  class="form-control"></select>
  </div>
  <div class="form-group">
    <button type="submit" class="btn btn-promary btn-block">搜索</button>
  </div>
</form>
（4）编写轮播组件（picture）的模板文件——picture.component.html
由图4可以看出，轮播组件显示的内容比较复杂，其源代码如下：
<div class="carousel slide" data-ride="carousel">
    <ol class="carousel-indicators">
        <li class="active"></li>
        <li></li>
        <li></li>
    </ol>
    <div class="carousel-inner">
        <div class="item active">
            <img class="slide-image" src="http://via.placeholder.com/800x300" alt="">
        </div>
        <div class="item">
            <img class="slide-image" src="http://via.placeholder.com/800x300" alt="">
        </div>
        <div class="item">
            <img class="slide-image" src="http://via.placeholder.com/800x300" alt="">
        </div>
        <a class="left carousel-control" href="javascript:$('.carousel').carousel('prev')">
            <span class="glyphicon glyphicon-chevron-left"></span>
        </a>
        <a class="right carousel-control" href="javascript:$('.carousel').carousel('next')">
            <span class="glyphicon glyphicon-chevron-right"></span>
        </a>
    </div>
</div>
第2-6行代码是在轮播图片下方显示三个空心圆点以表示显示哪张图片；第7-16行代码插入三张轮播图片（来源网络，开发时，用户可以指定自己的图片）；第17-19行代码在图片左侧显示轮播按钮；第20-22行代码在图片右侧显示轮播按钮。
（5）在news.component.ts文件用typescript编写新闻信息类——News
①在news.component.ts文件中创建包含news新闻相关信息的类（有typescript语言编写在news.component.ts文件的末尾），其详细代码如下：
export class News{
  constructor(
      public id:number,
      public title:string,
      public time: string,
      public rating:number,
      public descript:string,
      public categories:Array<string>
  ){    
  }
}
从上述代码可以看出使用export class News创建一个News类，该类有一个构造方法constructor，里面包含了新闻ID（number类型）、新闻标题、发布时间、星级评价、详细描述和新闻类别（一个新闻可以同属于不同的类别，定义为数组类型）。
②在Newscomponent类（即在news.component.ts文件）中用typescript声明一个数组存储在页面上需要展示的数据，其详细代码如下：
export class NewsComponent implements OnInit {
  public  news:Array<News>;
  constructor() { }
  ngOnInit() {
   this.news = [
      new News(1, '第1条新闻', '2017-1-2', 3.5, '这是第1个新闻的详细信息', ['普通新闻', '特色新闻']),
      new News(2, '第2条新闻', '2017-10-2', 2.5, '这是第2个新闻的详细信息', ['特色新闻']),
      new News(3, '第3条新闻', '2017-2-22', 3.5, '这是第3个新闻的详细信息', ['普通新闻']),
      new News(4, '第4条新闻', '2017-1-12', 4.5, '这是第4个新闻的详细信息', ['外部新闻']),
      new News(5, '第5条新闻', '2016-11-2', 5.0, '这是第5个新闻的详细信息', ['普通新闻']),
      new News(6, '第6条新闻', '2015-8-12', 3.5, '这是第6个新闻的详细信息', ['特色新闻', '外部新闻']),
    ];  }
}
上述代码中第2行声明一个News类型的数组news，第6-12行初始化6条新闻信息并放到news数组中。
当然也可以用下列代码向news数组中增加物品信息：
this.news.push(new News(7, '第7条新闻', '2017-1-12', 4.5, '这是第7个新闻的详细信息', ['外部新闻']),)
（6）编写商品展示（news）的模板文件——news.component.html
由图4可以看出，新闻展示组件显示的内容比较复杂，需要用angular的语法从（5）中初始化的新闻类中读出新闻信息，并显示出来，其源代码如下：
<div *ngFor="let news of news" class="col-md-4 col-sm-4 col-lg-4">
    <div class="thumbnail">
        <img src="http://via.placeholder.com/320x150">
        <div class="caption">
            <h4 class="pull-right">{{news.time}}</h4>
            <h4><a>{{news.title}}</a></h4>
            <p>{{news.descript}}</p>
        </div>
        <div>
            <app-stars></app-stars>
        </div>
    </div>
</div>
上述代码的第1行*ngFor="let news of news"表示从news数组中分别取出news元素，这儿的news也就是在Newscomponent类中定义并初始化的news，此两处的名字一定要相同。class="col-md-4 col-sm-4 col-lg-4"表示每一个news占一行的1/3。第5行的{{news.time}}是angular中数据绑定中的插值绑定方法。
此时轮播图与新闻卡片之间的距离有点小，可以通过修改app.component.css文件来实现，也就是使用样式属性来进行控制，代码如下：
.picture-ff{
    margin-bottom:30px;
}
代码中的picture-ff需要在app组件模板文件——app.component.html中修改代码，具体如下所示：
<div class="row picture-ff">
     <app-picture></app-picture>
</div>
（6）编写星级评价展示（news）的模板文件——star.component.html
①显示实心星星 ：在star.component.html模板文件添加下列代码：
<span class="glyphicon glyphicon-star"></span>
②显示空心星星 ：在stars.component.html模板文件添加下列代码：
<span class="glyphicon glyphicon-star glyphicon-star-empty"></span>
③显示5个星星：若要在前台显示5个星星，就要在后台有包含5个元素的数组，后台写这样数组的方法：
打开stars.component.ts文件，在StarsComponent类中添加下列代码中的第2行（定义boolean的数组）和第6行（初始化数组元素）：
export class StarsComponent implements OnInit {
    public stars: boolean[];
    constructor() {
    }
    ngOnInit() {
        this.stars=[true,true,true,true,true]
    }
}
④控制5个星星中有的空心有的实心（用angular中的数据绑定中的属性绑定方法实现）：将②中的代码改为如下代码：
<span *ngFor="let star of stars" class="glyphicon glyphicon-star" [class.glyphicon-star-empty]="star"></span>
[class.glyphicon-star-empty]="star"是属性绑定中的一个特例——样式绑定，即将要显示空星星的样式绑定到后台的属性stars数组元素上，即当数组元素的值为true时，显示空心星星。glyphicon-star-empty前面的class表示绑定的东西是一个css样式。
所谓属性绑定就是把html中的标签属性与后台控制器中的属性进行绑定，例如：上面（picture.component.html）文件中设置html标签img属性用的传统方法，代码如下：
<img class="slide-image" src="http://via.placeholder.com/800x300" alt="">
如果要将这个传统的方法改为angular下的属性绑定方法，就需要在控制器（picture.component.ts）文件中定义属性，代码如下：
export class PictureComponent implements OnInit {
    constructor() {
    }
    public imgUrl = "http://via.placeholder.com/800x300";
    ngOnInit() {
    }
}
使用属性绑定的代码如下：
<img class="slide-image" [src]=imgUrl alt="">
⑤将新闻组件中的评价值传递给星级评价组件
在星级评价组件StarsComponent.ts文件中使用装饰器将新闻组件中的评价值传递过来，即使用下列两行代码：
@Input()
public rating =0;
此处需要使用以下语句导入Input包：
import {Component, Input, OnInit} from '@angular/core';
然后在新闻组件的模板news.component.html文件中通过属性绑定的方法将news.rating值传递给星级评价组件，代码如下：
<app-stars [rating]="news.rating"  ></app-stars>
最后在星级评价组件StarsComponent.html文件中将评价值显示出来，代码如下：
<span >{{rating}}星</span>
⑥根据商品的星级值决定空心星星或实心星星
在星级评价组件StarsComponent.ts文件中增加下列代码的第8-11行，第8行初始化stars空数组元素，第9-11行通过循环来控制每条新闻显示的星为空心还是实心星。例如第一个新闻的rating若为2.5时，该新闻对应的stars数组值为[false,false,true,true,true]，那么对应的星显示效果为 。
export class StarsComponent implements OnInit {
    @Input()
    public rating:number=0;
    public stars: boolean[];
    constructor() {
    }
    ngOnInit() {
        this.stars=[]
        for(let i=1;i<=5;i++){
            this.stars.push(i>this.rating);
        }
    }
}

