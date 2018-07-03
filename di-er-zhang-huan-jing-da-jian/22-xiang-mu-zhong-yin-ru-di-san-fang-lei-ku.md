以jquery为例，介绍将第三方类库安装到本地步骤：

（1）单击开始→所有程序→Node.js→Node.js command prompt，进入Node.js命令行；

（2）把当前目录改为项目保存位置（本例中是d:\angular\_web\newsreport），然后输入npm install jquery –save，此时从网上下载jquery第三方类库包（输入npm install bootstrap –save，则可以安装bootstrap包）；如图2所示。下载完后的第三方类库包保存在开发环境的node\_modules目录下，并且在package.json文件中会增加相应的配置命令。

![](/assets/22.JPG)

图2安装第三方类库

l在项目中引入库文件——修改.angular-cli.json文件

打开.angular-cli.json文件，从中找到scripts，添加如下代码：

"styles": \[

"styles.css",

"../node\_modules/bootstrap/dist/css/bootstrap.css"

\],

"scripts": \[

"../node\_modules/jquery/dist/jquery.js",

"../node\_modules/bootstrap/dist/js/bootstrap.js"

\],

l为了在WebStorm开发环境中使用jquery、bootstrap的一些命令，就需要继续安装它们的类型描述文件，这样就可以在typescript中调用jquery、bootstrap里面的相关内容。其安装步骤如下：

（1）单击开始→所有程序→Node.js→Node.js command prompt，进入Node.js命令行；

（2）把当前目录改为项目保存位置（本例中是d:\angular\_web\newsreport），然后输入npm install @types/jquery --save-dev，安装jquery的类型描述文件；输入npm install @types/bootstrap -- save-dev，安装bootstrap的类型描述文件。其运行效果如图3所示。

![](/assets/23.JPG)

图3安装类型描述文件

