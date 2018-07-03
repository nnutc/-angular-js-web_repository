![](/assets/21.JPG)

图1 Angular项目结构

1．e2e：端到端（End-to-End）的测试目录，包含基本的测试桩。

2．node\_modules：Node.js创建了这个文件夹，并且把package.json中列出的所有第三方模块都存放在该目录中；

3．src：应用源代码目录，开发者写的代码均存储在此目录中，其包含的目录和文件主要有如下几类：

lapp：包含了应用程序的所有组件和模块；一个angular至少包含一个组件和一个模块；开发环境生成项目时，会自动生成下面两个文件：

（1）app.component.ts：组件，它是整个应用的基础；其默认的源代码如下：

//从angular的core\(核心\)模块中引入了一个叫Component的装饰器

import{ Component } from '@angular/core';

//4-7行用Component装饰器定义了一个组件

@Component\({

selector: 'app-root',/\*表示使用这个组件的名称 \*/

templateUrl: './app.component.html',

styleUrls: \['./app.component.css'\]

}\)

//10-12行定义了组件的控制器及数据

export classAppComponent {

title= 'app';

}

selector：CSS选择器，它通知Angular在父级HTML中查找&lt;app-root&gt;标签，创建并插入该组件。例如，如果应用的HTML包含&lt;app-root&gt;&lt;/app-root&gt;，Angular就会把AppComponent的一个实例插入到这个标签中。

templateUrl：组件HTML模板的模块相对地址，本例中为“./app.component.html”。

styleUrls：组件使用的样式文件地址，本例中为“./app.component.css”。

（2）app.module.ts：定义AppModule，Angular模块（无论是根模块还是特性模块）都是一个带有@NgModule装饰器的类；其默认的源代码如下：

/\*引入组件\*/

import { BrowserModule } from '@angular/platform-browser';/\*浏览器解析模块\*/

import { NgModule } from '@angular/core'; /\*核心模块\*/

import { AppComponent } from './app.component'; /\*自定义的模块\*/

/\*@NgModule装饰器将AppModule标记为Angular模块类，也叫NgModule类。@NgModule 接受一个元数据对象，告诉Angular如何编译和启动应用。/

@NgModule\({

declarations: \[/\*引入当前项目运行的组件，自定义组件需要引入并且在这里面配置\*/

AppComponent

\],

imports: \[/\*引入当前项目运行依赖的其他模块\*/

BrowserModule

\],

providers: \[\],/\*定义的服务，此处为空，以后如果有服务可以添加于此\*/

bootstrap: \[AppComponent\] /\*默认启动哪个组件，也就是指定应用的主视图或称为根组件，通过引导根AppModule来启动应用，这里一般写的是根组件\*/

}\)

export class AppModule { }/\*根模块不需要导出任何东西，因为其他组件不需要导入根模块，但一定要写\*/

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

