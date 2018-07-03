Angular开发环境需要安装三种软件：

（1）Node.js

Node就是JavaScript语言在服务器端的运行环境（类似于Java的JVM），可以用来构建Web服务应用、操作文件、操作网络。

（2）Angular CLI

创建Angular项目的脚手架

（3）WebStorm

1．下载并安装Node.js

打开Node.js的官方网站：https://nodejs.org/en/。网站会根据你的操作系统弹出一些相应的Node.js版本，可以任意选择一个版本（建议选择Current）。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image002.jpg)![](/assets/1.JPG)

图1选择Node.js的版本

下载完后会得到一个.msi格式的文件\(如node-v8.7.0-x64.msi\)，双击它就可安装了，如图2所示，然后直接根据提示安装即可。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image004.jpg)![](/assets/2.JPG)

图2 Node.js安装界面

测试软件安装后的效果：

在“命令提示符”界面中，输入命令：npm -v

可以输出所安装的Node.js的版本，如图3所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image006.jpg)![](/assets/3.JPG)

图3测试Node.js的安装版本

2．安装Angular CLI

在“命令提示符”界面中输入命令：npm install -g @angular/cli

此时，系统会自动下载并安装Angular CLI

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image008.jpg)![](/assets/4.JPG)

图4安装Angular CLI

测试软件：

在“命令提示符”界面中输入命令：ng -v

会显示出Angular CLI的版本如图5所示。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image010.jpg)![](/assets/5.JPG)

图5测试Angular CLI版本

如果安装不成功，也可以使用国内的镜像网站：https://npm.taobao.org/

3．下载并安装WebStorm

进入官方网站https://www.jetbrains.com/webstorm/，如图6所示。点击网站右上角的“Download”按钮。进入下载界面，然后根据你的操作系统选择相应的软件版本。下载完成后，会得到一个.exe文件（如WebStorm-2017.2.5.exe）。直接双击安装即可（如图7~图9所示）。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image012.jpg)![](/assets/6.JPG)

图6 WebStorm官方网站

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image014.jpg)![](/assets/7.JPG)

图7安装目标位置选择

![](/assets/8.JPG)

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image016.jpg)

图8安装选项设置



![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image018.jpg)![](/assets/9.JPG)

图9完成安装界面



4．激活WebStorm

当第一次启动WebStorm时会提示激活WebStorm对话框，如图10所示，也可以跳过此步，可以获得30天的试用期。当然也可以打开集成开发环境界面后使用以下步骤实现激活。

（1）打开webstorm软件后，单击菜单栏中的“help”。

（2）单击之后，会有列出Help的下拉菜单，选择其中的“register”（注册）选项。

（3）同样弹出如图10所示的对话框，并有提示免费体验的剩余天数，其中有三种激活方法：即购买软件之后的账户、激活码和特许站点。

（4）选择第三种方法（特许站点），然后在输入框中输入相应的网站地址（笔者使用的http://idea.ibdyr.com或http://idea.codebeta.cn网址，根据实际情况可以百度查询）。然后点击active，进行激活。

![](file:///C:\Users\angular\AppData\Local\Temp\msohtmlclip1\01\clip_image020.jpg)![](/assets/10.JPG)

图10激活WebStorm

